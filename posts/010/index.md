<!--
.. title: 010 - Make and Read the docs
.. slug: 010
.. date: 2023-02-28 05:41:40 UTC-05:00
.. tags: house cleaning, static website
.. category: story time
.. link: 
.. description: 
.. type: text
-->

During my last contract as a research engineer at Inria, I spent a lot of my time working on the project called [LECTAUREP](https://lectaurep.hypotheses.org/), in collaboration with the [National Archives in France](https://www.archives-nationales.culture.gouv.fr/). The goal of this project was to explore new ways to index the content of thousands of thousands of notary registries which, put together, form [one of the most used collections](https://www.siv.archives-nationales.culture.gouv.fr/siv/cms/content/fonds.action?uuid=12b&template=pog/pogLevel2&preview=false) of the National Archives. I joined the project at the end of 2019, during its second phase, almost at the same time as [eScriptorium](https://gitlab.com/scripta/escriptorium) was initiated. LECTAUREP had worked with [Transkribus](https://readcoop.eu/transkribus/?sc=Transkribus) during the first phase (in 2018) but, given the connections between [my research team](http://almanach.inria.fr/index-en.html) and the team behind eScriptorium, we quickly switched to the newer software and contributed to its development.  

One of my most important contribution is the redaction of [a tutorial for the software](https://lectaurep.hypotheses.org/documentation/prendre-en-main-escriptorium), which was initially only intended as an internal resource for our team of annotators. This is the reason why the tutorial was published on LECTAUREP's blog. [OpenITI](https://openiti.org/), and in particular [Jonathan Allen](https://twitter.com/Mar_Musa) rapidly offered [an English translation](https://lectaurep.hypotheses.org/documentation/escriptorium-tutorial-en) which, eventually, was also published on LECTAUREP's blog. Since the publication of this translation, it is listed on eScriptorium's home page as its official tutorial.  

Unfortunately, the tutorial hasn't been updated in a long time whereas major updates and new features have been added on eScriptorium's side.  

LECTAUREP's blog is not a good solution. It is built with [Wordpress](https://wordpress.com/) and hosted by [Hypotheses](https://hypotheses.org/) which is very convenient to allow a small, well defined, group of people to collaboratively work on a research blog, but it's too heavy and not adapted to publish the documentation of a software like eScriptorium. The documentation needs to be updated frequently to keep up with the software and, in general, a blog is not a place to publish the extensive documentation of a software. To top it all, it is not even that easy to update for me, so can you imagine someone outside of LECTAUREP trying to offer an update?  

I have been thinking of finding a better solution since at least 2020, but it was never so urgent that I was able to put it at the top of my to-do lists. Last Summer, I took the advantage of a rather slow couple of weeks in August, when every one but me seemed to have gone on vacations, to put something different in place.  

[Readthedocs](https://readthedocs.org/) quickly appeared to me as an ideal solution: the platform is designed for publishing software documentations, it handles software versions and multi-lingual contents. Last but not least, it uses static website generators. This is fundamental because it allows for the publication of the source code on a platforms like [Github](https://github.com/) and will actually use this public source code to build the website.  

Github is a platform designed for sharing and opening codes to external contributors. Relying on it solves a major issue with the current tutorial: if anyone can suggest the correction, edition or translation of eScriptorium's documentation, then it is more likely to keep up with the evolutions of the application!  

In August, I created a new Github repository called [escriptorium-tutorial](https://github.com/alix-tz/escriptorium-tutorial). I [set a basic configuration and connected it to Readthedocs](https://docs.readthedocs.io/en/stable/tutorial/#getting-started). As soon as this was done, the website became available at [online](https://escriptorium-tutorial.readthedocs.io/) with a URL based on the following structure: `{gh_repo_name}.readthedocs.io`. Then, I started rewriting the content of the tutorial... following [Sphinx' syntax](https://docs.readthedocs.io/en/stable/intro/getting-started-with-sphinx.html).  

It was so painful that I never got back to it after I came back from my own vacations.  

Why painful? Well, I had discovered Markdown in 2017 and I have used it since. It's so powerful and yet so light! In comparison, Sphinx felt like such a complicated and heavy syntax. Not as heavy as HTML, but less intuitive nonetheless. I had to go through the documentation every time I wanted to add something as simple as a hyperlink or an image!  

In January, when I gathered enough motivation[^1] to go back to working on eScriptorium's tutorial, I decided to look for an alternative to [Sphinx compilers](https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinx.builders.html.DirectoryHTMLBuilder).  

The only non-sphinx-based option available with readthedocs is [Mkdocs](https://docs.readthedocs.io/en/stable/intro/getting-started-with-mkdocs.html). Like its name hints at, [Mkdocs](https://www.mkdocs.org/) is a Markdown compiler, capable to quickly build websites. The set-up is really quick, it's well documented, fairly easy to customize and it's possible to add a lot of [cool extensions which are based on Python](https://squidfunk.github.io/mkdocs-material/setup/extensions/python-markdown-extensions/#emoji). It was the bomb!

> I liked Mkdocs so much that I also used it to rebuild [my personal website](https://alix-tz.github.io/)![^2]

Over the past month, I have spent a lot of time working on this new tutorial for eScriptorium. I designed a basic structure, breaking down the features into different categories. Now the pages are progressively being filled and I am very happy to have been joined in my efforts by my colleagues Hugo Scheithauer and Floriane Chiffoleau. As we progressively merge the content of new pages to the main branch, the [escriptorium-tutorial](https://escriptorium-tutorial.readthedocs.io/en/latest/) website expands. It will be ready soon for an official release!  

I really hope that the transparency and simplicity brought by Mkdocs and Markdown will allow many people to add their contributions to the documentation of eScriptorium! Who knows, maybe you will too!

[^1]: Also when I got more free time after [my classes were over](https://alix-tz.github.io/phd/posts/009/)!  

[^2]: It is not necessary to use readthedocs to deploy a website built with Mkdocs. In the case of the tutorial, it simply allows us to have a domain name more meaningful than ".github.io".  
