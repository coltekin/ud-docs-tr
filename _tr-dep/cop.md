---
layout: relation
title: 'cop'
shortdef: 'copula'
---

A copula is the relation between a subject complement and a copular verb or copular suffix.
We always mark copula as dependent of the subject complement.

In Turkish, the auxiliary verb _ol-_ 
and in some constructions the negative particle _değil_ act like a free copula.
The main means of forming copular constructions, however, is through
the bound morpheme _-(y)_,
and (infrequently) its clitic form _i-_.
Since the morpheme _-(y)_ consists only of a "buffer" consonant,
in many morphological contexts, it is not realized.

Copular morphemes carry features, e.g., [Number](tr-feat/Number), [Person](tr-feat/Person),
that may conflict with the complement they are attached to.
Furthermore, the copular suffixes can also attach to verbal nouns,
causing conflicting dependency relations besides more feature conflicts.
As a result, all copular markers, 
including the "zero copula" are considered as a separate syntactic tokens.

~~~ sdparse
Güzel olacak . \n (He\/she\/it) will be beautiful
cop(Güzel, olacak)
~~~

~~~ sdparse
Güzel idi . (He\/she\/it) was beautiful
cop(Güzel, idi)
~~~

~~~ sdparse
Güzel –di . (He\/she\/it) was beautiful
cop(Güzel, –di)
~~~

~~~ sdparse
Güzel –im . \n I am beautiful
cop(Güzel, –im)
~~~

~~~ sdparse
Güzel –0 . \n He\/she\/it is beautiful (–0 represents the zero morpheme)
cop(Güzel, –0)
~~~

When an overt subject is present,
it is headed by the subject complement (not the copula).

~~~ sdparse
Kitap güzel –0 . \n The book is nice\/beautiful 
cop(güzel, –0)
nsubj(güzel, Kitap)
~~~

~~~ sdparse
Kitap güzel olacak . \n The book will be nice\/beautiful
cop(güzel, olacak)
nsubj(güzel, Kitap)
~~~

### Why do we split copular suffixes?

The copular suffixes carry subject-verb agreement as well as a few other verbal features.
These features may conflict with the features of the noun they are attached to.
In the following example, the noun _arabaları_ is plural,
although the predicate formed by the copular suffix has no number marking,
hence defaulting to singular subject-predicate agreement
(Hoover over the POS labels to see the relevant features).
As a result, the complete word _arabalarıydı_ carries both `Number=Plur` and `Number=Sing` features.

~~~ conllu
# His/her passion was racing cars
1	Tutkusu	tutku	NOUN	NOUN	Number=Sing	3	nsubj
2	yarış	yarış	NOUN	NOUN	Number=Sing	3	nmod:poss
3	arabaları	araba	NOUN	NOUN	Number=Plur	0	root
4	–ydı	_	VERB	VERB	Number=Sing|Person=3|Tense=Past	3	cop
~~~

The issue is more pronounced if the copular suffix is attached to a nominalized verb.
In the following example, the single word _konuşmamasıydı_ includes two verbal syntactic units,
the subordinated content verb _konuş_ "to speak" and the copular suffix _-ydı_.
Besides more features conflicts (e.g., `Tense`),
both predicates have their own subjects. 
Hence, requiring separate syntactic units.

~~~ conllu
# The problem was (the fact that) the parties did not talk
1	Sorun	sorun	NOUN	NOUN	_	4	nsubj
2	trafların	traf	NOUN	NOUN	_	3	nsubj
3	konuşmaması	konuş	VERB	VERB	Tense=Pres	4	acl
4	–ması	_	NOUN	NOUN	_	0	root
5	–ydı	_	VERB	VERB	Tense=Past	4	cop
~~~

#### Why zero-copula?

UD has a clear stand against zero elements.
However, as explained above, the copular suffix in Turkish may be dropped in some morpho-phonological contexts.
For example, the present tense counterpart of the above example sentence (_Tutkusu yarış arabalarıydı_ "His passion was racing cars") would be _Tutkusu yarış arabaları_ "His passion is racing cars".
We analyze present version of the sentence similar to past one:

~~~ conllu
# His/her passion was racing cars
1	Tutkusu	tutku	NOUN	NOUN	Number=Sing	3	nsubj
2	yarış	yarış	NOUN	NOUN	Number=Sing	3	nmod:poss
3	arabaları	araba	NOUN	NOUN	Number=Plur	0	root
4	_	_	VERB	VERB	Number=Sing|Person=3|Tense=Pres	3	cop
~~~

Although the subject complement _arabalar_ (cars) carries the plural
feature,
the resulting predicate agrees with a singular subject.
Without the additional "zero-unit",
there is no (clear) way to specify that it is fine for the predicate to have a
singular subject.
