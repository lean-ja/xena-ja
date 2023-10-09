# Division by zero in type theory: a FAQ 日本語訳

原文は [Division by zero in type theory: a FAQ](https://xenaproject.wordpress.com/2020/07/05/division-by-zero-in-type-theory-a-faq/) です．

---

## Hey! I heard that Lean thinks 1/0 = 0. Is that true?

Yes. So do Coq and Agda and many other theorem provers.

## Doesn’t that lead to contradictions?

No. It just means that Lean’s $/$ symbol doesn’t mean mathematical division. Let $\mathbb{R}$ denote the real numbers. Let’s define a function $f:\mathbb{R}^2\to\mathbb{R}$ by $f(x,y)=x/y$ if $y\not=0$ and $f(x,0)=0$. Does making that definition give us a contradiction in mathematics? No, of course not! It’s just a definition. Lean uses the symbol $/$ to mean $f$. As does Coq, Agda etc. Lean calls it `real.div` by the way, not $f$.

## But doesn’t that lead to confusion?

It certainly seems to lead to confusion on Twitter. But it doesn’t lead to confusion when doing mathematics in a theorem prover. **Mathematicians don’t divide by 0** and hence in practice they never notice the difference between `real.div` and mathematical division (for which `1/0` is undefined). Indeed, if a mathematician is asking what Lean thinks `1/0` is, one might ask the mathematician why they are even asking, because as we all know, dividing by `0` is not allowed in mathematics, and hence this cannot be relevant to their work. In fact knowing `real.div` is the same as knowing mathematical division; any theorem about one translates into a theorem about the other, so having `real.div` is equivalent to having mathematical division.

## This convention is stupid though!

It gets worse. There’s a subtraction `nat.sub` defined on the natural numbers $\lbrace 0,1,2,\ldots \rbrace$, with notation `x - y`, and it eats two natural numbers and spits out another natural number. If `x` and `y` are terms of type `ℕ` and `x < y`, then `x - y` will be `0`. There’s a function called `real.sqrt` which takes as input a real number, and outputs a real number. If you give it $2$, it outputs $\sqrt{2}$. I don’t know what happens if you give it the input $-1$, beyond the fact that it is guaranteed to output a real number. Maybe it’s $0$. Maybe it’s $1$. Maybe it’s $37$. I don’t care. I am a mathematician, and if I want to take the square root of a negative real number, I won’t use `real.sqrt` because I don’t want an answer in the reals, and the type of `real.sqrt` is `ℝ → ℝ`.

## Why can’t you just do it the sensible way like mathematicians do?

Great question! I tried this in 2017! Turns out it’s really inconvenient in a theorem prover!

Here’s how I learnt Lean. I came at it as a “normal mathematician”, who was thinking about integrating Lean into their undergraduate introduction to proof course. I had no prior experience with theorem provers, and no formal background in programming. As a feasibility study, I tried to use Lean to do all the problem sheets which I was planning on giving the undergraduates. This was back in 2017 when Lean’s maths library was much smaller, and `real.sqrt` did not yet exist. However the basic theory of sups and infs had been formalised, so I defined `real.sqrt x`, for `x` non-negative, to be $Sup\lbrace y\in\mathbb{R} ∣ y^2\leq x\rbrace$, and proved the basic theorems that one would want in an interface for a square root function, such as $\sqrt{ab}=\sqrt{a}\sqrt{b}$ and $\sqrt{a^2}=a$ and $\sqrt{a^2b}=a\sqrt{b}$ and so on (here $a,b$ are non-negative reals, the only reals which my function would accept).

I then set out to prove $\sqrt{2}+\sqrt{3}<\sqrt{10}$, a question on a problem sheet from my course. The students are told not to use a calculator, and asked to find a proof which only uses algebraic manipulations, i.e. the interface for `real.sqrt`. Of course, the way I had set things up, **every time** I used the $\sqrt{\phantom{2}}$ symbol I had to supply a proof that what I was taking the square root of was non-negative. Every time the symbol occurred in my proof. Even if I had proved `2 > 0` on the previous line, I had to prove it again on this line, because this line also had a $\sqrt{2}$ in. Of course the proof is just by `norm_num`, but that was 10 characters which I soon got sick of typing.

I then moaned about this on the Lean chat, was mocked for my silly mathematician conventions, and shown the idiomatic Lean way to do it. The idiomatic way to do it is to allow garbage inputs like negative numbers into your square root function, and return garbage outputs. It is in the **theorems** where one puts the non-negativity hypotheses. For example, the statement of the theorem that $\sqrt{ab}=\sqrt{a}\sqrt{b}$ has the hypotheses that $a,b\geq 0$. Note that it does not also have the hypothesis that $ab\geq 0$, as one can deduce this within the proof and not bother the user with it. This is in contrast to the mathematicians’ approach, where the proof that $ab\geq 0$ would also need to be supplied because it is in some sense part of the $\sqrt{\phantom{2}}$ notation.

## So you’re saying this crazy way is actually better?

No, not really. I’m saying that it is (a) mathematically equivalent to what we mathematicians currently do and (b) simply more convenient when formalising mathematics in dependent type theory.

What actually is a field anyway? For a mathematician, a field is a set $F$ equipped with $0,1,a+b,-a,a\times b,a^{-1}$ where the inversion function $a^{-1}$ is only defined for non-zero $a$. The non-zero elements of a field form a group, so we have axioms such as $x\times x^{-1}=1$ for $x\not=0$ (and this doesn’t even make sense for $x=0$). Let’s say we encountered an alien species, who had also discovered fields, but their set-up involved a function $\iota :F\to F$ instead of our $x^{-1}$. Their $\iota$ was defined, using our notation, by $\iota(x)=x^{-1}$ for $x\not=0$, and $\iota(0)=0$. Their axioms are of course just the same as ours, for example they have $x\times \iota(x)=1$ for $x\not=0$. They have an extra axiom $\iota(0)=0$, but this is no big deal. It’s swings and roundabouts — they define $a/b:=a\times\iota(b)$ and their theorem $(a+b)/c=a/c+b/c$ doesn’t require $c\not=0$, whereas ours does. They are simply using slightly different notation to express the same idea. Their $\iota$ is discontinuous. Ours is not defined everywhere. But there is a canonical isomorphism of categories between our category of fields and theirs. There is no difference mathematically between the two set-ups.

Lean uses the alien species convention. The aliens’ `\iota` is Lean’s `field.inv` , and Lean’s `field.div x y` is defined to be `field.mul (x, field.inv y)`.

## OK so I can see that it can be made to work. Why do I still feel a bit uncomfortable about all this?

It’s probably for the following reason. You are imagining that a computer proof checker will be checking your work, and in particular checking to see if you ever divided by zero, and if you did then you expect it to throw an error saying that your proof is invalid. What you need to internalise is that Lean is just using that function $f$ above, defined by $f(x,y)=x/y$ for $y\not=0$ and $f(x,0)=0$. In particular you cannot prove false things by applying $f$ to an input of the form $(x,0)$, because the way to get a contradiction by dividing by zero and then continuing will involve invoking theorems which are true for mathematical division but which are not true for $f$. For example perhaps a mathematician would say $a/a=1$ is true for all $a$, with the implicit assumption that $a\not=0$ and that this can be inferred from the notation. Lean’s theorem that `real.div a a = 1` is only proved under the assumption that $a\not=0$, so the theorem cannot be invoked if $a=0$. In other words, the problem simply shows up at a different point in the argument. Lean won’t accept your proof of $1=2$ which sneakily divides by $0$ on line 8, but the failure will occur at a different point in the argument. The failure will still however be the assertion that you have a denominator which you have not proved is nonzero. It will simply not occur at the point when you do the division, it will occur at the point where you invoke the theorem which is not true for `real.div`.

Thanks to Jim Propp and Alex Kontorovich for bringing this up on Twitter. I hope that this clarifies things.
