---
layout: base
title:  Subordination in Turkish
permalink: tr/overview/subordination.html
---

# Issues with UD annotation of subordination in Turkish

The primary means of subordination in Turkish is through a set of subordinating 
suffixes that attach to the predicates.
These suffixes may form clauses that function as adverbs, adjectives or nouns.
This type of subordinate clauses behave exactly as the basic word types.
For example,  just like a simple adjective, an adjectival clause precedes the noun they modify,
or a nominal clause takes inflections a simple noun can take and can occupy any slot that a simple noun occupies in a sentence.
Examples (1) and (2) below show an adjective and adjectival clause.

~~~
(1) Kalın   kitap
    thick   book
    "thick book"
(2) Ali'nin  okuduğu         kitap
    Ali.GEN  read-PART.PAST  book
    The book that Ali read/is reading
~~~

In METU-Sabancı (MS) treebank (and in Turkish dependency parsing/annotation work so far), the subordinating suffix introduces a new syntactic unit, "inflectional group" (IG),
and it is also the head of the subordinate clause.
Since this IG is an adjective, it modifies the noun as any other adjective.
In general, the MS treebank considers the last IG as the head of the word.
This is such a basic property for the treebank that
the original format of the treebank relies on this fact implicitly,
leaving the dependencies between the IGs within a word unspecified
(CONLL-X version introduces a "DERIV" relation to cover this, and chains all IGs wehre the head is the last IG).

According to MS treebank, the analysis of example (2) above is as follows
(relation lables are (approximately) translated to UD): 

