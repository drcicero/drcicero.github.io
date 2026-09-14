---
title: Research Projects
layout: single
weight: 20
---

An overview of my research grouped by topic. See [Research](/research/) for the full chronological list of publications and preprints, and [Teaching](/teaching/) for all courses and supervised theses.

{{< topic icon="tabler:outline/topology-star-3" title="Choreographic & Distributed Programming" motivation="Write a whole distributed system — a smart contract, a tierless web app — as one script, and let the compiler split it up." >}}

{{< background >}}

{{< concept icon="tabler:outline/message-2" title="Alice-and-Bob notation" >}}
A shorthand from cryptography and protocol design for describing who sends what to whom, one step at a time, before worrying about how it is implemented.

```
Alice -> Bob: "order(item)"
Bob -> Alice: "confirmation(id)"
```
{{< /concept >}}

{{< concept icon="tabler:outline/git-merge" title="CRDTs" >}}
Conflict-free Replicated Data Types are data structures that several copies can update independently — e.g. offline — and later merge automatically, always landing on the same result. That guarantee follows from three simple algebraic laws `merge` must obey.

```
merge : State x State -> State

merge(a, b)           = merge(b, a)             -- commutative: order of merging doesn't matter
merge(a, merge(b, c)) = merge(merge(a, b), c)   -- associative: grouping doesn't matter
merge(a, a)           = a                       -- idempotent: merging twice changes nothing
```
{{< /concept >}}

{{< /background >}}

{{< topicbody >}}
- Publications:
  - _Mechanizing Choreographic Programs and Hoare Logic with State Transformers_, TyDe 2026
  - _On Eliminating the Impossible with Dependent Types: Choreographic Libraries with Proof-Carrying Located Values_, TyDe 2026
  - _Prisma: A tierless language for enforcing contract-client protocols in decentralized apps_, TOPLAS 2023 / ECOOP 2022
  - _Multiparty Languages: the Choreographic and Multitier Cases (Pearl)_, ECOOP 2021 🏅
- Preprints:
  - _Choreographies First, Session Types Later: Decoupling Deadlock-Freedom from Endpoint Projection_
- Theses supervised:
  - MSc 2023/24, Higher Order Functional Choreographies in Lean4, Simon *Daniel*
  - BSc 2020, Functional and Reactive Programming for Smart Contracts, Stefan *Sauer*
  - BSc 2019/20, Reactive Programming for Smart Contracts, Fabio d'Aquino *Hilt*
- PhD students: Simon Daniel
{{< /topicbody >}}

{{< /topic >}}

{{< topic icon="tabler:outline/refresh" title="Incremental & Reactive Programming" motivation="Compilers that turn plain functional code into computations that redo only what changed — from live UIs to build systems." >}}

{{< background >}}

{{< concept icon="tabler:outline/topology-full" title="Dependent Moore machines" >}}
A Moore machine is a state machine whose output depends only on its current state. Making it dependent lets the type of the next input change with the state — e.g. once a protocol is "closed", the type system won't let you send another message.

```
state Open   : NextMsg = Send(_) | Close
state Closed : NextMsg = Void        -- nothing left to send

step(Open, Close) = Closed
```
{{< /concept >}}

{{< concept icon="tabler:outline/refresh" title="Incremental programming" >}}
Instead of recomputing a result from scratch whenever the input changes a little, an incremental program derives a small update directly from the old result and the change — much faster for large, slowly-changing data.

```
class Incremental (A B A' B' : Type) (f : A → B) where
  Cache : Type
  init  : A → B × Cache
  step  : A' → Cache → B' × Cache
  -- law: replaying a change through `step` must match
  -- recomputing `f` from scratch on the whole updated input
  correct : ∀ a a' c, (init a).snd = c →
    (step a' c).fst = f (a.patch a')
```
{{< /concept >}}

{{< /background >}}

{{< topicbody >}}
- Publications:
  - _DeCo: A Core Calculus for Incremental Functional Programming with Generic Data Types_, OOPSLA 2026
  - _Incrementalizing Polynomial Functors_, FTFJP 2024
  - _Turning Unobservable into Unreachable: Dynamic Reactive Programming without Leaks_, REBLS 2019
  - _From Debugging Towards Live Tuning of Reactive Applications_, LIVE 2018
- Courses:
  - 6CP Project *IMPL* (Implementation of Modern Programming Languages)
  - 3CP Seminar *DaIMPL* (Design and Implementation of Programming Languages)
