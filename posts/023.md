<!--
.. title: 023 - Writing a PhD manuscript with Markdown and Quarto
.. slug: 023
.. date: 2025-02-18
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

The deadline for finishing the dissertation is approaching. And there is still so much to do! This is one of the main reasons why this research blog has been quiet for the last few months, even though there are many topics I would like to write about. 

But I guess I can take a short break from time to time and go with the flow of writing a blog post in one sitting. Who knows, maybe I'll do a few more before it's time to turn in my dissertation. I want to talk about my writing setup because it is something I have thought about a lot, trying to find the best compromise. 

Writing my dissertation in Microsoft Word has never been an option, although I do use Google Docs from time to time to get quick feedback from my supervisors. 

[LaTeX](https://www.latex-project.org/) may seem like an obvious choice to some of my fellow PhD writers, but I usually limit my use of LaTeX to [Overleaf](https://www.overleaf.com), an online LaTeX editor. On the one hand, I didn't necessarily want to install LaTeX locally for the time being, and on the other hand, I couldn't imagine writing a whole dissertation using Overleaf, because working in my browser can be distracting, and because it would require that I always have access to the Internet to work. To be honest, I mostly didn't want to use LaTeX in the first place because I find the syntax too distracting when I'm writing. It's super useful for getting good control over the layout of the document for the final version of the manuscript, but it's not convenient to work with while I'm formulating ideas and arguments.

I will probably use LaTeX to prepare the final version of the manuscript, but I wanted to use something lighter to structure my document, but easily convertible to LaTeX down the road. 

And I am a big fan of Markdown.

Markdown has a syntax that is light enough not to be too distracting - I use it all the time when taking notes anyway, so it is fully part of my writing reflexes. Also, in the context of writing my dissertation, I think of Markdown as text that I can easily copy and paste into a Google document when I need feedback, without losing formatting and without compromising readability in Google Docs. I've seen some LaTeX copy-pasted into Google Docs for supervisor feedback, and I don't think it would work for me. 

In addition to Markdown, I wanted to be able to use a modular approach to building my manuscript. A modular approach means having several smaller text files that are eventually merged into a single master document. LaTeX also relies on modularity with commands like `\include{}`. Modularity is important because in a very long text document it is easy to get lost between inline comments, draft passages, and finished paragraphs. There's also the risk of accidentally deleting passages. With a modular structure, it will also be easier to move paragraphs around as I progress. Also, my manuscript is versioned with Git and synchronized with a private GitHub repository, and modularity makes versioning much easier.

Instead of programming my own manuscript builder - yes, that was my first impulse - I took a closer look at the documentation for [Quarto](https://quarto.org/), which I've been using for a little over a year to create slides and websites for the courses I teach. Quarto offered me a solution on a silver platter, because it supports building [books](https://quarto.org/docs/reference/projects/books.html) with Markdown, which is close enough to a phD thesis. 

Quarto implements a single-source publishing paradigm and acts as a shell around [pandoc](https://pandoc.org/), which allows for swift conversion from one format to another, including from Markdown to LaTeX. I can split the document into multiple smaller Markdown files, and use my book's config file to specify the order in which the Markdown files are aggregated. Quarto's Markdown implementation includes some cool stuff from pandocs, including citation and cross-reference management. It's really worth taking a look at the documentation.

So with Quarto, I can write my dissertation as a series of smaller Markdown files, and end up with a master .md file, a .tex file ready to import into Overleaf, or even an already parsed PDF file generated with [tinytex](https://quarto.org/docs/output-formats/pdf-engine.html). 

Quarto is not a text editor, it is simply a processor that starts with a set of markdown files and a config file, and then builds one or more outputs. To write, I use Visual Studio Code and have a `quarto preview` command running in the background. For now, it just produces an HTML preview that I see in my browser. When I'm closer to a stable version of the manuscript, I'll start working with PDF output.

The syntax for some of the more specific Markdown features in Quarto is more complex than I am used to, so I still have to look at the documentation from time to time. But I am getting the hang of it, and I use a cheat sheet for the features I use more often. 

Pandoc's Markdown support lets you apply classes to entire paragraphs or inline portions of text. This is useful because it has allowed me to create some CSS transformations with classes like "draft" or "missing-information" to keep track of passages I need to rewrite, or blocks where I need to get away from my text editor and go back to my notes (usually in [Zotero](https://www.zotero.org/)). I find it super useful to avoid (at least as much as possible) falling into loopholes that distract me from actually writing. It's more efficient for my time management to divide my time between actual writing sessions and other sessions where I work on improving the drafty passages or doing the research I'm missing to illustrate an argument. 

Another use of inline classes is to keep track of concepts or specific terms that I could include in a glossary or at least a list of acronyms. By keeping track of them directly in the text, I can automate the generation of these sections. Some might say that this is the kind of thing I could do with [TEI XML](https://tei-c.org/release/doc/tei-p5-doc/en/html/index.html)- I agree, since this is semantic annotation. But as I said, I wanted a lightweight syntax, and I really like Markdown.  

