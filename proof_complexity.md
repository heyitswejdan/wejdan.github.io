---
layout: article
title: "Proof complexity, informally"
author: Mosaad Al Thokair
date: 2021-05-18
excerpt: "A fun excursion through the hills of proof systems"
image: "/assets/images/proofcomplexity/featured-image.png"
---

* Table of Contents
{:toc}
# Meta-matters

## Why am I writing this article?

I think proof complexity is cool and I would like to share it with as many people as possible. Towards that end, I will optimize for simplicity while writing in the hope that this 1) will remove the barriers for getting into the subject and 2) will give motivation for those who might find this topic interesting to read more about it. 
And for the sake of complete honesty, I also have an ulterior motive: I want to make sure that I understand proof complexity; and I believe that a well-written article is an easily verifiable certificate that the author understands what they are writing about. So... yeah.

## Why would you care about proof complexity?

I can think of three layers to the answer of this question: the practical, the theoretical, and the philosophical. I will not say much about the practical importance of proof complexity (e.g. its role in SAT solvers which are used in different industries today everywhere from hardware design and verification to scheduling and planning) since this is not an interest of mine. Rather, I will briefly mention its theoretical and philosophical importance.

Although the questions of proof complexity are interesting on their own, it was from the start suggested as an approach towards the [million-dollar problem](https://www.claymath.org/millennium-problems) P vs. NP. However, this is not the only connection to computational complexity. As we will see, close connections between proof complexity and circuit complexity have been established. Understanding proof coomplexity has the potential to illuminate our understanding of complexity theory on multiple levels (P vs. NP being the ultimate one).

Personally, I think the close connection of proof complexity to the notion of "proof", and therefore to the heart of mathematical thinking, gives it a certain kind of potential to say something interesting about the language of mathematics, the human mind (that created/discovered mathematics), and the nature of knowledge (even if we quantiyf knowledge to be that of mathematics). Of course, like any other intellectual endavor, proof complexity might not live up to all of this potential but such is the risk of taking any intellectual adventure. One just has to make sure that they are having fun during these adventures.

I will end this section with a couple of quotes I found loosely connected to the last point mentioned:

> Mathematics can be seen as a special kind of language developed for the world of virtual objects like points and lines, numbers and equations, sets, spaces, algebras and other occupants of this mathematical world and for our thoughts about them. This language is quite powerful and is akin in many respects to natural languages. And just as a talented poet can describe in few lines or even in few words a thought or a situation for which you or I may need volumes, miraculously short and eloquent proofs which nobody would have dreamt of may exist. I think that at present we have only a tentative understanding of the power of the language of mathematics. 
>
> **Proof Complexity** by Jan Krajicek

> understanding our capacities for mathematical thought is an important part of understanding our capacities to think philosophically.
>
> **[Principia](https://aeon.co/essays/does-philosophy-still-need-mathematics-and-vice-versa)** by Jeremey Avigad

## History and pre-requisites

I will skip the very interesting history and story of the development of the theory of computation because other people have told it beautifully. If you are completely new to this subject, I would suggest reading [The Annotated Turing](https://www.goodreads.com/en/book/show/2333956) by Charles Petzold and [Gödel, Escher, Bach](https://www.goodreads.com/book/show/24113.G_del_Escher_Bach) by Douglas Hofstadter. 

There are also some pre-requisites that I think are going to increase your appreciation of the subject, but I will go over them whenever needed, which include: basic understanding of propositional logic, turing machines, and popular complexity classes. These are fascinating topics on their own. If you haven't already, I would encourage you to explore them.

# Ok, but what *is* proof complexity?

We will start with the paper that started it all, the 1987 Cook-Reckhow paper. But first, consider the following two scenarios:

It is easy to convince your friend that a given (from now on, propositional)<!---[footnote: what does 'propositional' mean?]---> formula is not a tautology <!---(footnote: tautology definition)--->. Simply give them an assignment that evaluates the formula to *false*. Finding this assignment might be hard, but once you find it, they have no reason not to believe you. Any set (language) that has the property that you can easily verify the refutation of membership claims is in the class of problems called $$coNP$$. The set of tautologies $$TAUT$$, then, is in $$coNP$$. 

On the other hand, how easy is it to convince your friend that a formula *is* a tautology? We convince one another of the correctness of our claims via *proofs*. So the question becomes how easy is it for your friend to understand a proof you present? If you present a very long proof that would surely make it harder to understand. Persuasion and understanding are fuzzy concepts, but we can use the well-established concepts of computational resources needed to verify the proof's correctness. Similar to $$coNP$$, $$NP$$ is defined as the class of languages that have an easily verifiable proof of membership. The main question of proof complexity is whether $$TAUT \in NP$$.

Cook and Reckhow laid out a plan to answer this question in 1987:

> The question of whether TAUT is in NP is equivalent to whether there is a propositional proof system in which every tautology has a short proof, provided "proof system" and "short" are properly defined. - Cook-Reckhow

So the question of whether $$TAUT \in NP$$ is now 'reduced' to whether there is a "proof system in which every tautology has a short proof".

Before discussing strategy, progress, and open questions, what are the implications of answering the above question?

- If there is no proof system that can prove tautologies efficiently, then $$TAUT \not \in NP$$ which means that $$NP \not= coNP$$, which implies that $$P \not= NP$$ <!---[footnote: you can see that because $$P = coP$$]---> and we're done.

- If $$TAUT \in NP$$ and $$NP = coNP$$, then first of all we have short proofs, which is fantastic, but this also implies a lot of interesting things.. e.g. the "polynomial heirarchy" $$PH$$ collapses and we have $$NP = PH$$ which implies $$BPP \subseteq NP$$ (Sipser Lautemann) i.e. any problem solved efficiently using a randomized algorithm can be easily verified.

The authors conjecture that an efficient proof system does not exist. To prove this conjecture, we would have to prove that for *every* proof system there is at least one formula that has no short proof. This is a monumental task, but one thing is sure: we first have to prove lower bounds for weak proof systems and work our way upwards. A central question in proof complexity is whether a very natural class of proof systems called Frege has super-polynomial lower bounds. But even the answer to this question is hard.

So this is what proof complexity is about: studying the efficiency of poropositional proof systems. 

In the following sections we will explore some of the fundamental concepts laid down by the Cook-Reckhow paper, progress made towards proving lower bounds since then, and current research directions. But before all of that, let's look at a concrete example of what a tautology looks like.

# The pigeonhole principle: what a tautology looks like

One of the most studied tautologies in proof complexity is the pigeonhole principle. The reason for its popularity is that it was used to prove lower bounds for a number of proof systems as we will see later.

The pigeonhole principle states <!---[footnote: there are many variant of the pigeonhole principle, where you can vary the number of pigeons and number of holes.. but for simplicity, we will just consider the variant where the number of pigeons is exactly one more the number of holes.. this is the hardest variant]--->  that if you have $$n$$ holes and $$n+1$$ pigeons, and each pigeon sits in exacly one hole, then there must be a hole that at least two pigeons share. Obvious, isn't it?

To express this formally in propositional logic:

* let $$Q_{ij}$$ where $$i \in \{1, ..., n+1\}$$ and $$j \in \{1, ..., n\}$$, express the proposition that pigeon $$i$$ sits in hole $$j$$.
* Let $$PIGEONS_n$$ be the subformula: Each pigeon is in at least one hole (for simplicity, instead of saying exactly one hole): $$\bigwedge\limits_i \bigvee\limits_j Q_{ij}$$ <!---[footnote: this means $$(Q_{11} \lor Q_{12} \lor \ldots ) \land (Q_{21} \lor Q_{22} \lor \ldots) \land \ldots$$]--->
* and $$HOLES_n$$ the subformula: Each hole contains only one pigeon: $$\bigwedge\limits_{j}\bigwedge\limits_{i_1 \not= i_2} \lnot (Q_{i_1j} \land Q_{i_2j})$$
* Then, $$PHP_n := \lnot (PIGEONS_n \land HOLES_n)$$

The pigeonhole principle is known to be a hard tautology for some proof systems (i.e. it has no short proofs). One of these systems is the Resolution proof system. Before discussing Resolution, let's explore the notions of proof size, and proof system efficiency. 

# Proof size, lower bounds, and simulations: is there an optimal proof system?


The concept of 'proof size' is the generalization of the number of lines/steps in a proof since not all proof systems have "lines" or use "steps" (as we will see, in the Ideal Proof System the proof size is the size of the circuit computing some polynomial). 

We have a super-polynomial lower bounds for a tautology in a proof system if there is no proof (in that proof system) whose length is bounded by a polynomial (i.e. the shortest proof we can hope to get grows faster than any polynomial). 

We measure the efficiency of proof systems by proof size. One of the achievements of the Cook-Reckhow paper is that it established a framework of comparing different proof systems: simulations.

A proof system $$A$$  polynomially simulates another proof system $$B$$ if there is a way to translate every proof in $$B$$ to a proof in $$A$$ while maintaining a polynomial blow-up in the proof size. Sometimes, two proof systems simulate each other. In that case, we say that the two proof systems are polynomially equivalent. 

From this, we can see a semi-ordering of proof systems. A question posed by J. Krajíček and P. Pudlák (1989) <!---proper citation---> is whether this chain of proof systems end in an optimal proof system, i.e. a proof system that polynomially simulates every other proof system. This is still an open question. However, Krajíček and Pudlák were able to show that if $$EXP = NEXP$$ then such a proof system exists (notice that this is a sufficient but not a necessary condition).

The landscape of proof systems is starting to shape up, but there is still a lot of mysteries. In what follows, we will go in a brief journey through the "hills" of proof systems. We will start with the weaker (less efficient) ones with a known lower bound, and then progressively move towards more sophisticated proof systems.

# Known lower bounds: the Resolution proof system

Below is the tl;dr for this section.

> Haken [23] showed super-polynomial  lower  bounds  on  the  size  of  proofs  of  the Pigeonhole Principle in the Resolution proof system. Ajtai [3] obtained a significant extension of this result by showing that the Pigeonhole Principle is also super-polynomially hard for Bounded-Depth Frege - a restricted version of the standard Frege system where the lines are bounded-depth circuits. 
>
> — **Why are Proof Complexity Lower Bounds Hard?** Jan Pich and Rahul Santhanam

<!--- appropriately import ciations--->

The idea behind the resolution proof is that it shows that the negation of the tautology is not satisfiable (i.e. always false under any assignment). The only inference rule used is  the 'resolution rule', which states that given two clauses $$(\alpha \lor \beta)$$ and $$(\lnot \beta \lor \gamma)$$ you can conclude $$(\alpha \lor \gamma)$$, where $$\alpha, \beta, \gamma$$ are either propositions or formulas. A proof is just repeated application of this rule on the [CNF](https://en.wikipedia.org/wiki/Conjunctive_normal_form) of the negated tautology until the empty clause is reached. 

The amazing thing about this strategy is that reaching the empty clause is *guaranteed* when you start with a tautology (but the number of steps can be huge). Furthermore, if the formula you started with is not a tautology you will reach a point where you cannot apply the resolution rule anymore. Because of this and other practical properties, the resolution proof system is widely used in SAT solvers, verification, automated theorem proving systems and other areas.
<!---[Link to papers about the topic.. interesting results.. etc..]--->
As mentioned above, Haken proved that the pigeonhole principle does not have a polynomial size proof in Resolution. The result of Haken has been extended by Ajtai to show that even bounded-depth Frege systems don't have a "short" proof for the pigeonhole principle. So what is bounded-depth Frege? To know this, we have to first know the more general Frege proof systems.

# Frege-style systems

A Frege system is the one you're most familiar with: any set of sound <!---[explain]---> and implicationally complete <!---[footnote: explain. i.e. every true formula can be proven using the axioms and inference rules. ]---> axioms and inference rules (e.g. modus ponens: if $$A$$, and $$A \implies B$$; then $$B$$) <!---[example for set of axioms and inference rules from wikipedia]--->. Cook and Reckhow showed that all Frege-style proof systems are polynomially equivalent regardless of the axioms and inference rules chosen. 

A Frege proof is one where 1) each line is an axiom, or a derivation from a previous line by one of the inference rules and 2) the last line ends in the formula you want to prove. 

Due to the naturalness of this proof system, proving super-polynomial lower bounds for Frege has become the central question of proof complexity.  It is also believed that obtaining these lower bounds might illuminate our understanding of some of the complexity classes like $$NC^1$$ and P/poly. <!---[although historically, lower bounds for circuit complexity always precede proof system lower bounds]--->. Furthermore, we cannot hope to progress in the Cook-Reckhow program before achieving these lower bounds. 

Although Frege systems are polynomially equivalent regardless of axioms and inference rules chosen, restricting the kind of formulas that can show up in the proof generates new proof systems with varying efficiency. 

## Bounded-depth Frege

One variant of Frege is the bounded-depth Frege proof system. In this proof system, proofs are allowed to have *fixed depth* formulas only <!---[footnote: fixed depth formula example]--->. As mentioned previously, Ajtai proved super-polynomial lower bounds for bounded-depth Frege systems, which were later improved by [Krajicek, Pudlak, and Woods 1995 and Pitassi, Beame, Impagliazzo 1993...] to truly exponential lower bounds. <!---proper citations--->

## $$AC^0[p]$$-Frege

Another variant of Frege is the $$AC^0[p]$$-Frege, where formulas in a proof are allowed to look like $$AC^0[p]$$ circuits, i.e. constand depth $$d$$, unbounded fan-in (i.e. its operators take an arbitrary number of input bits), and in addition to the classic logical gates we have $$\mod p$$ gates for a fixed prime $$p$$ with additional axioms related to the $$\mod p$$ gates. Apparently, adding the mod gates adds some power to the proof system. However, no lower bounds are known for this proof system (although the corresponding lower bounds in circuit complexity were obtained independently by Razborov and Smolensky in 1987). $$AC^0[p]$$-Frege is a very interesting proof system to try and get lower bounds for because 1) it can illuminate barriers, 2) its lower bounds are believed to be more feasible than the unrestricted Frege; and 3) exponential lower bounds for it yield super-polynomial lower bounds for unrestricted Frege (Filmus, Pitassi, and Santhanam 2011).

