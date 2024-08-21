<!--
.. title: 019 - The CATMuS Modern dataset #2
.. slug: 019
.. date: 2024-08-14 13:55:01 UTC-04:00
.. tags: CATMuS, HTR, guidelines
.. category: dataset
.. link: 
.. status:
.. description: 
.. type: text
-->

[Previous experiments](https://x.com/JMFradeRue/status/1730191566508060883) have shown that conflicting transcription guidelines in training datasets make it less likely that a model will learn to transcribe correctly. This is particularly relevant when it comes to abbreviations and it's something to keep in mind when merging existing datasets. We didn't really address this when we trained the [Manu McFrench model](https://inria.hal.science/hal-04094241) because it's difficult to retroactively align datasets to follow the same transcription rules. Unless you can afford to manually check every line, of course. In the case of Manu McFrench however, we only merged datasets that didn't solve abbreviations, so we ensured a minimum of cohesion.

CATMuS was built on the foundation laid by CREMMALab and the [annotation guidelines](https://hal.science/hal-03716526) developed by Ariane Pinche at the end of a seminar organized in 2021. These guidelines are intended to be generic, meaning they should be compatible with most transcription situations and are not project-specific. Following these guidelines will help data producers create ground truth that is compatible with data from other projects. It will also help those projects save time by not having to create transcription rules from scratch. From my experience, it is indeed easy for the members of a project discovering HTR to get caught up in the specifics of one project and forget what is and is not relevant (or even complicating) in the transcription phase.

> *It's worth mentioning that a project can choose to some of the CATMuS guidelines, while maintaining more specific rules for certain cases. If that's the case, the CATMuS guidelines can (should?) be used as a reference point. Ideally, the specific rules defined by a project should be retro-compatible with CATMuS. For example, if a project decides to use a special character will be used to mark the end of each paragraph, then in order to create a CATMuS-compatible version of the dataset, I should only have to replace or remove that character. In such cases, the special character that was chosen should be unambiguous and the rule should be explicitly presented.*

As CREMMALab focused on the transcription of medieval manuscripts, so did the first CATMuS dataset and guidelines. As I said in my [previous post](../018/), I focused on data covering the modern and contemporary periods, for which there was no equivalent to the CREMMALab guidelines. So, when extending CATMuS to these periods, I started with collecting existing guidelines and comparing them. I used the [CREMMA Medieval guidelines](https://hal.science/hal-03697382), the [CREMMA guidelines for modern and contemporary documents](https://gist.github.com/alix-tz/6f89444521bf1cab0522da520f7e4ff4), [SETAF's guidelines](https://hal.science/hal-04281804) and [CATMuS Print's guidelines](https://hal.science/hal-04557457) as a basis to elaborate the transcription rules for McCATMuS.

For each rubric, I [compared](https://docs.google.com/spreadsheets/d/1bFE-rRk6ZwgIHqXAOgwPo1s1zwQ-UPTLPnzjaRmTMsk/edit?usp=sharing) what each set of rules suggested, when they covered it. It was rare for all guidelines to align, but some cases were easy to solve. For example, all the guidelines recommended not to differentiate between regular s (`⟨s⟩`) and long s (`⟨ſ⟩`), except for the rules I had set for the modern and contemporary sources transcribed by CREMMA in 2021, before the CREMMALab seminar. It was thus decided that for McCATMuS there would be no distinction between all types of s's.

Some rubrics needed to be discussed to figure out why the rule had been chosen in the first place by some of the projects, to decide which one to keep for McCATMuS. In February, I met with Ariane Pinche and Simon Gabay to go over the rubrics that still needed to be set. One example of a rule we discussed is how hyphenations are handled. CATMuS Medieval and the two CREMMA guidelines say to always use the same symbol (`⟨-⟩`), whereas for the SETAF and CATMuS Print datasets, inline hyphenations (`⟨-⟩`) are differentiated from hyphenations at the end of a line (`⟨¬⟩`). Other symbols, like `⟨⸗⟩`, were unanimously rejected.

Two factors were considered when making those decisions: the feasibility of a retro-conversion for the existing datasets and the compatibility of the rule with a maximum of projects. In the case of hyphenations, I eventually decided to follow the same rule as CATMuS Medieval and CREMMA. On top of simplifying the compatibility of McCATMuS with CATMuS Medieval, I found that replacing all `⟨¬⟩` with `⟨-⟩`, rather than retroactively place `⟨¬⟩` where there was indeed an hyphenation at the end of a line[^hyphen] was much more straightforward.

Once the set of rules was fixed, I used it to sort between the different datasets I had identified (I'll discuss this in the next post) and to decide which one would be retained for McCATMuS v1. I also defined the transformation scenarios necessary to turn each of these datasets into a CATMuS-compatible version. Then, once McCATMuS v1 was ready, I integrated the modern and contemporary guidelines into the [CATMuS website](https://catmus-guidelines.github.io/), where the transcription guidelines for CATMuS medieval were already published.

Now that I am done integrating the rules set for McCATMuS into the website, I am confident that we have successfully designed rules that are overall compatible across the medieval, modern and contemporary periods, despite some unavoidable exceptions. Two good examples of the impossibility to cover a whole millennium of document production with the same rule are the [abbreviations](https://catmus-guidelines.github.io/html/guidelines/en/abbreviations.html) and the [punctuation signs](https://catmus-guidelines.github.io/html/guidelines/en/punctuation.html).

I've now explained how the transcription guidelines were established for McCATMuS. Next, I'll cover how they were integrated into existing datasets to create the first version of the McCATMuS dataset.

[^hyphen]: You can't assume that every instance of `⟨-⟩` at the end of a line must be replaced with a `⟨¬⟩`. In many cases, this can be a simple typographic decoration marking the end of a paragraph or the end of a title.