~~~ sdparse
Ali'nin oku –duğu kitap
deriv(–duğu, oku)
nsubj(oku, Ali'nin)
amod(kitap, –duğu)
~~~

This conlicts with present UD specification because

1. Violates the "primacy of content" principle. The head is the
   subordinating particle/suffix, not the content word.
2. Disregards the separation of clausal/non-clausal modification

I think we can live with the second (here, there seems to be a fundamental inconsistency in UD),
but as I understand,
the first one is quite important for UD to keep things between languages as much
as parallel.

So, an alternative is:

~~~ sdparse
Ali'nin oku –duğu kitap
mark(oku, –duğu)
nsubj(oku, Ali'nin)
acl(kitap, oku)
~~~

In case of adjectivals and adverbials, I would even be happy with not
splitting the subordinating suffix, and signaling its function with some
feature like `VerbForm=Part` (for adverbials `VerbForm=Converb`) or
something similar.

~~~ sdparse
Ali'nin okuduğu kitap
nsubj(okuduğu, Ali'nin)
acl(kitap, okuduğu)
~~~


However, for noun clauses this is not that straightforward.
In these constructions, the clause really behaves like a noun.
Since nouns can get all sorts of inflections, they do too.

A straightforward example is omitting the modified noun of a relative clause in (2) above (Göksel & Kerslake 2005 calls these "headless relative clauses").
This happens quite freely if the object modified can be inferred from the context.

The following is a (probably) comprehensive list of examples that these clauses can be used in parallel with a simple noun:


* Subject:  
   _Kitap güzel._ "the book is nice"  
   _[Ali'nin okuduğu] güzel._ "the one Ali is reading is nice"
* Plural:  
   _Kitap-lar güzel._ "the books are nice"  
   _[Ali'nin okuduk]-ları güzel._ "the ones Ali is reading are nice"
* Object:  
   _Kitab-ı okudum._ "I read the book"  
   _[Ali'nin okuduğu]-nu okudum_ "I read the one Ali is reading"
* Case markers (dative):  
  _Kitab-a bakıyorum_ "I am looking at the book"  
   _[Ali'nin okuduğu]-na bakıyorum_ "I am looking at the one Ali is reading"
* Case + plural:  
   _Kitap-lar-a bakıyorum_ "I am looking at the books"  
   _[Ali'nin okuduk]-lar-ı-na bakıyorum_ "I am looking at the ones Ali is reading"
* Case + postposition:  
   _Kitab-a göre bu yanlış_ "This is wrong according to the book"    
   _[Ali'nin okuduğu]-na göre bu yanlış_ "This is wrong according to the one Ali is reading"
* Another case (ablative):  
   _Kitap-tan parçalar okuduk_ "We read parts from the book"  
   _[Ali'nin okuduğu]-ndan parçalar okuduk_ "We read parts from the one Ali is reading"
* Yet another (genitive in genitive-possessive construction):  
   _Kitap-ın kapağı_ "The cover of the book"  
   _[Ali'nin okuduğu]-nun kapağı_ "The cover of the one Ali is reading"
* Nominal predicate:   
  _Benim en sevdiğim bu kitap_ "The one I like most is this book"
   (yes, two _benim en sevdiğim_  is also an example of the same type of construction)   
   _Benim en sevdiğim [Ali'nin okuduğu_ "The one I like most is the one Ali is reading"

In summary, such phrases can be used wherever a noun can be used, with very few restrictions.
Furthermore, the heads of these phrases are actually the missing noun, or implied pronoun ("the one").

For the above cases, I think we should split the subordinating suffix and mark it as the head of the phrase. 
It also matches with the cases where the noun is not missing,
and one can easily argue that this is "promotion by elision".

~~~ sdparse
Ali'nin oku –duğu kitap düştü . \n The book Ali was reading fell .
nsubj(oku, Ali'nin)
mark(oku, –duğu) 
acl(kitap, oku)
nsubj(düştü, kitap)
~~~

~~~ sdparse
Ali'nin oku –duğu düştü . \n The one Ali was reading fell .
nsubj(oku, Ali'nin)
acl(–duğu, oku) 
nsubj(düştü, –duğu)
~~~


It does not end here. The same sort of nominal clauses may also be used for referring to a more abstract concept, state, fact, way etc.
For example, very same phrase above _Ali'nin okuduğu_ could translate to "the fact that Ali is reading".
And similar to the headless relative clauses discussed above these can be used in all possible contexts a simple noun can be used. 
In these cases, there isn't a missing noun phrase.
It is just how this sort of concepts are expressed in the language.
These constructions include the "adjectival" case discussed above, but there are a few additional varieties.
Here are a few examples (The glosses are intentionally a bit sloppy translations.
They are not the most fluent translations, but highlights the fact that there is a abstract concept involved):

- *Ali'nin oku<b>duğ</b>unu biliyorum* "I know the fact that Ali read/reads"
- *Ali'nin oku<b>ma</b>sını destekliyorum* "I support the fact that Ali reads"
- *Ali'nin oku<b>ma</b>sını destekliyorum* "I support the fact that Ali reads"
- *Ali'nin oku<b>ma</b>sı herkesi şaşırttı* "The fact that Ali reads surprised everyone"
- *Ali'nin oku<b>yuş</b>u sorunlu* "The way Ali reads is problematic"
- *Ali'nin oku<b>ma</b>sı sorunu* "The problem of the way/situation/fact that Ali reads" (-sI nominal compound)
- *Ali'nin okuma<b>ma</b>sının sorumlusu* "The one responsible from of the situation/fact that Ali does not read" (genitive-possessive nominal compound)

In these cases one can always translate the sentence into English with inclusion of an abstract noun like "fact", "way", "state" etc. (although this may not always be the most fluent translation).
So, in a way, these are not a lot different than "headless relative clauses" discussed above.
So, I think we should definitely split the subordinating suffix here,
and mark it as the head of the subordinate clause.

Here are a few example analyses:

~~~ sdparse
Ali'nin oku/VERB –duğunu/NOUN biliyorum
acl(–duğunu, oku)
dobj(biliyorum, –duğunu)
~~~

~~~ sdparse
Ali'nin okuma –masının/NOUN sorumlusu/NOUN
acl(–masının, okuma)
nmod:poss(sorumlusu, –masının)
~~~

~~~ sdparse
Konumuz Ali'nin okuma/VERB –ması/NOUN –ydı/VERB
nsubj(okuma, Ali'nin)
nsubj(–ması, Konumuz)
acl(–ması, okuma)
cop(–ması, –ydı)
~~~

This analysis automatically preserves the parallel with the simple noun analyses.
This is especially important in determining the type of modifier.
If a nominal verb is inflected for case, it would typically function as an adjectival or adverbial modifier.
If we do not mark the subordinating IG as the head,
we would need to use `acl` or `advcl` (`ccomp` only if non-case-marked or accusative case).
On the other hand, a simple noun would be marked as `nmod` here,
relying on the case marking to determine the type of modification.

The only thing we sacrifice is a direct indication that the modification is clausal, 
but the clausal part is already encoded in `acl`.

Still not done. One more case before summing it up.

----------------------------

A particular case of nominal clauses, formed by the suffix _-mAK_ diverge from the nominal clauses discussed above.
Although all other nominal clauses have subjects of their own (or subject agreement on the verb functions as one), 
the clauses formed by _-mAK_ cannot have their own subjects.
The subject is typically a noun phrase (or marked on a verb) in a superordinate clause
(although not always, sometimes it cannot be inferred at all).
These clauses seem to be a good candidate for being analyzed as `xcomp`.
For example:

~~~ sdparse
Ali oku –may–ı seviyor \n Ali likes reading
nsubj(seviyor, Ali)
xcomp(seviyor, oku)
mark(oku, –may–ı)
~~~

However, things can get complicated with these constructions as well.

~~~ sdparse
Başarısız olmasının nedeni yeterince okuma –mak –tı \n The reason for his failure was [not reading] enough
mark(okuma, –mak)
dep(–tı, –mak)
nsubj(–tı, nedeni)
~~~

I do not like this analysis, since it loses the parallel between the other nominal clauses,
and we mark the copula as the head 
(in that case I am nos also sure what `dep` should be, this is a clausal subject complement relation, but could not find it a similar one in UD).

The other alternative is something along the lines of

~~~ sdparse
Başarısız olmasının nedeni yeterince okuma –mak –tı \n The reason for his failure was the fact that he was not reading enough
acl(–mak, okuma)
cop(–mak, –tı)
nsubj(–mak, nedeni)
~~~

### Summary & questions

The conventional dependency annotation of Turkish (always last IG in a word being the head) seems to be in conflict with UD principles.
Resolving this in favor of UD principles seems to be easy for adjectival and adverbial phrases.
However, nominals are tricky, and it seems we will need to split the subordinating constructions resulting in a noun,
and mark them as the head of the subordinate clause that they mark. 

I am quite convinced that above proposal does a good job in analyzing these structures, and it fits into the UD principles fine.
The questions left are the following:

- Should we treat nominal clauses with _-mAK_ different than others.
    - If not, should we introduce a subtype for `acl` to signal that the clause has a missing subject potentially expressed in the higher-level clauses.


## References

* Aslı Göksel and Celia Kerslake. Turkish: A Comprehensive Grammar.  London: Routledge, 2005
