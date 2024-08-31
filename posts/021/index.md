<!--
.. title: 021 - McCATMuS #4 - Cleaning data, collection metadata
.. slug: 021
.. date: 2024-08-30
.. tags: CATMuS, HTR, datasets
.. category: dataset
.. link: 
.. status:
.. description: 
.. type: text
-->

Preparing the data for CATMuS would certainly have taken much more time had I not been able to benefit from Thibault Clérice's experience with CATMuS Medieval. Not only was I able to build on the workflow he set up when he built it, but I also relied heavily on his scripts to parse and build the final dataset into [PARQUET](https://parquet.apache.org/) files that were pushed to HuggingFace. Most of these steps are described in [Thibault Clérice, Ariane Pinche, Malamatenia Vlachou-Efstathiou, Alix Chagué, Jean-Baptiste Camps, et al.. CATMuS Medieval: A multilingual large-scale cross-century dataset in Latin script for handwritten text recognition and beyond. 2024 International Conference on Document Analysis and Recognition (ICDAR), 2024, Athens, Greece](https://univ-paris8.hal.science/hal-04453952v1), presented at the [ICDAR](https://icdar2024.net/) conference in Athens in a few days.

For McCATMuS, I started by downloading all the datasets (keeping track of the official releases) then I manually reorganized all the datasets so that the transcription and images were always under `{dataset_repo}/data/{sub_folder}`, which made later manipulation easier. Based on the notes I took while filtering the datasets, and after generating a character table for each dataset with [Chocomufin](https://github.com/PonteIneptique/choco-mufin), I created several conversion tables to harmonize the transcription. The conversions are a mix of single character or multiple character replacements (`[` and  `[[?]]`) and more or less sophisticated replacements based on regular expressions (`#r#« `).[^chocomufin]

Here is a sample of the Chocomufin conversion table used for the LECTAUREP datasets. If the character is replaced by itself, it remains unchanged in the dataset, while replacing it allows either to remove a character from the dataset (the `¥`) or to harmonize its transcription with the CATMuS guidelines (see `œ` and `°` for example).

```
char,name,replacement,codepoint,mufidecode,order
#r#« ,Repl extra space before LEFT-POINTING DOUBLE ANGLE QUOTATION MARK,"""",00AB,,0
#r# »,Repl extra space before RIGHT-POINTING DOUBLE ANGLE QUOTATION MARK,"""",00BB,,0
[[?]],replace [[?]] with ⟦⟧,⟦⟧,,,0
[?],replace [?] with ⟦⟧,⟦⟧,,,0
),RIGHT PARENTHESIS,),0029,),
m,LATIN SMALL LETTER M,m,006D,m,
É,LATIN CAPITAL LETTER E WITH ACUTE,É,00C9,E,
a,LATIN SMALL LETTER A,a,0061,a,
",",COMMA,",",002C,",",
e,LATIN SMALL LETTER E,e,0065,e,
^,CIRCUMFLEX ACCENT,^,005E,^,
œ,LATIN SMALL LIGATURE OE,oe,0153,oe,
̂,COMBINING CIRCUMFLEX ACCENT,̂,0302,,
W,LATIN CAPITAL LETTER W,W,0057,W,
°,DEGREE SIGN,^o,00B0,*,
¥,YEN SIGN,,00A5,,
½,VULGAR FRACTION ONE HALF,1/2,00BD,0.5,
h,LATIN SMALL LETTER H,h,0068,h,
r,LATIN SMALL LETTER R,r,0072,r,
æ,LATIN SMALL LETTER AE,ae,00E6,ae,
ȼ,LATIN SMALL LETTER C WITH STROKE,c,023C,c,
∟,RIGHT ANGLE,,221F,[UNKNOWN],
```

It wasn't possible to use a single conversion table for all the datasets because some had different transcription approaches. While replacing  `¬` with `-` could, in principle, be used for each dataset, normalizing the way corrections and uncertainties were transcribed was another story. For example, in some of the CREMMA datasets, `><` is used to signal a crossed word, while in other datasets `<>` is used. So replacing `>` with `⟦` and `<` with `⟧` in `>hello<` meant that in some cases we would successfully get `⟦hello⟧`, while in other cases we would end up with `⟧hello⟦`. There are a few documents where I had to manually intervene in the XML file to fix the transcription. In such cases, I fork the dataset repository to keep track of the corrected version of the ground truth or I push the correction back into the original dataset to create a new, more consistent version.

In general, the converted dataset is saved as `.catmus.xml` files, which allows us to keep track of the original ground truth and easily adjust the conversion table later if necessary afterwards.

In the [second post](../19/) of this series, I mentioned that "*the CATMuS guidelines can (should?) be used as a reference point*" and that "*if a project decides to use a special character to mark the end of each paragraph, then in order to create a CATMuS-compatible version of the dataset, I should only have to replace or remove that character. In such cases, the special character that was chosen should be unambiguous and the rule should be explicitly presented*." Providing a Chocomufin conversion table along with a dataset that uses project-specific guidelines would be an excellent practice to ensure that the dataset is indeed compatible with CATMuS.

Once all the `.catmus.xml` files were ready, I created a new metadata table for McCATMuS listing all the subdirectories under each dataset's "data" folder. This table was used as a basis to start collecting additional metadata at the document level rather than at dataset level, like the language used in the source or the type of writing (printed, handwritten or typewritten). Working at the document level is important because some dataset contain different types of writing and/or are multilingual. In some cases, when a document would mix different languages and/or different types of writing in the source, if the distinction could be made at the image level, I manually sorted them and created two different subfolders. This is what I did in the "Memorials for Jane Lathrop Stanford" dataset, for example: the subfolder "PageX-LettreX" mixed typewritten and handwritten letters, so I sorted them into "PageX-LettreX-handwritten" and "PageX-LettreX-typewritten" in order to have the most accurate metadata possible.

Other metadata included the assignment of a call number (or shelf mark) for each source represented in the datasets. In some cases a call number may apply to multiple subfolders, but in most cases, each subfolder is de facto a different document. Retrieving the call number is useful for several reasons: it allows for an accurate assessment of the diversity of documents in McCATMuS, it allows for a document to be associated with additional metadata found in its institution's catalog, or the list of call numbers can be used during benchmarking or production to check whether a document is known to the models trained on that dataset, thus explaining potentially higher accuracy scores.

In the few cases where the source used to build the ground truth did not have a corresponding call number, I simply made one up, keeping "nobs_" as a signal that it was a made-up call number. Thus, if "cph_paris_tissage_1858/" in "timeuscorpus" is now associated with its corresponding call number at the Paris archive center (Paris, AD75, D1U10 386), CREMMAWiki's "batch-04", which is composed of documents we created for the project, is associated with a made-up call number: "nobs_cremma-wikipedia_b04".

In the end, when the PARQUET files are created, the metadata from the table I just presented is collected, along with information extracted from parsing the contents of the XML file. Each of the metadata is then represented at the text line level. If you compare [McCATMuS](https://huggingface.co/datasets/CATMuS/modern) with [CATMuS Medieval](https://huggingface.co/datasets/CATMuS/medieval) using HuggingFace's dataset viewer, you can see that they don't use exactly the same metadata.

"Language", "region type" and "line type" (which are based on the segmOnto classification), "project" and "gen_split" are common to both datasets, along with "shelfmark" I just described above. They both have a "genre" column with similar values (treatise, epistolary, document of practice, etc.). In the case of CATMuS Medieval, "genre" is complemented by "verse" (prose, verse).

Following Thibault's advice, I defined the creation date of a text line using two numbers ("not_before" and "not_after") instead of a single "century" value. This allows for a precise dating when it is possible or on the contrary, to spread the dating over several centuries when it cannot be avoided, which is more accurate in both cases.

McCATMuS mixes printed, handwritten and typewritten documents, so it was important to have a "writing type" column to help filter the dataset based on this information, in cases where one does not want to mix them. This metadata also makes it possible to use McCATMuS to train a classifier capable of distinguishing between the different types of writing. CATMuS Medieval on the other hand presents only handwritten sources, so such a metadata would be useless and is able to rely on paleographic classifications to characterize each text line based on a "script type" metadata, that includes values such as "caroline", "textualis", "hybrida", etc.

McCATMuS also has a "color" column that helps sort text lines based on whether the source image is colored (true) or in grayscale (false).

Although I reused the scripts developed by Thibault to build this dataset, I had to make several modifications to include this new metadata in the PARQUET files and to add additional filtering to the text lines. This included updating the mapping to the segmOnto vocabulary to match what existed in my datasets, or filtering some types of lines such as those identified as signatures.[^signatures] I also included an update of "writing_type" at the line level whenever the value in "line_type" allowed it to be controlled. 

```python
if ":handwritten" in line_type:
    writing_type = "handwritten"
    line_type = line_type.replace(":handwritten", "")
elif ":print" in line_type:
    writing_type = "printed"
    line_type = line_type.replace(":print", "")
elif ":typewritten" in line_type:
    writing_type = "typewritten"
    line_type = line_type.replace(":typewritten", "")
else:
    writing_type = metadata["writing_type"]
```

In the end, having built such a dataset (the first version of McCATMuS contains 117 text lines!) with such a variety of metadata is very satisfying although there is room for improvement. I have already mentioned that it would be interesting to have a greater variety of languages in McCATMuS. I also know that some of the values in "writing_type" are not completely accurate so adding a control based on a classifier might be interesting. Finally, I've noticed that some transcriptions in the "FoNDUE_Wolfflin_Fotosammlung" dataset are not correct at all, probably due to an automatic transcription that wasn't corrected.

However, before we dive into improving McCATMuS, it's important to first examine the accuracy of the models that can be built on top of it! This will be the topic of the next and last post in this series!


[^chocomufin]: To learn more about how [`chocomufin convert`](https://github.com/PonteIneptique/choco-mufin?tab=readme-ov-file#commands) works, just read the software's short documentation.

[^signatures]: I don't think it makes sense to include signatures in a dataset to train a generic model, since the transcription of such lines can be very context specific.  
