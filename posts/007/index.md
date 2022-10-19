<!--
.. title: 007 - WikiCREMMA
.. slug: 007
.. date: 2022-10-19 14:49:00 UTC-04:00
.. tags: wikicremma, experiment
.. category: dataset
.. link: 
.. description: 
.. type: text
-->

"WikiCREMMA" (or CREMMA-Wiki, or CREMMA-Wikipedia) is (are?) the name(s) Thibault Clérice and I gave to a dataset created within the perimeter of the CREMMA funding. In 2022, we were able to use this funding to hire several students from the [École nationale des chartes](https://www.chartes.psl.eu/) to produce entirely new transcriptions of documents from various periods (from Medieval times to 21st century), or to align already existing transcriptions with the corresponding images. We will hopefully have occasions to present this experiment further in the months to come, but today I would like to talk about the specific case of WikiCREMMA.  

Unlike the other CREMMA datasets[^They can all be found via the [HTR-United catalog!](https://htr-united.github.io/catalog.html)], WikiCREMMA also consisted in creating new images. Using exerpts from [randomly selected](https://github.com/PonteIneptique/wikicremma) Wikipedia articles (in French), we created forms which were then printed before we asked volunteers to copy the text by hand using the tools of their choice, thus collecting examples of nowadays handwritings.  

The form mostly consists in:

- explanations regarding the context of the experiment,
- instructions on how to fill the form,
- a short section where contributors can add their names, their writing hand (left or right) and their gender,
- the excerpt to copy,
- and a blank space where contributors ought to write.

Each form offer a different excerpt to copy: it is thus possible for volunteers to contribute many forms. The excerpts were collected by sending requests to the "[Page au hasard](https://fr.wikipedia.org/wiki/Sp%C3%A9cial:Page_au_hasard)" feature available in Wikipedia (in English, see "[random article](https://en.wikipedia.org/wiki/Special:Random)").  

Once the forms were filled, we automatically [anonymized them](https://github.com/alix-tz/cremmawiki-anonymizer) and then uploaded them on eScriptorium. This anonymization mostly meant adding a big black rectangle over the part of the page where contributors are invited to give us details about who they are. Once in eScriptorium, the images are segmented (aka lines of text are detected on the image) and then transcribed. We ignore all the printed text and only focus on the handwritten elements.  

Since we asked volunteers to copy a text, it is impossible to avoid crossed out words, blanks or mispellings. Therefore, even if we possess the original text, we did not automatize the transcription. This manual transcription takes time but it allows us to respect what was actually written by a volunteer. On the other hand, having access to the text they were asked to copy allows us to read them very quickly and to lift any doubt regarding what we should be reading!

We created several batches of forms in order to test out the success (or failure) of our project. They don't always have the same size: Batch-01 contains 10 images whereas Batch-04 has 96 of them. Initially, we imagined leaving stacks of these forms in different spots, giving volunters the possibility to send us their form after they filled it. However, even if we dream big, it seemed safer to start with asking people around us. I've asked friends, colleagues, classmates, but also very random people to participate in this experiment -- maybe you, reader, also contributed! Overall, as far as I am concerned, this has been a lot of fun! 

As of Fall 2022, more than 135 people took part in the experiment and we collected more than 250 pages. Not everything was transcribed yet, but we were already able to publish 215 files in the [CREMMA-WIKIPEDIA](https://github.com/HTR-United/cremma-wikipedia) repository. It adds up to a total of 1~181 lines and 57~490 characters for contemporary handwritings in French. 

On top of the images and the transcription, I proposed to add a series of metadata. They would allow potential users of the dataset to sort the files according to several criterias. They include the writing hand, the color of the ink or else the type of tool used to write (pencil / marker / ink pen / etc).

We were already able to use part of this dataset since it was included in the train set of [Manu McFrench](https://zenodo.org/record/6657809#.Y1BEtEyZOuU), an HTR model for modern and contemporary French!

I have lots of ideas of future usages and developments for this dataset so it will be back in other posts of this blog!