- PhD students: Timon Böhler
- Projects: [drx](https://github.com/drcicero/drx), dynamic reactive programming without memory-leaks; involved with REScala
{{< /topicbody >}}

{{< /topic >}}

{{< topic icon="tabler:outline/atom-2" title="Advanced Type Systems & Mechanized Proofs" motivation="Types catch bugs before a program runs, and let languages say exactly what they mean." >}}

{{< background >}}

{{< concept icon="tabler:outline/certificate" title="Proofs by Curry-Howard" >}}
The Curry-Howard correspondence says "propositions are types, and proofs are programs": a mathematical statement becomes a type, and a proof of it becomes a program of that type. Type-checking and proof-checking become the same activity.

```
theorem fst : A and B -> A
proof fst (a, b) = a   -- this program IS the proof
```
{{< /concept >}}

{{< concept icon="tabler:outline/git-branch" title="Dependent pattern matching" >}}
In a dependently-typed language, matching on one value can tell the type-checker facts about later ones — e.g. matching a length-indexed list against "non-empty" proves there is a first element, ruling out empty-list errors at compile time.

```
head : Vec (n+1) A -> A
head (x :: xs) = x   -- only the non-empty pattern type-checks
```
{{< /concept >}}

{{< /background >}}

{{< topicbody >}}
- Publications:
  - _Extended Abstract: From Pattern Unification Towards Pattern Matching Unification_, TyDe 2026
  - _A Direct-Style Effect Notation for Sequential and Parallel Programs_, ECOOP 2023 🏅
- Courses:
  - 6CP Lecture *TYPES* (Type Systems)
- Theses supervised:
  - BSc 2026, Implementation of a Dependently-Typed Programming Language, Robin *Gutroff*
  - BSc 2025/26, Monomorphization for System F, Iurii *Khosoi*
  - BSc 2023/24, Connecting Automata- and Semantics-based Program Synthesis, Matthias *Conrad*
{{< /topicbody >}}

{{< /topic >}}

{{< topic icon="tabler:outline/math-function" title="Differentiable Programming & Neuro-Symbolic Learning" motivation="Compilers that differentiate and vectorize functional code, paired with neural networks that guess a law's shape while symbolic search fills in the details." >}}

{{< background >}}

{{< concept icon="tabler:outline/grid-dots" title="Array programming" >}}
An array of shape `S` holding elements of type `A` is really just a function from indices to values — `map`/`zip`/`reduce` over the whole array replace hand-written loops.

```
def Arr (S A : Type) := S → A          -- an array IS a function

class ArrayOps (S A B : Type) where
  get : Arr S A → S → A                -- get = function application
  map : (A → B) → Arr S A → Arr S B    -- map = function composition

instance : ArrayOps S A B where
  get arr i := arr i
  map f arr := f ∘ arr                 -- no loop needed
```
{{< /concept >}}

{{< /background >}}

{{< topicbody >}}
- Publications:
  - _Prompting Neural-Guided Equation Discovery Based on Residuals_, DS 2025
  - _Compiling with Arrays_, ECOOP 2024 🏅
  - _Using Rewrite Strategies for Efficient Functional Automatic Differentiation_, FTfJP 2023
- Preprints:
  - _Neural-Guided Equation Discovery_
  - _Verified Search For Inverse Functions, with an Application to Normalizing Flows_
- Theses supervised:
  - BSc 2025, AiNF - Automatic Differentation, Optimization and Codegeneration, Julius *Schuchert*
  - BSc 2025, Optimization of Automatically Differentiated Programs via Partial Redundancy Elimination, Jan *Groen*
  - MSc 2024, Probabilistic Programming with Holonomic Functions, Manuel *Adam*
  - MSc 2023, An Optimizing Compiler for a Differentiable Array Programming Language, Timon *Böhler*
  - MSc 2022/23, Type Inference for Tractable Probabilistic Programming, Frank *Pfirmann*
  - MSc 2022, [Towards an End-to-End Neuro-symbolic DSL of Transformers](https://tuprints.ulb.tu-darmstadt.de/entities/publication/ae67651b-ecad-4721-addd-93166f2ceb45), Daniel **Manninger**
  - BSc 2021/22, Differential Programming in an Array Language, Timon *Böhler*
  - BSc 2021/22, Comparing Implementation Strategies for Differentiable Programming, Daniel *Stricker*
- PhD students: Timon Böhler, Benedict Smit
{{< /topicbody >}}

{{< /topic >}}
