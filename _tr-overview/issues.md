---
layout: base
title:  'Issues with UD annotation of Turkish'
permalink: tr/overview/issues-with-turkish.html
---

# Issues in UD annotation of Turkish

This document is a summary of issues that arose during "pilot" UD annotation effort of Turkish.
Here is an overall summary:

* Changes/additions to the feature values.
* Multiple feature values for the same label.  
* Tokenization of surface words to IGs.  
    - copula / subordinating suffixes
    - other productive derivation
    - zero derivation from adjectives to nouns
* Compound verbs - light verb constructions
* Arguments/adjuncts, indirect objects

## Changes or additional values for `Tense`, `Aspect`, `Voice` and `Mood`

* **New `Tense` value `Pfut`**  
  Turkish verb forms allow combination of past and future tenses to
  refer to events that happened after a reference in the past.
  *oku-yacak-tı-m* "I was going to read". This, currently, is not
  covered with possible values of `Tense` feature. I currently mark
  these with `Pfut`, but open to suggestions for a better label.
  The double past (referring to an event before a past reference)
  is also possible *oku-muş-tu-m* 'I had read', but this matches the 
  description of `Tense=Pqp`.
* **`Narr`, currently listed as a possible `Tense` value, does not mark for tense.**  
  Current UD description lists `Narr` as a Tense value, explicitly
  referring to Turkish suffix *-mIş*.
  However, the main function of this indicating _evidentiality_ which
  matches `Mood` more than `Tense`. In its simplest use it also
  indicates `Tense=Past`. My suggestion is to drop the `Tense` value
  `Narr` (unless there is such a tense in another language), and add
  `Evid` as a potential value for `Mood`.  
  Some consider evidentiality as an orthogonal dimension in
  Tense/Aspect/Modality, but -mIş is definitely closer to specifying
  modality (speaker's view) then tense (point in time).
* **New values for `Aspect`**  
  - `Habit` habitual. One of the main function of the aorist marker in
    Turkish is to indicate things that are done repeatedly or
    habitually, *sigara iç-er* "he smokes".
  - `Rapid` the verbal suffix *-Iver* in Turkish causes an aspect
    change in the predicate that roughly corresponds to action being
    done quickly/immediately/rapidly, *gid-iver* "go quickly".
  - `Dur` durative. The suffix *-Akal* indicates that the action
    lasts/lasted for a (long) time, *bak-akal-dı* "he/she froze
    looking/staring".
* **New values for Mood**  
  - `Abil` possibility/permission. The verbal suffix *-Abil* indicates 
     whether an action is possible or allowed. *gid-ebil-ir-sin* "you
     may go", *oku-yabil-ir* "she/he can read".
  - `Gen`. This is called "generalized modality" by Göksel & Kerslake
    (2005, p295). It expresses a generalization of hypothesis by the
    speaker. The verbal suffix *-DIr* serves exclusively for this
    purpose, but aorist marker *-Ir/-Ar* also introduces this mood.
    *gitmişler-dir* "they must have gone/arrived", *çocuk şimdi
    uyuyordur* "the child must be sleeping now".

## Multiple feature values

Following standard UD features, some word forms in Turkish require multiple feature values.
Occasionally it also involves repeating the same value.
For Turkish this mainly includes verbal features `Voice`, `Aspect` and `Mood`,
but `PronType` also needs multiple values.
The issue with the `Voice` feature raised 
[here](https://github.com/UniversalDependencies/docs/issues/197).

In most languages, _Aspect_, _Voice_ and _Modality_ of a clause/predicate is constructed analytically.
In Turkish these features are often packed into a single verb in most cases.

