<!--
.. title: 020 - The CATMuS Modern dataset #3
.. slug: 020
.. date: 2024-08-29
.. tags: CATMuS, HTR, datasets
.. category: dataset
.. link: 
.. status:
.. description: 
.. type: text
-->

HTR-United made identifying candidate datasets for McCATMuS a piece of cake. Once it was agreed with the rest of the CATMuS community  agreed on the period to be covered by a "modern and contemporary" dataset, I created a simple script to parse the content of the HTR-United catalog and make a list of existing datasets covering documents written in Latin alphabet and matching our time criteria. 

Actually, here is the script!

```python
url_latest_htrunited="https://raw.githubusercontent.com/HTR-United/htr-united/master/htr-united.yml"
     
import requests
import yaml

import pandas as pd
     
# get latest htr-united.yml from main repository
response = requests.get(url_latest_htrunited)
catalog = yaml.safe_load(response.content)

def in_time_scope(dates):
    century_scope_min = 1600
    century_scope_max = 2100
    # this means that we allow datasets that intersect with the period
    if int(dates.get("notBefore")) < century_scope_min and int(dates.get("notAfter")) < century_scope_min:
        return False
    elif int(dates.get("notBefore")) > century_scope_max and int(dates.get("notAfter")) > century_scope_max:
        return False
    return True

filtered_by_date = []
for entry in catalog:
    if in_time_scope(entry.get("time", {})):
        filtered_by_date.append(entry)
print(f"Found {len(filtered_by_date)} entries matching the time scope.")

targeted_script = "Latn"
filtered_by_script = []
for entry in filtered_by_date:
    if targeted_script in [s.get("iso") for s in entry.get("script")]:
        filtered_by_script.append(entry)
print(f"Found {len(filtered_by_script)} entries matching the script criteria.")

cols = ["Script Type", "Time Span", "Languages", "Repository", "Project Name", "Dataset Name"]

metadata_df = pd.DataFrame(columns=cols)

selected_entries = filtered_by_script
for entry in selected_entries:
    row = {k:"" for k in cols}
    languages = [l for l in entry.get("language")]
    if len(languages) == 1:
        row["Languages"] = languages[0]
    elif len(languages) > 1:
        row["Languages"] = ", ".join(languages)
    else:
        print("Couldn't find a field for language in this repository")
        row["Languages"] = "no language"
    # get centuries/y
    # ideally we would automatically build the value, but I don't want to deal with that right now and it might cause undetected bugs
    row["Time Span"] = f'{entry.get("time").get("notBefore")}-{entry.get("time").get("notAfter")}'
    row["Project Name"] = entry.get("project-name", "no project name")
    repository = entry.get("url", "no url found")
    if repository.startswith("https://github.com/"):
        row["Repository"] = repository.split("https://github.com/")[-1]
    elif repository.startswith("https://zenodo.org/"):
        row["Repository"] = repository.replace("https://zenodo.org/", "zenodo:")
    else:
        row["Repository"] = repository
    row["Dataset Name"] = entry.get("title", "no title found")
    script_type = entry.get("script-type")
    if script_type == "only-typed":
        row["Script Type"] = "Print"
    elif script_type == "only-manuscript":
        row["Script Type"] = "Handwritten"
    else:
        row["Script Type"] = "Mixed"
    metadata_df.loc[len(metadata_df)] = row
     
metadata_df
```

I saved the output as a CSV and proceeded to go through each of the selected datasets and its metadata. I checked several things, as follows:

- I made sure the datasets were available and easy to download. For example, I excluded those requiring manual image retrieval.
- I checked the format of the data because I decided to initially focus only on datasets available in ALTO XML and PAGE XML.
- I controlled the overall compatibility between the transcription guidelines used for the dataset and those designed by CATMuS.
- I also checked the conformity of the dataset when trying to import it into eScriptorium. This import allowed me to detect when there was a discrepancies between the names of the image files and the value for the source image in the XML file.[^images]
- Loading a sample of the dataset in eScriptorium also allowed me to visually control other incompatibilities with CATMuS that may not have been documented by the producers of the data.[^segmentation] 
- Finally, I considered the structure of the repository and, when necessary, the facility to reorganize it into a single `data/` folder containing the images and the XML files, often distributed among sub-folders.

I assigned each dataset a priority number from 1 to 6. The lowest number was for dataset compatible with CATMuS without any modification and 6 for massive datasets that would require a nerve-racking script to be built correctly. My grading system is shown below.

```
1=ready as is
2=need to be chocomufin-ed
3=require manual corrections but the dataset is very small, or the dataset is chocomufin/catmus compatible but requires a script to build it
4=require manual corrections but the dataset is relatively big, or require a script to be fixed
5=require manual corrections but the dataset is really big
6=require manual corrections but the dataset is really big and require a personalized script to be built
```

For example, ["Notaires de Paris - Bronod"](https://htr-united.github.io/share.html?uri=507bb514d) had to be modified to comply with CATMuS requirements. This included replacing `[[` with `⟦`, and also to ignore lines containing `¥`, a symbol used by LECTAUREP to transcribe signatures and paraphs. These were straightforward modifications. On the complete opposite, ["University of Denver Collections as Data - HTR Train and Validation Set JCRS_2020_5_27"](https://htr-united.github.io/share.html?uri=7a99090c5) is a massive dataset (2660 XML files), but there are segmentation errors in this dataset and the annotation of the superscripted text is not compatible with CATMuS. This makes it necessary to control and correct each page one by one.

I chose to focus on datasets with priority 2 for the *first* version of McCATMuS. Indeed, it's possible to add more datasets into CATMuS in later versions, so there was no need to spend too much time on manually cleaning datasets. I had 23 with priority 2 to go through.

Identifying eligible datasets was not as time consuming as cleaning them and collecting additional metadata turned out to be. However, it gave me a good idea of the challenges I would face when trying to aggregate the datasets. I would have liked to be able to find a greater diversity of languages, but this is mainly due to the fact that many non-French datasets require more elaborate corrections than applying Chocomufin.  I'll cover this question of data cleaning in more detail in my next post!

[^images]: It was the case in "[Données vérité de terrain HTR+ Annuaire des propriétaires et des propriétés de Paris et du département de la Seine (1898-1923)](https://htr-united.github.io/share.html?uri=c326a6fee) where the ALTO XML files are not explicitly linked to their corresponding source images. I believe it can be fixed, but it would require creating a script just for this purpose and the dataset presented other incompatibilities with CATMuS' guidelines.

[^segmentation]: For example, "[Argus des Brevets](https://htr-united.github.io/share.html?uri=43d1c93c7)" contains some segmentation errors that will need to be corrected manually. 