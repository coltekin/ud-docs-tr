---
layout: base
title:  'Tokenization'
permalink: tr/overview/tokenization.html
---

# Tokenization

Word tokenization of Turkish is similar to the other languages writing using Latin alphabet.
However, syntactic analysis of Turkish often requires sub-word units as "syntactic words."
These tokens are called *inflectional groups* (IGs) in Turkish NLP literature,
and typically determined during morphological analysis/disambiguation.

## Inflectional Groups

In Turkish (and some/all other Turkic languages),
the POS of a word may change with suffixation in a way that diverge from derivational morphology observed in other languages.
The important difference is
that parts of a word may refer to different entities and/or actions may carry their own inflectional features
and may participate in different syntactic relations.

A typical example is the following sentence:
~~~~
Mavi arabadakiler   uyuyor
blue car-LOC-KI-PLU sleep-PROG
`the ones in the blue car are sleeping'
~~~~

In this example, the suffix *-ki* in word *arabadakiler* introduces a new syntacic unit.
The first IG refers to a car,
while the second IG refers to the people (in the car). 
We split the word *arabadakiler* into two syntacitc units,
which would be represented in  CoNLL-U as follows:

~~~~
1   Mavi            mavi    ADJ     _
2-3 arabadakiler    _
2   _               araba   NOUN    _   Number=Sing|Case=Loc
3   _               -ki     NOUN    _   Number=Plur|Case=Nom
5   uyuyor          uyu     VERB    _
6   .               .
~~~~
In principle, instead of introducing unspecified surface form for the parts,
the word can be segmented (*arabada*+*kiler*).
However, we do not require surface forms for IGs,
since determining correct segmentation is non-trivial in some cases.

The decision of splitting words into multiple IGs is based on two reasons:

- parts of a word have their own set of inflections,
  potentially with conflicting feature-value pairs
- parts of a word may  participate in different syntactic relations

In the example above, *araba* 'car' is singular (`Number=Sing`)
and marked for locative case (`Case=Loc`),
while *arabadaki* 'the one/person in the car' is not marked for case,
and receives the plural suffix.
In other words,
the locative marker only applies to the first IG,
and the plural marker to the second.
Furthermore, the adjective *mavi* 'blue' modifies *araba*,
the car is blue not not the people inside.
And the subject of the verb *uyu* 'sleep' is the second IG,
(sleeping is done by the people in the car, not by the car).


~~~ conllu
# His/her passion was racing cars
1       Tutkusu tutku   NOUN    NOUN    Number=Sing     3       nsubj
2       yarış   yarış   NOUN    NOUN    Number=Sing     3       nmod:poss
3       arabaları       araba   NOUN    NOUN    Number=Plur     0       root
4       –ydı    _       VERB    VERB    Number=Sing|Person=3|Tense=Past 3       cop
~~~


~~~ conllu
# The ones in the blue car are sleeeping
1	Mavi	mavi	ADJ	ADJ	_	2	amod
2-3	arabadakiler	_	_	_	_	_	_
2	_	araba	NOUN	NOUN	Number=Sing|Case=Loc	3	nmod
3	_	-ki	NOUN	NOUN	Number=Plur|Case=Nom	4	nsubj
4	uyuyor	uyu	VERB	VERB	Number=Sing|Aspect=Prog|Tense=Pres	0 root
5	.	.	PUNCT	PUNCT	_	4	punct
~~~

METU-Sabancı treebank makes excessive use of IGs.
In UD, we limit the suffixes/cases that introduce a new IG to the following.

## Some general principles

* We do not split lexicalized derivations, even if the suffix may be
  productive and split in other cases.
  For example, the suffix *-lIk* in word *kitap<b>lık</b>* 
  "book-lIk (= book shelf)" does not introduce a new IG. On the other
  hand, the same suffix makes adjectives with the approximate meaning
  of 'fit/suitable for', where the parts may participate in different
  syntactic relations. For exmaple in *çantamda üç kitap<b>lık</b> yer var* 
  'I have space for three books in my bag' the numeral *üç* modifies
  the noun *kitap* not the resulting adjective.

## Suffixes that introduce new IGs 

### Copular markers 

Copular suffixes attached to nouns an adjectives
create predicates. For example, the word/sentence *arabadaydım*

~~~
Arabada -ydık.
car-LOC -CPL.PAST.1PL
I was in the car.
~~~

Note that the `Number` feature on the noun is `Sing`, while the
predicate agrees with a `Plur` subject. If both IGs are combined
resulting feature values would be confusing. Furthermore, in
combination with the subordinating suffixes,
we end up with situations with multiple subjects headed by the same word (see below for an example).

We do not mark copula as the head (see below for a possible exception).

#### Zero copula

In Turkish, a noun or adjective is suffixed with a copular suffix or followed by copula *ol-* (and less frequently with *i-* or *bul-*) for it to function as a predicate.
The suffix form, however, is not expressed in case of third person singular subject.
Currently we mark these forms with a zero-copula as well.
Although this is somewhat against the UD principle of not marking null elements,


### Subordinating suffixes

The primary means of subordination in Turkish is through a set of subordinating suffixes that attach to the predicates.
The resulting clauses function as nouns, adjectives or adverbs.
We split all subordinating suffixes regardless of their form or function.

~~~
oku─duklarımdan
oku.PAST─VN.P1SG.PLU.ABL
of the things I read
~~~

Here, if we do not split off the subordinating suffix, we end up an
`Case` feature (`Abl`) on a verb, or `Tense` feature on a noun (which
may be fine if the noun was a predicate, but that is not necessary).
Also note that predicate *götür* 'take' needs `Number=Sing`, while the resulting nominal phrase is plural. 

This becomes even more complicated if a copula is attached to the verbal noun:
~~~
(O)    (benim) eve       dün        götür-düklerimden-miş.
It     I.GEN   house.DAT yesterday  take.PAST.1SG-VN.PLU.ABL-cop.EVID.3SG
It was (apparently) one of those I took home yesterday.
~~~
Now, not only that all sorts of feature conflicts between two predicates and the intervening nominal are possible, 
the two predicates have different subjects.

Splitting these suffixes also makes syntactic relation parallel to other languages, where the word that causes the subordination marked with relation `mark`.

Similar to the suffix *-ki* we treat the subordinating suffixes that result in an adjective or adverbial phrase than the subordinating suffixes that result in a noun phrase.

### -ki 

The suffix *-ki* attaches to a genitive- or locative-marked noun and derives adjectivals. The scope of *-ki* is the whole noun phrase headed by the nominal it attaches (the noun may be modified by any other means nouns can be modified, including complex relative clauses). The adjectival, like any adjective in Turkish, can act as a (pro)noun. We treat the adjective and noun usage separately. In both cases we split the original word and introduce a new IG starting with *-ki*.

#### If the result is an adjective

In this usage, *-ki*'s forms phrases that modify nouns,
*Evde<b>ki</b> kitap* 'the book (that is) at home'.
We mark the IG with *-ki* as `ADJ`.
The head of the word is a `NOUN`, and the *-ki* is attached to the noun using the `case` relation.
Although the resulting phrase functions as an adjective, since we mark the noun as the head, we also mark the relation between the *-ki* phrase and the noun it modifies as `nmod`.
We do not use an subtypes since the necessary information can be recovered from the IG with *-ki*.

~~~ sdparse
büyük masada ─ki kitap \n big table-LOC─ki book (= the book [that is] on the big table)
amod(masada, büyük)
nmod(kitap, masada)
case(masada, ─ki)
~~~

The use of `case` here does not refer to a accepted linguistic case in the language, but it is inline with non-standar case marking in UD.

#### If the result is a noun

In this case *-ki* refers to the person/thing that is modified  by the stem (either owned by or located at), *evdeki* 'the one at home', *annem-ler-in-ki* 'the one that belongs to my parents'.
In this usage, we mark the IG with *-ki* as noun (without the intervening adjective IG).
In this case the head is the IG with  *-ki*, and relation is `nmod`.

~~~ sdparse
akşam yedide ─ki  altyazılı \n the one at seven in the evening is with subtitles
nmod:tmod(─ki, yedide)
nsubj(altyazılı, ─ki)
nmod(yedide, akşam)
~~~

The resulting nominal may further receive any nominal inflections, and potentially another *-ki* which may again function as a (pro)noun or adjective.
This process may potentially repeat indefinitely, but more than two *-ki* suffixes are generally difficult to comprehend.
The following is an example from the web with two *-ki*'s.

~~~
bizim   ─kinin  ─kiler  alltan    çıktı
We.GEN  ─ki.GEN -ki.PLU below.LOC come_out.PAST
The ones of ours came out from below
(referring to the first teeth of the speaker's baby)
~~~


## Some other (derivational) suffixes

Some (derivational) suffixes in Turkish scope over complete phrases/clauses rather than a single word.
Not splitting these suffixes results in dependencies relating wrong words.
The following lists the suffixes that should be split.

Some of these suffixes have multiple functions/meanings,
and in some these functions it splitting may not be necessary.
However, this decision is generally difficult to make.
We always split the suffixes listed below,
with the exception when the derived form is lexicalized.
For example the suffix -lIk in *çantamda üç kitap<b>lık</b> yer var* 'I have
space for three books in my bag' should be split, 
but not in *bir kitap<b>lık</b> aldım* 'I bought a book shelf'.

#### The suffix -lI

This is a productive derivational suffix that derives adjectives and nouns from nouns. It has multiple functions. In some of these functions we can perfectly do without splitting, but for the sake of ease of annotation we split all, except clearly lexicalized cases, e.g., *akıl<b>lı</b>* 'mind-lI = clever', *köy<b>lü</b>* 'village-lI = villager', *üç-lü* 'three-lI = trio'.

##### Examples

* *üç yatak odalı ev* 'a/the house with three bedrooms'.
  The correct bracketing is *[üç [yatak oda]]-lı ev* not 

* *nefis çikolata-lı kek* 'delicious chocolate cake' but
  *siyah çikolata-lı kek* 'dark-chocolate cake'. And further,
  *nefis siyah çikolata-lı kek* 'delicious dark-chocolate cake'

* *kırmızı çiçek-li kumaş* 'fabric with red flowers' or (unlikely)
  'red fabric with flowers'.

#### The suffix -sIz

This suffix affects noun phrases, roughly meaning 'without'. It can
also be attached to inflected nouns.

*spor arabam<b>sız</b> yapamam* 'I cannot do without [my sports car]'.

#### The suffix -CI

**-CI** derives nouns from nouns, typically it derives "occupation nouns", e.g.
*şarap-çı* 'the wine maker/seller' but also someone who has a taste/preference
for something, so the example could also mean 'person who prefers wine (over
beer)'. Although not very frequent, the non-derived noun can be modified by
other words: *[kırmızı şarap]-çı* 'person preferring red wine (over white
wine)'/'red wine seller'.

#### The suffix -lIk

This adds the meaning of 'sufficient/suitable for'.

Repeating the example above: *çantamda üç kitap<b>lık</b> yer var* 'I have
<b>space for</b> [three books] in my bag'.

#### The suffix -DIr forming time adverbials

Suffix -DIr has a number of different functions, one of which is
deriving adverbials from time expressions, e.g., *yıllar-dır*
'for/since years'. If the time expression is modified, this
modification is local. For example *üç yıl-dır* 'for/since three
years' be analyzed as *[üç yıl]-dır* 'for [three years]'.


## What we do not split

As much as what to split, it may be useful to know what not to.

### Zero derivation from an ADJ to NOUN

In Turkish any adjective functions a (pro)noun referring to a thing
with the property described by the adjective, *<b>Mavi</b>-yi ver.* 'hand me
<b>the blue one</b>'. This is typically analyzed as adjective becoming
a noun with a zero derivation. We do not introduce multiple IGs in
this case, we just mark the adjective as a noun. 
