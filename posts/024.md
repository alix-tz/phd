<!--
.. title: 024 - The messy backstage of a literature review
.. slug: 024
.. date: 2025-06-21 15:15:27 UTC-04:00
.. tags: survey, HTR, OCR, literature review
.. category: tools & methods
.. link: 
.. description: 
.. type: text
-->

A few weeks ago, I began a thorough review of articles published in four digital humanities venues to track mentions of automatic text recognition and understand how, where, and why scholars use it. Although I wish I had started sooner in my doctoral journey, I stay positive holding on to the idea that "it's never too late." I learn a lot about Digital Humanities as a field of research and gain a better understanding of ATR's presence in the field.

While catching up on our dissertation progress, I was telling Roch Delanney about the survey I'm conducting, my goals for it, and how I selected and sorted the articles. Roch suggested that I share my method more widely. It seems a little clumsy at times, but I am also able to use many different skills I have learned and sharpened over the years so I think it is indeed interesting to share a bit of my *cuisine*.

## Perimeter of the literature review

My literature review focuses on four publication venues. I think they are, collectively, representative of research in the Digital Humanities: 

- [*Digital Scholarship in the Humanities*](https://academic.oup.com/dsh) (DSH), which is presented by the Alliance of Digital Humanities Organizations (ADHO) as an international, peer-reviewed journal published by Oxford University Press on behalf of ADHO and the European Association for Digital Humanities (EADH). It was published under the title *Literary and Linguistic Computing: The Journal of Digital Scholarship in the Humanities* until 2014. I counted a total of 174 volumes for a total of 1741 articles (excluding retracted articles, book reviews, editorials and committee reports) published since 1985 until the first half of 2025.

- [*Digital Humanities Quarterly*](https://dhq.digitalhumanities.org/) (DHQ) is an open-access peer-reviewed journal, probably more representative of research in North America. It is published by the Association for Computers and the Humanities (ACH). I counted a total of 790 articles published since its first issue in 2007. Most articles are in English.

- The [*Journal of Data Mining and Digital Humanities*](https://jdmdh.episciences.org/) (JDMDH), is published by Episciences since 2017. Contrary to DHQ, its focus is more European-centric, and it has a special volume dedicated specifically to automatic text recognition (directed by Ariane Pinche and Peter Stokes). I found a total of 162 articles published in JDMDH, including the special volume on ATR.

- Lastly, the proceedings from the more recent *Computational Humanities Research* (CHR) conferences (see the [2024 conference proceedings](https://2024.computational-humanities-research.org/) for example) offer a perspective on research focused on more intensively computational methods in the Humanities. The conference is held annually since 2021. I found a total of 214 articles in the proceedings.

Aside from DSH, that I can access thanks to the library of the University of Montréal, all the other journals are in open access. 

## Collecting the articles and their metadata

For JDMDH, articles are not centralized on the journal website but rather published on platforms like [HAL](https://hal.archives-ouvertes.fr/) or [arXiv](https://arxiv.org/) and sometimes [Zenodo](https://zenodo.org/). Getting an overview of the articles published in JDMDH is not straightforward, but it is possible to browse the articles per [volumes](https://jdmdh.episciences.org/browse/volumes). I opened and downloaded each article in each volume, as well as collected the article entries in Zotero using the Zotero connector. The process was cumbersome and required many clicks, but the variety of publishing platforms deterred me from writing a script to automate the downloading process.  

CHR, on the other hand, was very easy to scrape, partly because there are only four volumes of proceedings so far. For each series of proceeding, the index of all articles is compatible with the batch import scenario of the Zotero connector. To collect the PDFs, I used a section of the HTML page and regular expressions to identify the links to the PDF files, creating a list of URLs. Finally, I used a Python script to download the PDFs to my computer.  

For example, in [https://ceur-ws.org/Vol-2989/](https://ceur-ws.org/Vol-2989/), the `ul` contains simple HTML elements pointing to the PDF files, such as: 

```html
<h3><span class="CEURSESSION">Presented papers</span></h3>

<ul>
  <li id="long_paper5"><a href="long_paper5.pdf">
      <span class="CEURTITLE">Entity Matching in Digital Humanities Knowledge
      Graphs</span></a>
    <span class="CEURPAGES">1-15</span> <br>
    <span class="CEURAUTHOR">Juriaan Baas</span>,
    <span class="CEURAUTHOR">Mehdi M. Dastani</span>,
    <span class="CEURAUTHOR">Ad J. Feelders</span>
  </li>
...
```

All I had to do was copy and paste this entire list into a text editor (I like to use [Sublime Text](https://www.sublimetext.com/) in such a situation). Then, I used a simple regular expression like `href=".+?"` to select the value in the `a` element, which contains the links to the PDF files. I kept only the selected text and then rebuilt the complete URL with a couple of replacements such as `href="` -> `"https://ceur-ws.org/Vol-2989/` and `"\n` -> `",\n`. At this point I just added square brackets around the selection, et voilà! I had a Python list ready to be passed to a script like the one below to download the files:

```python
list_of_urls = ["https://ceur-ws.org/Vol-2723/short8.pdf",
                "https://ceur-ws.org/Vol-2723/long35.pdf",
                "https://ceur-ws.org/Vol-2723/long44.pdf",
                #...
                ]

import requests
import os
from tqdm import tqdm # it makes  progress bar so I know how long I can take to make a tea while the script runs

for url in tqdm(list_of_urls):
    r = requests.get(url)
    if r.status_code == 200:
        filename = f"{url.split("/")[-2]}-{url.split("/")[-1]}"
        #print(filename)
        with open(filename, "wb") as f:
            f.write(r.content)
    else:
        print(f"Failed to download: {url}")
    time.sleep(1)  # This cool down is to be polite to the server
```

I used a similar approach for downloading the articles from DHQ because the [Index of Titles](https://dhq.digitalhumanities.org/index/title.html) lists all of the published articles on a single page. I first downloaded the HTML pages of the articles (DHQ publishes articles in HTML format as well as PDF). I also used regular expressions to extract the list of links and used a Python script to download the files.  

Unfortunately, the Zotero connector only works on each article page individually, but not for batch-import on the index page. I investigated a bit to understand why it was so, and found that in the source code of each article page, there is a `span` element identified with the class `Z3988` that the Zotero connector uses to extract the metadata and create an entry in Zotero. In DHQ, these spans look like this:

```html
<span class="Z3988" title="url_ver=Z39.88-2004&amp;ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rfr_id=info%3Asid%2Fzotero.org%3A2&amp;rft.genre=article&amp;rft.atitle=Academics%20Retire%20and%20Servers%20Die%3A%20Adventures%20in%20the%20Hosting%20and%20Storage%20of%20Digital%20Humanities%20Projects&amp;rft.jtitle=Digital%20Humanities%20Quarterly&amp;rft.stitle=DHQ&amp;rft.issn=1938-4122&amp;rft.date=2023-05-26&amp;rft.volume=017&amp;rft.issue=1&amp;rft.aulast=Cummings&amp;rft.aufirst=James&amp;rft.au=James%20Cummings"> </span>
```

I understood recently, while discussing with Margot Mellet, that Z3988 is a reference to the [OpenURL Framework Standard (ISO Z 39.88-2004)](https://groups.niso.org/higherlogic/ws/public/download/14833/z39_88_2004_r2010.pdf), which is used by the Zotero connector. Also, I should note that such spans are not systematically used in online journals. JDMDH for example doesn't use them, and serves the metadata in a different way.  

Since I had already downloaded all the articles from DHQ as HTML files, I wrote a simple Python script that found all of such spans for each downloaded article and aggregated them in a single, very simple HTML file. Then, I simply opened this page in my browser after emulating a local server[^python_server] (with a command like `python -m http.server`), and I was able to use the Zotero connector to import all the articles in a single click. It was very satisfying! The only downside is that I couldn't collect the articles' abstracts because there weren't included in the spans.  

DSH was different from the rest of the journals. Because of the longevity of the journal and the amount of articles it published, it was quite overwhelming. Unfortunately, it is a paywalled journal and I couldn't figure out how to make the proxy of the University of Montreal library work with my Python scripts and the command line. As a result, I had to manually download the articles,[^proxy_dsh] but only when they were relevant! Since DSH has a fairly good search engine that allows to do multi-keyword searches, I only downloaded articles matching my search criteria (143 in total).

Additionally, I went through each of the 174 issues of DSH to batch-import the article references in Zotero. It was tedious but I figured I might be able to use these metadata for other projects in the future.  

## Filtering the articles

For DHQ, JDMDH and CHR, I ran a keyword surch using the command [`grep`](https://www.man7.org/linux/man-pages/man1/grep.1.html) on the content of the articles. I didn't want to limit my search to the titles, abstract or keywords because I really wanted to include anecdotal mentions of automatic text recognition in my results.  

To use grep, I created a file (pattern.txt) with the keywords I was looking for:  

```txt
HTR
OCR
text recognition
ATR
Transkribus
eScriptorium
automatic transcription
```

Then I converted the PDFs into text files using the command [pdftotext](https://man.archlinux.org/man/pdftotext.1.en). This was necessary because grep cannot search inside a PDF directly. I didn't need to do this conversion for DHQ, since I had download HTML files from that journal. 

The commands to search inside the PDFs of one of the journals would look like this:

```bash
ls *.pdf | xargs -n1 pdftotext # to convert PDFs to text files
grep -i -w -m5 -H -f ../pattern.txt *.txt # to search for the keywords in the text files and display the first 5 matches
```

After controlling how grep matched the keywords, I used `grep -l -f ../pattern.txt *.txt` to list the files that matched the keywords. This list was used to sort the documents into two folders, according to whether or not they matched my research.

In the case of DSH, I directly used the search engine to combine the keywords, using the "OR" operator. I set the full text of the articles as the scope of my research: [https://academic.oup.com/dsh/search-results?allJournals=1&f_ContentType=Journal+Article&fl_SiteID=5447&cqb=[{%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22automatic%20transcription%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22transkribus%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22text%20recognition%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22escriptorium%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22OCR%22}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22HTR%22}]}]&qb={%22_text_1-exact%22:%22automatic%20transcription%22,%22qOp2%22:%22OR%22,%22_text_2-exact%22:%22transkribus%22,%22qOp3%22:%22OR%22,%22_text_3-exact%22:%22text%20recognition%22,%22qOp4%22:%22OR%22,%22_text_4-exact%22:%22escriptorium%22,%22qOp5%22:%22OR%22,%22_text_5%22:%22OCR%22,%22qOp6%22:%22OR%22,%22_text_6%22:%22HTR%22}&page=1](https://academic.oup.com/dsh/search-results?allJournals=1&f_ContentType=Journal+Article&fl_SiteID=5447&cqb=[{%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22automatic%20transcription%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22transkribus%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22text%20recognition%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22escriptorium%22,%22exactMatch%22:true}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22OCR%22}]},{%22condition%22:%22OR%22,%22terms%22:[{%22filter%22:%22_text_%22,%22input%22:%22HTR%22}]}]&qb={%22_text_1-exact%22:%22automatic%20transcription%22,%22qOp2%22:%22OR%22,%22_text_2-exact%22:%22transkribus%22,%22qOp3%22:%22OR%22,%22_text_3-exact%22:%22text%20recognition%22,%22qOp4%22:%22OR%22,%22_text_4-exact%22:%22escriptorium%22,%22qOp5%22:%22OR%22,%22_text_5%22:%22OCR%22,%22qOp6%22:%22OR%22,%22_text_6%22:%22HTR%22}&page=1)

In both cases, the search was not case sensitive, in order to catch a maximum of occurrences of keywords like "automatic text recognition" or "Text Recognition" or "text recognition", etc. However, it meant that sometimes I found false positives: "democracy" often matches with "ocr", so does "theatre" with "atr". Since DSH's search engine returns the match in context, I was able to ignore these false positives. For the other journals, I had to manually check where the matches were. Usually, I combined this control with the next step of my investigation.  

### Hits per journal

- JDMDH: 47 hits (out of 162 articles)
- DHQ: 93 hits (out of 790 articles)
- DSH: 143 relevant hits (out of 1741 articles)
- CHR: 65 hits (out of 214 articles)

## *Dépouillement* and analysis

To this date, I am still in the process of reading the articles and taking notes on the occurrences of my keywords. 

I use Zotero to keep track of the articles I read and to confirm whether they are false positives. Sometimes, I leave out articles that are irrelevant, even if they mention a keyword I was looking for. For example, [Liu & Zhu (2023)](https://doi.org/10.1093/llc/fqac089)[^liu_zhu_2023] contains the string "OCR" but it only appears in a title in their bibliography, for work they refer to in a context where OCR is not relevant to their argument. With tags in Zotero, I clearly identify such articles as "to be left out" from my analysis, but I don't remove them from the collection.  

I use different tags to identify the various occurrences of the technology in the articles. For example, I distinguish between firsthand applications of ATR and the reuse of data produced by ATR before the experimentation presented by the authors. Typically, there are many mentions of documents that were OCRed by libraries and used by scholars to conduct their research. Overall, with this analysis, I am trying to add more depth to the observations made by [Tarride et al (2023)](https://doi.org/10.48550/arXiv.2304.13530)[^tarride_et_al_2023] in which they pragmatically considered three situations leading to the use of ATR: 1) for the production of digital editions; 2) for the production of large searchable text corpora; and 3) for the production of non-comprehensive transcriptions to feed knowledge bases. However, it is difficult to elaborate definitive categories before I am done processing all the collected articles.  

Due to the large number of articles to be analyzed, I have continued to use the grep command to quickly review the content of articles and speed up my sorting process. For example, I am more interested in firsthand usages of ATR, want to be able to quickly identify non relevant mentions of my keywords as was the case in Liu & Zhu (2023). The command `grep -i -w -C 5 -H -f ../pattern.txt *.txt > grep_out` allows me to generate a file, grep_out, in which, for each time a keyword is matched in a document, five lines of context are displayed before and after the match, as well as the name of the file. I still have to read the abstracts and parts of the articles to clearly understand in which contexts the automatic text recognition technologies are used. However, this is an effective method for quickly sorting through the articles.

I'm looking forward to sharing the results of this analysis in my dissertation! 


<!-- FOOTNOTES --->

[^python_server]: This emulation is necessary to allow the Zotero connector to work properly.

[^proxy_dsh]: I want to specify here that it was not by lack of reading documentations on proxies and requests. Unable to find a straightforward solution, unsure if it was even something that the UdeM proxy allowed, and because I would have still needed to write additional scripts afterwards, I decided that it would take just as long to do it manually (about 2-3 hours).

[^liu_zhu_2023]: Liu, Lei, and Min Zhu. "Bertalign: Improved Word Embedding-Based Sentence Alignment for Chinese–English Parallel Corpora of Literary Texts." *Digital Scholarship in the Humanities* 38, no. 2 (June 1, 2023): 621–34. [https://doi.org/10.1093/llc/fqac089](https://doi.org/10.1093/llc/fqac089).

[^tarride_et_al_2023]: Tarride, Solène, Mélodie Boillet, and Christopher Kermorvant. "Key-Value Information Extraction from Full Handwritten Pages." arXiv, April 26, 2023. [https://doi.org/10.48550/arXiv.2304.13530](https://doi.org/10.48550/arXiv.2304.13530).

