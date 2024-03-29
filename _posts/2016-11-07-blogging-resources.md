---
layout: post
title: Blogging Resources
categories:
    - 未分类
tags:
    - 未分类
---

* TOC
{:toc}

# Resources
1. git commands manual: <http://marklodato.github.io/visual-git-guide/index-zh-cn.html>
1. markdown manual: <http://www.appinn.com/markdown/>
1. kramdown
   - quickref/examples: <http://kramdown.gettalong.org/quickref.html>
   - syntax/manual: <http://kramdown.gettalong.org/syntax.html>
   - vs. regular markdown: <http://gohom.win/2015/11/06/Kramdown-note/>
1. gitment
   - <https://zhangquan1995.github.io/2016/03/07/Jekyll%E5%8D%9A%E5%AE%A2%E6%B7%BB%E5%8A%A0Gitment%E8%AF%84%E8%AE%BA%E7%B3%BB%E7%BB%9F/>
   - <http://xichen.pub/2018/01/31/2018-01-31-gitment/>
   - <http://www.xjdesyxx.top/2018/02/07/errsln/>
   - <https://extremegtr.github.io/2017/09/07/Add-Gitment-comment-system-to-hexo-theme-NexT/>
1. rouge options: <https://github.com/jneen/rouge#full-options>
1. latex cheat sheet: <https://wch.github.io/latexsheet/>
1. [Jekyll docs][jekyll-docs]

# Code block highlighting

## How to use highlighter
{::comment}
Overwrite the setting to not show line_numbers in fenced code blocks
{:/comment}
{::options syntax_highlighter_opts="{default_lang: ruby \}" /}

Code block using kramdown (**may not be compactible if not using kramdown**):

~~~ruby
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
~~~

Code block using Pygments or Rouge by using the `highlight` Liquid tag
(**putting it inside a list will work incorrectly**):

{% highlight ruby linenos %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

## How to highlight inlined code blocks

I discovered in 11/12/2016 that all (inlined) code blocks generated by rouge are
wrapped by a `<code>`{:.language-html} tag with css class set to
```css
language-some_language highlighter-rouge
```
. So we can copy the highlight.css to highlighter-rouge.css and make same css
rules applicable to
```css
highlighter-rouge span_css_class
```
(e.g. `highlighter-rouge cp`{:.language-css}).

If the code block is just a block of text, not code, we can append
`{:language-nothing}` to the end of the block to make rouge not generate
colorful html representations. E.g.
```markdown
`int i = 0;`{:.language-nothing}
```
looks like: `int i = 0;`{:.language-nothing}

# Block inline attribute lists (IAL)

A simple paragraph with an ID attribute: [link](#para-one)
{: #para-one}

Another example:

> A blockquote with a title
{:title="The blockquote title"}
{: #myid}

## Using IAL to generate a code block

```
{:.python}
    x = 0  # Initialize
    for i in range(10):
      x += func(i)
```

will look like:

{:.python}
    x = 0  # Initialize
    for i in range(10):
      x += func(i)

# Math equations

$$ a = b $$

$$ \Large a = b $$

$$\mathop{\underset{\mmlToken{mo}{⎵}}{78\,255\,300,00}}\limits_{10\text{zählende Ziffern}}$$

## MathJax macro

Macros are defined in `_layouts/default.html`{:.language-nothing} (see [here](https://docs.mathjax.org/en/latest/input/tex/macros.html) for more info).

```
$$ \RR $$
```

will look like:

$$ \RR $$

## MathML webpage conversion

To use MathJax javascript library to convert the MathML code in a webpage to Tex:

```javascript
var math_objs = MathJax.startup.document.getMathItemsWithin(document.body);
math_objs.map(m => m.math);
var i = 0;
for (obj of math_objs) {  // Use for..of, not for..in
  try {
    var tex = obj.math.replaceAll('|', '\\vert ');
    tex = tex.replaceAll('<', '\\lt ').replaceAll('>', '\\gt ');
    obj.start.node.innerHTML = '$$ ' + tex + ' $$';
  } catch (error) {
    console.error(i)
    console.error(error)
  }
}
```

where `math_objs` is a list of `MathItem`s, see [here](https://github.com/mathjax/MathJax-src/blob/a5ae9485cb7441fdd5ea59645cfbd1c12b7d53e1/ts/core/MathItem.ts), [here](https://docs.mathjax.org/en/latest/input/tex/extensions/configmacros.html) and [here](https://www.sqlpac.com/en/documents/mathjax-macros-and-packages-tex-simplified-commands.html) for more details.

Possible next steps:

- Replace recurring patterns with macros:
  - `\vert ... \vert` pairs -> `\abs`
  - `\mathrm{d}x` -> `\dx'
- Fix punctuations like `,()` -> `，（）`
- Fix fields like $$N,Q,R$$ -> $$\NN,\QQ,\RR$$
- Add more macros if necessary

Common helpers:

- To extract all Tex commands from a page:

  ```bash
  grep -o -P '\$\$[\x00-\x7F]+\$\$' my_file.md | sed -r 's/^[^$]*\$\$( ?)//g;s/( ?)\$\$$//g' | sort | uniq -c | sort
  ```

For more info:
- <https://jekyllrb.com/docs/extras/>
- <http://www.gastonsanchez.com/visually-enforced/opinion/2014/02/16/Mathjax-with-jekyll/>
- <http://haixing-hu.github.io/programming/2013/09/20/how-to-use-mathjax-in-jekyll-generated-github-pages/>
- <https://math.meta.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference>
- <http://latex.wikia.com/wiki/List_of_LaTeX_symbols>
- <http://www.onemathematicalcat.org/MathJaxDocumentation/TeXSyntax.htm>

# Tables

## With header row

```
| a | b | c |
|---+---+---|
| d | e | f |
```

will look like:

| a | b | c |
|---+---+---|
| d | e | f |

## Without header row

```
| a | b | c |
| d | e | f |
```

will look like:

| a | b | c |
| d | e | f |

# Graphviz, digraph, flowchart

See:

- <http://www.gravizo.com/>
- <https://www2.graphviz.org/>
- <https://www.draw.io/>
- <https://news.ycombinator.com/item?id=13326065>

For example:

```
<img src='https://g.gravizo.com/svg?
 digraph G {
   main -> execute;
   main -> init;
   main -> cleanup;
   execute -> make_string;
   init -> make_string;
   execute -> compare;
 }
'/>
```

will look like:

<img src='https://g.gravizo.com/svg?
 digraph G {
   main -> execute;
   main -> init;
   main -> cleanup;
   execute -> make_string;
   init -> make_string;
   execute -> compare;
 }
'/>

[jekyll-docs]: http://jekyllrb.com/docs/home
