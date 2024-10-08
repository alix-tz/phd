<!--
.. title: 016 - Text Recognition, Large Models and Expectations
.. slug: 016
.. date: 2023-11-28 05:28:15 UTC-05:00
.. tags: OCR, Large Language Models
.. category: reading-notes 
.. status:
.. link: 
.. description: 
.. type: text
-->

Since the boom around ChatGPT almost a year ago, I've heard several people wondering if "tools like ChatGPT" were more efficient than HTR models trained with [Kraken](https://kraken.re) and the like. The glimmer of hope in their eyes was most likely lit by their own struggle to set successful and/or efficient HTR campaigns with more traditional tools. The capacity of Large Language Models (LLMs) to reformulate a text[^spina] or, more specifically, of Large Multimodal Models (LMMs) to generate text based on a visual input may indeed lead people to believe that HTR technologies built on [CNNs](https://poloclub.github.io/cnn-explainer/) are on the verge of being flipped upside-down.[^multimodal_turn]

Annika Rockenberger recently conducted a series of small experiments on the matter and wrote [an interesting blog post](https://greflinger.hypotheses.org/739) about it. Let's summarize it!

She signed up for a premium subscription (25$/mo) to be able to chat with [GPT4](https://openai.com/gpt-4), which allows users to upload images. Then she submitted printed or handwritten documents she would normally transcribe with [Transkribus](https://readcoop.eu/transkribus) and assessed the results. She found that GPT4 was fairly good on ancient print (German Fraktur) and that it was even able to follow transcription guidelines if provided with an example. However on a letter bearing handwritten cursive, the model completely hallucinated the content and attempted a transcription in the wrong language. This didn't change when she provided more context on the document. Rockenberger concludes that there is a potential for using ChatGPT for HTR but that the capacity of scaling it up is completely unsure and that learning how to provide good prompts to get the appropriate results is a challenge. I would also add that in the end, Rockenberger paid 25$ to get 10 lines of raw text, whereas with software like Transkribus or eScriptorium, she would also get a standard structured output.

So, in other words, after reading Rockenberger's post, one can conclude that GPT4 (or, better, similar free and open source models) does have a potential for "quick and dirty-ish" OCR. However, I would argue that users tempted by this strategy might still miss an important point: even LMM-based tools will requires a little bit of organization and precision from the users. This, I find, often lacks in unsuccessful HTR campaigns. LMMs could generate a good output, but you will likely have to pay a counterpart one way or the other(s): with lower text recognition quality, with hallucinated text content, with impoverished non-structured output, with premium fees, etc.

Earlier this year, an article proposed by [Liu et al. (2023)](https://arxiv.org/abs/2305.07895), "On the Hidden Mystery of OCR in Large Multimodal Models", explored almost exactly the same topic but in a more comprehensive way. Their article presents an extensive survey of how well several Large [Multimodal](https://en.wikipedia.org/wiki/Multimodal_learning) Models (LMMs) performed on "zero-shot" tasks.

Zero-shot refers to the act of requesting an output from an LLM or a LMM without training it for this task in particular. It is very similar to Rockenberger's first attempt with GPT4, when she uploaded the image of a printed document and asked for its transcription. In such a case, she relied on the capacity of the model to transfer its knowledge to the specific tasks of Text Recognition, on a specific type of documents (historical printed text).

Other terms are often associated with "zero-shot:" "one-shot" and "few-shot". One-shot is equivalent to Rockenberger's second attempt: when she showed GPT4 an example of the output she expected on the 10 first lines of the documents, and requested that the model copied her strategy to generate the transcription of the 10 next lines. Few-shot would mean showing several pages and several expected output to the model before asking for the transcription of a new document.[^shot-definition]

The paper focused on currently available LMMs representing five different approaches for training LMMs:

- [BLIP-2](https://huggingface.co/docs/transformers/model_doc/blip-2),
- [Flamingo](https://arxiv.org/abs/2204.14198)/[Open-Flamingo](https://laion.ai/blog/open-flamingo/),
- [LLaVa](https://llava-vl.github.io/),
- [miniGPT4](https://minigpt-4.github.io/), and
- [mPLUG-Owl](https://huggingface.co/spaces/MAGAer13/mPLUG-Owl).

They evaluated the models on 4 tasks: text recognition, text-based visual question answering, key information extraction and handwritten mathematical expression recognition. Here are a few examples of what these tasks entail, as illustrated in the original article (on the images, P stands for Prediction and GT for Ground Truth):

| Task | Example |
| :--- | :------: |
| Text Recognition | ![Examples of failed Text Recognition](/images/LLM_text_recogntion.png "Four images contained printed of handwritten words along with the ground truth (expected transcription) and the prediction generated by the models. For example, the model predicted 'chocolate' when the expected transcription was 'choco'") |
| Visual Question Answering | ![Examples of failed Visual Question Answering](/images/LLM_textVQA.png "Two images of real-life views along with a question used as a prompt, the expected answer and the predicted answer. For example, when asked 'What is the yellow number?' on the image of an airport luggage retrieval conveyor belt showing a clear '7' in yellow in the background, the model provided the following answer: 'The yellow number on the luggage trolley is 32") |
| *Key Information Extraction | ![Examples of failed Key Information Extraction](/images/LLM_keyinfoextraction.png "Three images of real-life documents or textual information, along side with a question used as a prompt for the model, the expected answer and the predicted answer. For example, when asked 'what is the Sample No information in the input?', the model is expected to answer '1194-90' but answers 'The sample number is 33340'") |
| Handwritten Mathematical Expression Recognition | ![Examples of failed Handwritten Mathematical Expression Recognition](/images/LLM_HMExpr.png "Four example of failed attempts from the LMM to predict a LaTeX representation of handwritten mathematical expression: the numbers are wrong and/or the mathematical structure of the equations is made up") |

 <!-- "Four images contained printed of handwritten words along with the ground truth (expected transcription) and the prediction generated by the models. For example, the model predicted 'chocolate' when the expected transcription was 'choco'.") | -->
 <!--  "Two images of real-life views along with a question used as a prompt, the expected answer and the predicted answer. For example, when asked 'What is the yellow number?' on the image of an airport luggage retrieval conveyor belt showing a clear '7' in yellow in the background, the model provided the following answer: 'The yellow number on the luggage trolley is 32") | -->
 <!--  "Three images of real-life documents or textual information, along side with a question used as a prompt for the model, the expected answer and the predicted answer. For example, when asked 'what is the Sample No information in the input?', the model is expected to answer '1194-90' but answers 'The sample number is 33340'") | -->
 <!--  "Four example of failed attempts from the LMM to predict a LaTeX representation of handwritten mathematical expression: the numbers are wrong and/or the mathematical structure of the equations is made up") | -->

For each task, they used several datasets presenting different challenges. For each of these datasets and tasks, they retrieved the scores of the state-of-the-art (sota) for supervised methods and used them as a baseline. For example, for text recognition on the [IAM dataset](https://fki.tic.heia-fr.ch/databases/iam-handwriting-database), the sota method of AttentionHTR[^attentionhtr_ref] reaches a word accuracy of 91.24%.[^remark_wer] In comparison, Liu et al provide the following scores for the tested LMM on this dataset:

| test LMM                    | Score on IAM |
| :-------------------------- | :----------: |
| BLIP-2 OPT<sub>6.7b</sub>   | 38.00        |
| BLIP-2 FlanT5<sub>XXL</sub> | 40.50        |
| OpenFlamingo                | 45.53        |
| LLaVa                       | *50.40*      |
| MiniGPT4                    | 28.90        |
| mPLUG-Owl                   | 42.53        |
| ---------------             | -----        |
| Supervised SOTA             | **91.24**    |

The illustrations provided by the article are all of failed attempts, but it corresponds to the overall impression conveyed by the results of the experiments. Indeed, compared to the state-of-the-art supervised methods, zero-shot tasks prompted to LMMs yield results largely outperformed, similar to what is visible in the case of text recognition on the IAM dataset. The only exception is BLIP-2 on a Text Recognition task on a dataset of artistic text ([WordArt](https://github.com/xdxie/WordArt#wordart-dataset)) which is more challenging. The authors consider that this is a sign that LMMs have a promising potential for visually complex texts.

A very important section of their paper is their remarks on the relationship between LMMs and semantics. Submitting non-word images to the LMMs, they find that the LMMs systematically over-correct the prediction and suggest real-words as an answer. Traditional text recognition approaches, on the other hand, are much less sensitive to the notion of likelihood for the words to recognize. Similarly, the need for semantics interferes with the LMMs' output, and they tend to more easily recognize common words and make up additional letters ("choco" is read as "chocolate"). Lastly, LMMs are insensitive to word length: they are unable to count how many letters are in the image of a word. These results are similar to what Rockenberger experienced with the handwritten letter: the model hallucinated words to compose a semantically plausible letter. But using the wrong date, the wrong names, and the wrong language.

Liu et al conclude their paper reminding us that they experimented with the capacities of the models in the context of zero-shot prompts, whereas there are already successful attempts at fine-tuning LLMs and LMMs on specialized tasks, such as medical prediction. In fact, I think there already exist such attempts in the context of HTR as well: it seems to be the ambition of a model like Transkribus' Text Titan, released at the beginning of the Summer. It is based on a [Transformer](https://youtu.be/zxQyTK8quyY?feature=shared) coupled with an LLM. Unfortunately, I wasn't able to find more information on this model, aside from the community-oriented communications released by Transkribus on their website ([here](https://readcoop.eu/introducing-transkribus-super-models-get-access-to-the-text-titan-i/) and [here](https://help.transkribus.org/super-models)).

[^spina]: In stead of a multimodal approach, Salvatore Spina explored the possibility to use a LLM-based tool like ChatGPT3 to post-process the result of HTR and correct the text. See: Spina, S. (2023). *Artificial Intelligence in archival and historical scholarship workflow: HTS and ChatGPT* (arXiv:2308.02044). arXiv. [arXiv.2308.02044](https://doi.org/10.48550/arXiv.2308.02044).

[^multimodal_turn]: Multimodality is presented by some researchers of the Digital Humanities community as a real epistemological turn for the field. See for example: Smits, T., & Wevers, M. (2023). *A multimodal turn in Digital Humanities. Using contrastive machine learning models to explore, enrich, and analyze digital visual historical collections*. Digital Scholarship in the Humanities, fqad008. [doi: 10.1093/llc/fqad008](https://doi.org/10.1093/llc/fqad008) ; or Impett, L., & Offert, F. (2023). *There Is a Digital Art History* (arXiv:2308.07464). arXiv. [arXiv.2308.07464](https://doi.org/10.48550/arXiv.2308.07464).

[^shot-definition]: There are a few videos offering more or less detailed explanations on these expressions [in the context of prompting an LLM](https://www.youtube.com/watch?v=E6X1Ufhxtf0). However, this is not specific to LLM, it is often used in the context of [classification](https://huggingface.co/tasks/zero-shot-classification) or [NLP](https://joeddav.github.io/blog/2020/05/29/ZSL.html) tasks for example.

[^attentionhtr_ref]: Kass, D., & Vats, E. (2022). *AttentionHTR: Handwritten Text Recognition Based on Attention Encoder-Decoder Networks* (arXiv:2201.09390). arXiv. [arXiv.2201.09390](https://doi.org/10.48550/arXiv.2201.09390).

[^remark_wer]: In this case, the WER is used as a baseline to compare different approaches. However, in general, it is not a good idea to only take into account Word accuracy to understand a model's performance in real life. This is something I discussed in [this](../012/) post.