## Extended Frege

Just as there are variants of Frege that are less efficient, there are ones that are blieved to be more efficient. Extended Frege is the Frege proof system augmented with the Extension Axiom, which allows us to re-use intermidary formulas in a proof to get Extended Frege. Formulas that occur in a Frege proof look like $$NC^1$$ circuits, while in Extended Frege they look like P/poly circuits.

The landscape of proof systems is not limited to Frege systems. There are other types of proof systems like algebraic, and geometric proof systems, among others. One proof system in particular I found very interesting is the cleverly named Ideal Proof System. So let's take a closer look at it.

# Algebraic proof systems

Grochow and Pitassi (2018) <!---proper citation---> proposed the Ideal Proof System (IPS) that established strong connections between circuit complexity and proof complexity while hoping to utilize algebraic geometry tools to establish lower bounds for both. Proofs in IPS are algebraic circuits. Lower bounds for IPS imply lower bounds for Frege and even Extended Frege. A central theorem used in this proof system is the Hilbert's Nullstellensatz, which is a beautiful theorem in algebraic geomtry <!---[link to the book 'invitation to algebraic geometry']---> that is at the heart of a class of algebraic proof systems. By translating logical formulas into polynomials, the algebraic proof systems take advantage of this powerful theorem. 

## Nullstellensatz, simplified
Given a set of mult-variable polynomials$$f_1, f_2, \ldots, f_n$$ <!---[$$p(\vec{x}) = x_1^2 + x_2^5 - x_3$$ is an example of a multi-variable polynomial]--->, the set does not have a common root $$\vec{x}$$ iff . there is a set of polynomials $$g_1, g_2, .., g_n$$ such that $$\sum{f_i g_i} = 1$$. 

