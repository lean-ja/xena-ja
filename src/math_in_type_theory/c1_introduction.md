# イントロダクション

<!-- What is maths? I think it can basically be classified into four types of thing. There are definitions, true/false statements, proofs, and ideas. -->

数学とは何でしょうか？数学は基本的に４つのタイプに分類できると思います．定義，命題，証明，そしてアイデアです．

<!-- **Definitions** (for example the real numbers, or $\pi$) and **true/false statements** (for example the statement of Fermat’s Last Theorem or the statement of the Riemann Hypothesis) are part of the science of mathematics: these are black and white things which have a completely rigorous meaning within some foundational system. -->

**定義** (たとえば実数や $\pi$) や**命題** (たとえばフェルマーの最終定理やリーマン仮説のステートメント) は数学の科学の一部です：こういったものは，ある基礎的なシステムの中で完全に厳密な意味を持ち，白黒はっきりしています．

<!-- **Proofs** are in some sense the currency of mathematics: proofs win prizes. Constructing them is an art, checking them is a science. This explains, very simply, why computer proof verification systems such as Lean, Coq, Isabelle/HOL, Agda… are much better at checking proofs than constructing them. -->

**証明**はある意味で数学の通貨だといえます：証明は賞で報いられます．証明の構築は芸術であり，証明の検証は科学です．このことは，Lean, Coq, Isabelle/HOL, Agda などのコンピュータ証明検証システムが，証明を構築するよりもチェックすることをはるかに得意とする理由を，とても簡単に説明しています．

<!-- And **ideas** are the purely artistic part of mathematics. That “lightbulb” moment, the insight which enables you to solve a problem — this is the elusive mathematical idea. -->

そして**アイデア**は，数学の純粋に芸術的な部分です．「閃き」の瞬間，問題解決を可能にする洞察力，これこそが数学のとらえどころのないアイデアなのです．

<!-- Ideas are the part of mathematics that I understand the least, in a formal sense. Here are two questions: -->

アイデアは，形式的な意味で私が最も理解していない数学の一部です．ここに２つの疑問があります：

<!-- * What is a group? -->
* 群とは何ですか？
<!-- * How do you think about groups? -->
* 群をどう考えていますか？

<!-- The first one is a precise “scientific” question. A group is a set equipped with some extra structure, and which satisfies some axioms. The formal answer is on [Wikipedia’s page on groups](https://en.wikipedia.org/wiki/Group_(mathematics)#Definition). A group is a definition. But the second question is a different kind of question. Different people think about groups in different ways. Say $G$ is a group generated by an element x satisfying $x^5=x^8=1$. What can you say about $G$? If you are a mathematics undergraduate who has just seen the formal definition of a group, you can probably say nothing. If you have a more mature understanding of group theory, you instantly know that this group is trivial, because you have a far more sophisticated model of what is going on. Ideas are complicated, and human-dependent. A computer’s idea of what a group is, is literally a copy of the definition in Wikipedia, and this is one of the reasons that computers are currently bad at proving new theorems by themselves. You can develop a computer’s intuition by teaching it theorems about groups, or teaching it examples of groups, or trying to write AI’s which figure out group theory theorems or examples of groups automatically. But intuition is a very subtle thing, and I do not understand it at all well, so I will say no more about these ideas here. I think that the concept of a map being “canonical” is an idea rather than a definition — I think different mathematicians have different ways of thinking about this weasel word. In this post I’m going to talk about how the three other concepts are implemented in type theory, in the Lean theorem prover. -->

最初のものは正確な「科学的な」質問です．群とは，ある特別な構造を持ち，ある公理を満たす集合のことです．正式な答えは [Wikipediaの群についてのページ](https://en.wikipedia.org/wiki/Group_(mathematics)#Definition)にあります．群とは定義です．しかし，２つめの質問は種類の異なる質問です．群について考える方法は人によって異なります．$G$ が $x^5=x^8=1$ を満たす要素 $x$ によって生成される群だとします．$G$ について何が言えるでしょうか？もしあなたが数学の学部生で，群の形式的な定義を見たばかりなら，おそらく何も言えないでしょう．群論をより深く理解している人なら，即座にこの群が自明であることがわかるでしょう．何が起こっているのかについて，はるかに洗練されたモデルを有しているからです．アイデアは複雑で，人によって異なるものです．「群とは何であるか」という問いについてのコンピュータの理解は，文字通りウィキペディアの定義のコピーです．現在コンピュータは新しい定理を自力で証明することを苦手としていますが，その理由のひとつがこれです．コンピュータに群に関する定理を教えたり，群の例を教えたり，群論の定理や群の例を自動的に理解する AI を作ったりすることで，コンピュータの直感を発達させることができます．しかし直感というのはとても微妙なもので，私にはまったく理解できないので，ここではこういったアイデアについてはこれ以上語らないことにします．写像が canonical (正準) であるという概念は，定義というよりむしろアイデアだと思います．数学者によって，このイタチごっこのような言葉に対する考え方は違ってくるでしょう．この記事では，他の３つの概念 (定義，命題，証明) が型理論や Lean theorem proverでどのように実装されているかについてお話しようと思います．
