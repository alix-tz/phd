<!--
.. title: 012 - "It did a very good job"
.. slug: 012
.. date: 2023-07-15 08:06:20 UTC-04:00
.. tags: metrics, HTR, accuracy, evaluation
.. category: mood
.. link: 
.. description: 
.. status:
.. type: text
-->

A few weeks ago, I attended the presentation of an automatic transcription software. The majority of the audience was unfamiliar with the concept of handwritten text recognition (HTR) or had little experience using it. The presentation lasted only an hour, so it couldn't delve into much detail. Its main objective was to demonstrate the software's results. The presenter showed several slides, displaying on one side  images of manuscripts (often in a language unknown to the audience) and on the other side the transcriptions generated by the software. Throughout the presentation, the presenter repeatedly commented on the HTR software saying that "it did a very good job."

But what does it even mean?

The very first aspect to explore is what distinguishes a good job from a bad one. Normally, such an evaluation relies on the measurement of the accuracy of the result compared to the ideal transcription. The accuracy can be expressed positively or negatively using the error rates (a 0% error rate is the same as a 100% accuracy).

Measuring the accuracy of a prediction (another way to call the result of HTR) is commonly done at character level. The character accuracy of a model is equal to the number of matches between the prediction and the ideal transcription. The character error rate (CER) is a very common measure to express a model's theoretical efficiency.

Sometimes softwares also consider the word error rate (WER), which is the proportion of words in the prediction containing errors. A high score at WER doesn't actually mean that the transcription is bad. It only means that the errors are distributed on all the words. I never use WER alone because it is hard to get an exact impression of the quality of the prediction based on that metric alone.

There is a paper from [Neudecker et al. (2021)](https://dl.acm.org/doi/10.1145/3476887.3476888) where they test 5 different software used for evaluating the prediction. They also develop an interesting reflection on alternative metrics such as the "non-stopword accuracy", the "phrase accuracy", the "flexible character accuracy" (which is useful when the line order isn't always the same), the "figure of merit" (which "aims to quantify the effort required for manual post-correction" (p. 15)) or else the "unordered WER".

When your score is a rate, there is an implicit idea that 100% is both the maximum score and the targeted score (for accuracy of course). But in the case of HTR, 100% accuracy is extremely rare because there are also edge cases where the way a letter was drawn is ambiguous: in such cases the error is not particularly caused by the inaccuracy of the HTR engine but rather by the imperfection of the handwriting in the first place.

In [Hodel et al., (2021)](https://openhumanitiesdata.metajnl.com/articles/10.5334/johd.46), the authors provided a grid to interpret accuracy scores. They suggest the following three thresholds:

- CER < 10% == good (it allows efficient post-processing)
- CER < 5% == very good (errors are usually focused on rare or unknown words)
- CER < 2.5% == excellent (but it is usually only reached when the handwriting is very regular)

Personally, I think this grid should also include 20% and 0%. 20% as a threshold, because at 80% of accuracy, the transcription is supposedly good enough for fuzzy search and keyword spotting (I should add a reference here, but I can't find it anymore...); and 0% because it should be reminded that an accuracy of 100% is virtually impossible.

To complement this, I would like to mention another possible approach to get an interpretable score: during the DH2023 conference, Thibault Clérice and I [presented an experiment](https://inria.hal.science/hal-04094241) where we trained a model using the same data in the train set and the test set. Our model reached an accuracy close to 90%, which we were able to use as a baseline to define the highest accuracy score possible for the data we had. Thus we were able to consider that a model approaching 90% of accuracy would be an excellent model, as far as that dataset was concerned.

Still during [the DH2023 conference](https://www.conftool.pro/dh2023/index.php?page=browseSessions&form_session=76#paperID395), Wouter Haverals introduced [CERberus 🐶🐶🐶](https://github.com/WHaverals/CERberus), a web interface which addresses the same type of issues as [KaMI](https://huggingface.co/spaces/lterriel/kami-app): the lack of nuance in a plain CER computation. Indeed, in a CER score, every type of error has the same weight. This means that mistaking an "e" for a "é" costs the same as mistaking a "e" for a "0": in the first case the text is likely still readable or understandable, whereas in the latter, it might not be the case.

The CER metric is still very useful, but when applied to transcription projects, it is even more valuable when we can filter the types of errors we want to include in the evaluation.

*EDIT: I should have noted here that my reflection was focused on the evaluation of an automatic transcription in cases where you already have the expected transcription. When we apply an HTR model to a whole new set of documents, we usually don't have the correct transcription at hand (otherwise we wouldn't use HTR in the first place). This is the reason why many researchers try to find ways to evaluate the quality of the transcription without ground truth. One example can be found in [Clérice (2022)](https://enc.hal.science/hal-03828529).*

So, to go back to our initial problem, we can see that there are many ways to draw the line between a good job and a bad one. The threshold will depend on the metric used to express the accuracy of the prediction and also (and actually mostly) on the way the generated text will be used down the line. Even though the software presentation I attended was short, I think we should always remind future users of HTR that 100% of accuracy is not always what they are seeking.

A short reflection to finish this post: I was bothered by the expression used to qualify the transcription. I am still trying to figure out a way to put it into words. On top of lacking accuracy, the expression "it did a good job" was also calling for a vision of HTR as a magic tool at the service of the searchers and students. But, in which other cases do you say that someone did "a good job?" Likely when you delegate a task to a [subaltern](https://africanarguments.org/2023/03/the-invisible-labour-of-africa-in-the-digital-revolution/).

I see a problem here: in their current state, HTR engines are efficient but not to the point that people can use them without thinking clearly about what they want the engine to produce. It is easy to sell a software pretending that it is a magic servant that will do all the transcription in your place, a tool so smart that you can even consider delegating a part of your responsibility to it. But I think when new users of HTR fail to first reflect on the outcome they can reasonably expect from these engines, it creates disappointment and crappy data and workflows.