## IPS

The idea of IPS is to translate a propositional formula to a set of polynomials in such a way that the logical formula is satisfiable iff the corresponding polynomials have a common solution (i.e. $$p_1(\vec{x}) = p_2(\vec{x}) = ... = 0$$ for some $$\vec{x}$$.) Below is the explicit translation used:

$$tr(x) = 1-x$$
$$tr(\lnot A) = 1 - tr(A)$$
$$tr(A \lor B) = tr(A)tr(B)$$

Notice that we are not giving a translation for 'and' because we are only interested in translating clauses of a CNF formula. Each clause will be a separate polynomial.

Here is how we prove that a formula $$\tau$$ is a tautology in IPS: 

- Negate $$\tau$$ and convert it to CNF
- Translate the CNF clauses to a set of polynomials as mentioned above
- Notice that $$\tau$$ is a tautology iff its negation is unsatisfiable, by design (of the translation) this happens iff the set of polynomials do not have a common root, by Nullstellensatz this happens iff there is a $$g_1, g_2, \ldots, g_m$$ such that  $$\sum{f_i g_i} = 1$$.
- The set of polynomials $$g_1, g_2, \ldots, g_m$$ *is the proof*. To capture this, IPS requires coming up with a circuit $C$ such that:
  - $$C(\vec{x}, 0) = 0$$ (this makes sure that the polynomial expressed by $$C$$ is of the form $$\sum f_ig_i$$)
  - $$C(\vec{x}, f_1(\vec{x}), f_2(\vec{x}), \ldots, f_m(\vec{x})) = 1$$.

