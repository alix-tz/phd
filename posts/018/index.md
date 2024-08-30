<!--
.. title: 018 - McCATMuS #1 - Overview
.. slug: 018
.. date: 2024-08-14
.. tags: CATMuS, HTR, 
.. category: dataset 
.. status: 
.. link: 
.. description: 
.. type: text
-->

Last week, I attended [ADHO's annual conference](https://dh2024.adho.org/) in Washington DC. I presented a short paper, co-authored with Floriane Chiffoleau and Hugo Scheithauer, about the documentation we wrote for eScriptorium (I wrote [a post](./010) about it last year and you can also find our presentation [here](https://inria.hal.science/hal-04594142)). I was also a co-author on a long paper presented by Ariane Pinche on the [CATMuS Medieval dataset](https://inria.hal.science/hal-04346939).

CATMuS, which stands for "Consistent Approach to Transcribing ManuScripts", is a collective initiative and a framework to aggregate ground truth datasets using compatible [transcription guidelines](https://catmus-guidelines.github.io/) for documents from different period written in romance languages. It started with [CATMuS Medieval](https://huggingface.co/datasets/CATMuS/medieval), but since January this year, I have been working on a version of CATMuS for the modern and contemporary period. 

While I should (and will) try to publish a data paper on CATMuS Modern & Contemporary (I'll call it McCatmus from now on), I figured I could start with a series of blog posts here. I want to describe the various steps I followed in order to eventually release [a dataset on HuggingFace](https://huggingface.co/datasets/CATMuS/modern) and hopefully soon the corresponding transcription model.

I started working on McCatmus in January, but because of a major personal event (I moved to Canada!), it took seven month of stop-and-go before the release of the V1. This was particularly challenging due to the scale of the project and its technicality (it was hard to get back into McCatmus after several weeks of interruption, which I had to do several times).

To add to this complexity, McCatmus was also a multi-front operation. Indeed, to create McCatmus, it was necessary to:

- define transcription guidelines in collaboration with other data producers,
- identify datasets compatible with the guidelines and set priorities,
- actually make all the dataset compatible with each other and clean some of the data,
- model and collect metadata that made sense for this dataset,
- release the dataset and fix the issues that came up.

To this date, two tasks remain on my to-do list for McCatmus: train a transcription model corresponding to this dataset and compare it with other existing ones, and make sure to have a publication describing this dataset and its usefulness.

My plan is to dedicate one post to the creation of the guidelines for the dataset, then a post about the identification and collection of the datasets used in McCatmus v1, and then I'll wrap up with a post about the process to create the dataset, the metadata and the release. Stay tuned!