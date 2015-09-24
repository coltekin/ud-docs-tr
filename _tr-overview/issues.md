---
layout: base
title:  'Some issues with UD annotation of Turkish'
permalink: tr/overview/issues.html
---

# Some issues in UD annotation of Turkish

This document is a summary of issues that arose during "pilot" UD annotation effort of Turkish.
Here is an overall summary:

* Table of contents
{:toc}


## Changes or additional values for `Tense`, `Aspect`, `Voice` and `Mood`

* **New `Tense` value `Pfut`**  
  Turkish verb forms allow combination of past and future tenses to
  refer to events that happened after a reference in the past.
  *oku-yacak-tı-m* "I was going to read". This, currently, is not
  covered with possible values of the `Tense` feature. I currently mark
  these with `Pfut`, but open to suggestions for a better label.
  The double past (referring to an event before a past reference)
  is also possible *oku-muş-tu-m* 'I had read', but this matches the 
  description of `Tense=Pqp` defined already.
* **`Narr` is currently listed as a possible `Tense` value, but it does not mark for tense.**  
  Current UD description lists `Narr` as a Tense value, explicitly
  referring to Turkish suffix *-mIş*.
  However, the main function of this is indicating _evidentiality_ which
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

The issue definitely affects the`Voice` feature, and it was raised 
[here](https://github.com/UniversalDependencies/docs/issues/197) earlier.
In summary: Turkish verbs may get four different voice values,
*passive*, *causative*, *reflexive* and *reciprocal*.
First two are very productive.
A large number of combinations the voice features/suffixes may be observed on a single verb.
Here are a few examples:

* `Pass`+`Cau` *bu kitap oku-<b>t</b>-<b>ul</b>-du* "by someone's mandate this book was read".
  Somewhat direct translation attempt does not feel right in English,
  but this is a quite common construction, e.g., that one can see in a
  newspaper report meaning roughly 
  "(by the mandate of the ministry of education) the book to be read in the classes (by the students)".
* `Cau`+`Rcp` *öp-<b>üş</b>-<b>tür</b>-dü* "she/he caused them to kissed each other"
* `Cau`+`Pass`+`Rcp` *döv-<b>üş</b>-<b>tür</b>-<b>ül</b>-dü* "they are caused to fight each other"

An addition to the above is the fact that `Cau` and `Pass` can appear more than once on a single verb (See the examples/explanation
[here](https://github.com/UniversalDependencies/docs/issues/197))

If the `Aspect` and `Mood` values discussed above are introduced, we
also have the same issue with these features.
Here are some examples:

* `Rapid`+`Perf` *yap-<b>ıver</b>-<b>di</b>* "he did it quickly"
  (aspect: action is completed, and done quickly).
* `Rapid`+`Prog` *yap-<b>ıver</b>-<b>yor</b>* "he is doing it quickly"
  (aspect: action is in progress, but being done quickly).
* `Dur` + `Perf` *bak-<b>akal</b>-<b>dı</b>* "he froze staring (at
  something)" (aspect: completed action, took a while)
* `Gen` + `Nec` *yap-ıl-ma-<b>malı</b>-<b>dır</b>* "it must/should 
  not be done" (mood: it is a necessity according to the speaker, 
  but also a general rule or hypothesis. Contrasts with, e.g.,
  *yapılmamalıydı* "it should not have been done")
* `Abil` + `Nec` *yap-ıl-<b>a</b>-ma-<b>malı</b>* "it should not be possible"
* `Evid` + `Nec` *yap-ıl-<b>malı</b>-<b>ymış</b>* "Apparently it has
  to be done"

We either need to define separate features for the values, or allow set- of list-valued features.

A last note (orthogonal to the above discussion) is that, although
most suffixes in Turkish has clearly defined functions with respect to
tense/aspect/modality, there are cases of overlap in functions of
multiple suffixes or some suffixes may have multiple functions in
different contexts. In general some inference is necessary to assign
these features correctly.

## Tokenization of words into _inflectional groups_.

In dependency parsing/annotation of Turkish, 
we often need relations between sub-word units called _inflectional
groups_ (IGs) in the literature.
Here is an example (sub-word syntactic units are delimited with a
dash, inflectional features are separated with a dot):

~~~
Kaygımız    terörün     yeniden başlamasıdır.
Worry.P3PL  terror.GEN  again   start-VN.P3SG-COP.GEN
Our worry is that terror may/will start again.
~~~

The important part to note here is that, the verb *başla* "start" has
the subject *terör* "terror" and the copula has the subject *kaygımız*
"our worry". However, the copula is a suffix (eventually) attached to
the verb *start*. Hence, the same word needs two subject dependencies.
Here is a potential analysis with inflectional groups avoiding
multiple subjects:

~~~ sdparse
Kaygımız    terörün     yeniden başla –ması –dır .
nsubj(başla, terörün)
nsubj(–ması, Kaygımız)
cop(–ması, –dır)
~~~

The following is a (partial) proposal about which suffixes should be split, and how the resulting structure be analyzed.

### Copula

Copular suffixes attached to nouns and adjectives create predicates. 
For example, the word/sentence *arabadaydık*

~~~
Arabada -ydık.
car-LOC -CPL.PAST.1PL
I was in the car.
~~~

Note that the `Number` feature on the noun is `Sing`, 
while the predicate has `Number=Plur`.
If both IGs are combined resulting feature values would be confusing.
Furthermore, the `Case=Loc` only make sense for a noun, and
`Tense=Past` only makes sense for a predicate.

The parts may also participate syntactic relations.
For example,

~~~ sdparse
Biz mavi arabada –ydık . \n We were in the blue car.
cop(arabada, –ydık)
nsubj(arabada, Biz)
amod(arabada, mavi)
~~~

Although we avoid direct dependencies involving the copula because of UD principle of not marking copula as head,
the `nsubj` relation only makes sense for a noun if it is a predicate.
Furthermore, in combination with the subordinating suffixes discussed below,
we end up with situations where the split is important.

### Subordinating suffixes

The primary means of subordination in Turkish is through a set of subordinating 
suffixes that attach to the predicates.

~~~
oku–duklarımdan
oku.PAST─VN.P1SG.PLU.ABL
of the things I read
~~~

Here, if we do not split the subordinating suffix, we end up with a `Case` feature (`Abl`) on a verb, 
or `Tense` feature on a noun
(which may be fine if the noun was a predicate, but that is not necessarily the case).
Also note that predicate *oku* 'read' needs `Number=Sing` (singular subject), 
while the resulting noun phrase is plural (there are multiple items that were read). 

In general, a clause with a subordinating suffix functions as a `NOUN`, `ADJ` or `ADV`.
We treat `NOUN` case slightly differently (reasons are discussed below).

#### Adjectivals and Adverbials

A number of suffixes create subordinated clauses that behave exactly
like adjectives or adverbs.
Here is a (possibly incomplete) list:

* Adjective
    - *–DIK* *oku-<b>duğ</b>-um kitap* "the book (that) I read"
      (past or present), 
    - *–AcAK* *oku-<b>yacağ</b>-ım* "the book (that) I will read"
    - *–(y)An* *oku-<b>yan</b> çocuk* "the children who reads"
    - *–(y)AsI* *oku-n-<b>ası</b> kitap* "the book that is worth reading"
* Adverb
    - *–(y)ken* *oku-r-<b>ken</b> düşün* "think <b>while</b> reading"
    - *–(y)Ip* *oku-<b>yup</b> anla*  "understand <b>by</b> reading"
    - *–(y)IncA* *oku-<b>yunca</b> konuşalım*  "let's talk <b>after</b> reading"
    - *-dAn* *oku-ma-dan nasıl biliyorsun?* "how do you know <b>without</b> reading"
    - *-(y)ArAk* *oku-<b>yarak</b> daha iyi anlarsın* "you'd understand better <b>by</b> reading"
    - *-(y)A ... -(y)A*  *oku-<b>ya</b> oku-<b>ya</b> öğrenmiş* "he (apparently) learned </b>by</b> reading (repeatedly)"
    - *–cAsInA* *oku-muş-<b>çasına</b> konuşuyor* "he is talking <b>as if</b> he read (it)"
    - *-dIkçA* *oku-<b>dukça</b> daha çok ögrenisin* "you learn more <b>as</b> you (continue) read(ing)"
    - *–(y)AlI* *oku-<b>yalı</b> çok zaman geçti* "it has been a while <b>since</b> I(/he/she/we/them) read it"

Note that the list only uses simple verb "oku" as the subordinate clause for brevity,
but this could get as complicated as any clause with other (possibly clausal) modifiers.

Currently we split the subordinating suffix from the predicate,
and mark it with the resulting category of the word (this follows the
earlier work, e.g., METU-Sabancı treebank, or morphological analyzers
like TRmorph. 
It is also more informative then marking these units as `SCONJ` or `PART`).
We analyze the predicate as the head, and relate the subordinating IG with `mark`.
Here are a few examples:

~~~ sdparse
dün oku -duğum kitap \n the book I read yesterday
mark(oku, -duğum)
acl(kitap, oku)
~~~

~~~ sdparse
Ali bu kitabı okur -ken çok düşünmüş \n Ali thought a lot while reading
this book .
mark(okur, -ken)
advcl(düşünmüş, okur)
~~~

Since (in these cases) the subordinating marker itself does not follow any other suffixes
and since we attach the modifiers of the resulting adverbial or
adjectival to the head (predicate,
it may not be absolutely necessary to split the marker
(we could use `VerbForm` or a similar mechanism to flag that the clasues functions as an adjective or adverb).
However, the present proposal is to split it for a (more) parallel
treatment of them with the languages with a free subordinating
morpheme,
and a more uniform treatment of subordinating suffixes with the verbal nouns discussed below.

#### Verbal nouns

We use the term *verbal noun* for clauses that function as nouns in the higher-level clause.
Verbal nouns are similar verbal adjective and adverbs discussed above. 
However, they are treated differently, 
since the resulting nominal can be modified by modifiers/suffixes.
Within the main/higher-level clause, verbal nouns in Turkish act more like common nouns than clauses.
The following suffixes attached to predicates form verbal nouns.

* *–DIK/–AcAK/–An*   
  These are parallel to the verbal adjectives discussed above. 
  In general, any adjective in Turkish can be interpreted as a noun
  referring to an object having the property defined by the adjective.
  The verbal nouns formed by these suffixes has the same property.
    - *oku-<b>duğ</b>um evde* "<b>the one</b> I read (or am reading) is at home"
    - *oku<b>yacağ</b>ım bu* "this is <b>the one</b> I will read (next)"
    - *öğretmen okuma<b>yan</b>ları uyardı* "The teacher warned <b>the one</b>s who didn't/haven't read"
    - *öğretmen okuma<b>yan</b>ları uyardı* "The teacher warned <b>the one</b>s who did not read"
* *–yIş* makes nominal clauses with the approximate meaning of "the
  way the action is done".
  *Ali'nin oku<b>yuş</b>u* "The way Ali reads"
* *–mAK* makes nominal clauses. The resulting clause have a similar 
  meaning to English infinitives and gerunds, but there are
  differences in usage.
  *Ali oku<>mak</b> istiyor* "Ali wants <b>to</b> read"
* *–mA* is similar to *-mAK* but *-mA* clauses include their subject
  (either by a genitive marked overt noun, or a suffix attached after the
  subordinating suffix).
    - *oku<b>ma</b>sını istiyor* "he/she wants him/her to read"
    - *Ali'nin oku<b>ma</b>sını istiyor* "he/she wants Ali to read"
    - *Babası Ali'nin oku<b>ma</b>sını istiyor* "his father wants Ali to read"

With some restrictions (especially regarding *-mAK*), 
the resulting verbal nouns can inflect like nouns, 
and participate any syntactic relation that nouns can participate.
Here are some examples:

- *Ali'nin oku<b>duğ</b>u yere düştü* "The one Ali is reading fell"
  (subject of a verb)
- *Ali'nin oku<b>duğ</b>u çok güzel* "The one Ali is reading is very nice"
  (subject of a nominal predicate)
- *Ali'nin oku<b>duğ</b>unu ben de okudum* "I also read the one Ali is reading"
  (accusative object)
- *Ali'nin oku<b>duğ</b>una bakıyorum* "I am checking out the one Ali is reading"
  (dative marked argument/adjunct)
- *Ali'nin oku<b>duğ</b>undan bir şey anlamadım* "I did not understand anything from the one Ali is reading"
  (ablative adjunct/adverbial modifier)
- *Ali'nin oku<b>duğ</b>nun yazarını tanımıyorum* "I do not know/recognize the author of the one Ali is reading"  (modifier in a genitive-possessive noun compound)

In all examples above *Ali'nin okuduğu* can be replaces with any ordinary (definite) noun.
Besides the definite object that is defined by the corresponding
adjectival clause, 
the same verbal noun may represent a fact/concept, which is also
shared by the other verbal noun suffixes that does not form
adjectivals (*-mA* and *-mAK*).

- *Ali'nin oku<b>duğ</b>unu biliyorum* "I know the fact that Ali read/reads"
- *Ali'nin oku<b>ma</b>sını destekliyorum* "I support the fact that Ali reads"
- *Ali'nin oku<b>ma</b>sını destekliyorum* "I support the fact that Ali reads"
- *Ali'nin oku<b>ma</b>sı herkesi şaşırttı* "The fact that Ali reads surprised everyone"
- *Ali'nin oku<b>yuş</b>u sorunlu* "The way Ali reads is problematic"
- *Ali'nin oku<b>ma</b>sı sorunu* "The problem of the way/situation/fact that Ali reads" (-sI nominal compound)
- *Ali'nin oku<b>ma</b>sının sorunumuz* "Our problem of the way/situation/fact that Ali reads" (genitive-possessive nominal compound)

In most cases, the subordinate clause formed by one of these suffixes can be translated as "the object/concept/fact that ...".
Because of that, my current proposal is to mark the IG with the subordinating suffix as the head of the construction.
We mark the relation between the clause and the head with `acl` (as it would be marked in the parallel constructions in English).
This also allows treating these clauses like ordinary nouns,
keeping the analysis parallel to the cases where an ordinary noun occupies the same syntactic slot.
It also resolves the problem of two subjects if a copular suffix attached to the verbal noun.
This also means that we use normal noun modifier relations instead of
clausal relations in this context.
Here are a few examples:

~~~ sdparse
Ali'nin oku –duğnu biliyorum \n cf. I know the fact that Ali reads
nsubj(oku, Ali'nin)
dobj(biliyorum, –duğnu)
acl(–duğnu, oku)
nsubj(reads, Ali)
dobj(know, fact)
acl(fact, reads)
~~~

~~~ sdparse
Ali'nin doktor ol –duğnu biliyorum  \n I know that Ali is a doctor
nsubj(doktor, Ali'nin)
cop(doktor, ol)
dobj(biliyorum, –duğnu)
acl(–duğnu, doktor)
~~~

~~~ sdparse
Ali'nin oku –masının zorluğu \n The difficulty of Ali's reading
nsubj(oku, Ali'nin)
acl(–masının, oku)
nmod:poss(zorluğu, –masının)
~~~

~~~ sdparse
Ali'nin oku –duklarından biri \n One of those Ali has read
nsubj(oku, Ali'nin)
acl(–duklarından, oku)
nmod:part(biri, –duklarından)
~~~

~~~ sdparse
Bu kitap Ali'nin oku –duğu -ymuş \n This book is the one Ali has read
acl(–duğu, oku)
cop(–duğu, -ymuş)
nsubj(–duğu, kitap)
nsubj(oku, Ali'nin)
~~~

##### The suffix -mAK


The verbal noun suffix -mAK is somewhat different than the other verbal noun suffixes.
Most of the times (but not always) its subject resolves to one of the arguments of the higher-level clause.
It also cannot precede some of the nominal suffixes,
such as plural suffix and possessive suffixes.
Although, it receives any of the case suffixes,
and a verbal noun formed by -mAK can be in any position a noun can appear in a clause.
An option is to mark -mAK differently than other verbal noun suffixes.

~~~ sdparse
Ali oku –mak istiyor \n Ali wants to read
nsubj(istiyor, Ali)
xcomp(istiyor, oku)
mark(oku, –mak)
~~~

This sentence requires that Ali wants to read _himself_, he cannot be
wanting for someone else to read.
The above analysis signals that the subject of _oku_ "read" can be found in
the higher-level clause.
However, it also breaks the parallel with the other forms,
requires that the copula is marked as head if the verbal noun formed by *-mAK* is followed by one (in this case it is also unclear what relation to use between the copula and the subject-complement clause that *–mAK* is attached to).

<!---
<del datetime="2015-08-12 18:00">
**Change of minds as of 2015-08-12 18:00** - or maybe not.
-->
Current proposal suggests, marking -mAK clauses like other verbal nouns (–mAK as the head),
but possibly using a subtype of `acl` to signal that subject is missing.
<!---
</del>
-->


#### Possessive suffixes deriving (pro)nouns from adjectivals

A rather productive construction derives nouns from adjectives,
numerals and determiners by attaching possessive suffixes. 
The ones derived from determiners are mostly lexicalized, 
and determiner part cannot be modified. 
However, in other cases, the adjectives or numbers can be modified by other words in the sentence.
The verbal adjectives discussed above can also undergo the same derivation.

This is most commonly used in "partitive" constructions.
Here are a few examples:

* *küçüğ<b>ü</b> daha akıllı* "the little <b>one</b> is smarter"
* *bunun mavi<b>si</b>nden istiyorum* "I want the blue <b>one</b> of this"
* *o en büyüğ<b>müz</b>* "he is the biggest/oldest <b>of us</b>"
* *sadece üç<b>ünüz</b> buraya sığabilir* "only three <b>of you</b> can fit here"
* *en iyi gören<b>iniz</b> arkada oturmalı* "the <b>one (of you)</b> who sees best should sit at the back"

In this case, we split the word, and mark the last part of the IG
(derviation) as the head of the construction (reason for this is
similar to the reason for verbal nouns above).

~~~ sdparse
en büyüğ –ümüz
advmod(büyüğ, en)
amod(–ümüz, büyüğ)
~~~

Note that there is a fine difference between possessive drived
pronouns discussed here and zero-derived nouns followed by a
possessive suffix.

- possessive derivation: *benimki bunun mavisi* "mine is the blue (version) of this"
- possessive inflection: *mavisi bozulmuş* "his blue one is broken"

#### Other derivational suffixes that introduce new IGs

A number of other suffixes derive new word forms.
We currently limit the suffixes that introduce new IGs.
Furthermore, as a general rule,
if the use of the suffix results in a lexicalized form, 
we do not split it.

~~~ sdparse
tuzlu su \n salty water (water with salt)
amod(su, tuzlu)
~~~

~~~ sdparse
iki havuz –lu ev \n house with two swimming pools
nummod(havuz, iki)
case(havuz, –lu)
nmod(ev, havuz)
~~~

The use of `case` above does not refer to a accepted linguistic case in
the language, but it is in-line with non-standard case marking in UD.

The following lists the suffixes that we split,
provided that the derivation does not result in a lexicalized form.

##### *-ki* 

The suffix *-ki* attaches to a genitive- or locative-marked noun and
derives adjectivals. The scope of *-ki* is the whole noun phrase
headed by the nominal it attaches (the noun may be modified by any
other means nouns can be modified, including complex relative
clauses). The adjectival, like any adjective in Turkish, can act as a
(pro)noun. We treat the adjective and noun usage separately. In both
cases we split the original word and introduce a new IG starting with
*-ki*.

Similar to the verbal- and adjectival- noun difference,
we treat the noun and adjective use of *–ki* differently.

~~~ sdparse
büyük masada ─ki kitap \n big table-LOC─ki book (= the book [that is]
on the big table)
amod(masada, büyük)
nmod(kitap, masada)
case(masada, ─ki)
~~~

~~~ sdparse
akşam yedide ─ki  altyazılı \n the one at seven in the evening is with
subtitles
nmod:tmod(─ki, yedide)
nsubj(altyazılı, ─ki)
nmod(yedide, akşam)
~~~

The resulting nominal may further receive any nominal inflections, and
potentially another *-ki* which may again function as a (pro)noun or
adjective.
This process may potentially repeat indefinitely, but more than two
*-ki* suffixes are generally difficult to comprehend.
The following is an example from the web with two *-ki*'s.

~~~
bizim   ─kinin  ─kiler  alltan    çıktı
We.GEN  ─ki.GEN -ki.PLU below.LOC come_out.PAST
The ones of ours came out from below
(referring to the first teeth of the speaker's baby)
~~~

##### _–lI_

-lI is very productive, but there are also many lexicalized forms.

~~~ sdparse
iki havuz –lu ev \n house with two swimming pools
nummod(havuz, iki)
case(havuz, –lu)
nmod(ev, havuz)
~~~

##### _–lIk_

~~~ sdparse
çantamda iki kitap –lık yer var \n I have space enough/suitable for two books
nummod(kitap, iki)
case(kitap, –lık)
nmod(yer, kitap)
~~~
##### _–sIz_

~~~ sdparse
güclü lider –siz ülkeler \n The countries without a strong leader
amod(lider, güçlü)
case(lider, –siz)
nmod(ülkeler, lider)
~~~

~~~ sdparse
Araba–m –sız yapamam . \ I cannot do without my car
case(Araba–m, –sız)
nmod(yapamam, Araba–m)
~~~

##### _–CA_

~~~ sdparse
bizim bakanlığımız –ca duyruldu \n announced by our ministry
nmod:poss(bakanlığımız, bizim)
case(bakanlığımız, –ca)
nmod(duyruldu, bakanlığımız)
~~~

##### _–CI_

This derives nouns, that are actually the head of the resulting phrase.

~~~ sdparse
kırmızı şarap –çı \n "[red wine] seller" or "person who prefers red wine"
amod(şarap, kırmızı)
nmod(–çı, şarap)
~~~

##### _–lArI_ and _DIr_

These two attach to time expressions.

~~~ sdparse
Sabah –ları kahve içer \n she/he drinks coffee in the morning(s)
case(Sabah, –ları)
nmod:tmod(içer, Sabah)
~~~

~~~ sdparse
Beş yıl –dır çalışmıyor \n He hasn't been working for five years
case(yıl, –dır)
nummod(yıl, Beş)
nmod:tmod(çalışmıyor, yıl)
~~~

#### "zero" derivation from adjective to noun

Adjectives in Turkish can be used as nouns referring to an object
having the property described by the adjective, e.g.,  *Maviyi ver* "give me
the blue one". We currently **do not** split an IG with a zero-morpheme
in these cases. However, we may want to consider this since there are
cases where the modifier modifies the adjective. Here is an example:

~~~ sdparse
En küçük –0 yarışı kazandı . \n The smallest one won the race
advmod(küçük, En)
amod(–0, küçük)
nsubj(kazandı, –0)
~~~

## Compound verbs - light verb constructions

Deciding which usage of a verb is "light" and which one is "heavy" is in general difficult.
In Turkish only verb that (almost) exclusively functions as a light verb seems to be *et-*.
Other verbs always vary in their usage, and probably it is not just a binary decision between light and heavy,
but a scale.
[ On a somewhat related note, the "light" use may become "heavy" in time.
For example, the verb *çek-* "pull" in *faks çek-* "to send fax" looks definitely "light",
but lately, *email çek-* or *SMS çek-* also became common. ]

It seems any any criteria for deciding whether a construction is a compound/light verb, or a "heavy" verb will be arbitrary.
Here are a few I found useful:

* The verbal compound requires an argument with the same case marking
  of the non-verbal part.
* The verb is semantically empty, the rest of the compound (noun or
  adjective) provides the meaning.
* The compound form is (likely to be) found in dictionaries.

In difficult-to-decide cases, I prefer to err on the side of normal
(non-compound/lvc) analysis.

## Arguments/adjuncts, indirect objects

This is another difficult decision during annotation. Some verbs in
Turkish seems to take non-bare or accusative objects. A few examples:

* dative: *Ali'ye inandım* "I believe (in) Ali"
* ablative: *Ali'den hoşlanıyor* "She likes Ali"
* locative: *Ali'de diretiyorlar* "The are insisting on Ali"
* instrumental: *Ali'yle iligileniyor* "She is interested in Ali"

A few seems to take an indirect object besides a bare/accusative one.
All I can think of/find out seems to take dative.  A few (not so
pleasant examples):

* *Ali'yi ölüm-e mahkum ettiler* "They sentenced Ali to death"
* *Ali'yi uyuşturucuy-a itiyor* "He is pushing Ali to drugs"
* *Ali'yi istifa etme-ye zorladılar* "He forced Ali to resign"

The difficulty of deciding whether a accompanying noun phrase is an
argument or an adjunct is amplified by the fact that any argument can
be left out from a Turkish clause as long as it can be recovered from
the (possibly non-linguistic) context. This also results in confusion
in cases with multiple objects, since UD spec suggests that if
sentence has a single argument then it should be marked as `dobj`. In
the sentences with two arguments above, it is more likely leave the
accusative-marked (direct) object out.


The simple guideline seems to be if the meaning of the verb is
"incomplete" without an argument than it should be a direct/indirect
object independent of the case assignment. But the practice is not
that simple. A few additional helpers:

* The arguments can be left out only when they are accessible from the
  context (test suggested
  [here](http://wiki.apertium.org/wiki/Dependency_parsing_for_Turkic#Testing_for_argument_status)).
* The arguments tend to be opaque with respect to the usual function
  of the case marking. For example, there is no direction "to/towards"
  involved in *Ali'<b>ye</b> inandım*.
* Cases of arguments are generally listed in the dictionaries.
* Translating to a language where arguments are obligatory (e.g.,
  English) sometimes helps. If the verb meanings are close, the
  arguments will be obligatory in the translated sentence.

Whichever test/guideline is used, the decision is not always clearcut.
Another reason is the polysemy. For example if *git-* "go" probably
requires a destination argument, but it can as well mean "leave" where
it does not need a destination.

METU-Sabancı treebank seems to make the distinction as well.
The arguments are marked as `OBJECT` regardless of their case, and
adjuncts are marked as `<case>.ADJUNCT`. However, it is quite
inconsistent, indicating the difficulty of the task.

## Clausal subjects with/without external arguments

[this may not be specific to Turkish/Turkic]

Some verbs take clausal subjects that may or may not missing arguments.
In the following, the subject of the clausal subject has to be the
object of the main clause.

~~~
(1) Reddedilmek         Aliyi   çok     üzmüştü.
    refuse.PASS.VN      Ali.ACC very    sadden.PAST
    Having been refused saddened Ali very much.

(2) Sigara_içmek Ali'yi   öldürecek.
    somoke.VN    Ali.ACC  kill-PAST
    Smoking will kill Ali.
~~~

In the following, it is not or it is not necessarily the case.

~~~
(1) Ayşe'nin reddedilmesi     Aliyi   çok     üzmüştü.
    Ayşe.GEN refuse.PASSV.VN  Ali.ACC very    sadden.PAST
    The fact that Ayşe was refused saddened ali very much.

(2) Sigara_içmesi Ali'yi   öldürecek.
    smoke.VN      Ali.ACC  kill-PAST
    The fact that someone (not necessarily Ali) is somoking will kill Ali.
~~~

This may need different subject relations based on the same need one
needs `xcomp`-`ccomp` distincition.

Although with the proposal above,
the second case would be analyzed as normal nominal dependencies.

## References

* Aslı Göksel and Celia Kerslake. Turkish: A Comprehensive Grammar.  London: Routledge, 2005