The size of the proof is the size of the circuit $$C$$.  

A few interesting facts about IPS proved in the Grochow, Pitassi paper:

IPS polynomially simulates Extended Frege (and transitively, $$AC^0[p]$$, and Frege). This means that proving super-polynomial lower bounds for IPS imply super-polynomial lower bounds for Extended Frege.

If PIT axioms have p-bounded proofs, then AC^0[p]-Frege is equivalent to IPS, Frege, and Extended Frege, which can explain the apparent difficulty of proving super-polynomial lower bounds for it.

> Theorem 1.2 (Brief; see Section 4 for the full statement). Super-polynomial lower bounds for the Ideal Proof System imply that the permanent does not have polynomial-size algebraic circuits, that is, $$VNP\not=VP$$ (a long-standing and impactful problem).
>
> **Circuit complexity, proof complexity, and polynomial identity testing: The ideal proof system** by Joshua Grochow, and Toniann Pitassi

# Final remarks

Two questions excite me when talking about proof complexity:

1- Why is proving super-polynomial lower bounds for $$AC^0[p]$$-Frege taking more than 30 years after proving the corresponding lower bounds for circuit complexity? We don't even have barrier results like relativization or natural proofs that rule out a class of techniques/approaches. 

2- Sufficienct conditions have been established (and later improved) for the existence of "optimal" proof systems. Can we (dis)prove this unconditionally?