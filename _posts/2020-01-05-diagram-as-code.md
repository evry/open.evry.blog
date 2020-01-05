# Diagram as Code

With the recent adoption of Infrastructure as Code we aim to eliminate the impedent mismatch of documentation (eg. how is it supposed to look like) and how it actually set up by driving all changes from a textual representation.

While JSON, YAML and HCL does a good job of represebting out intent to a computer it is not easily consumable for hunans - unless you live and breath for these types of configuration languages!

A diagram does a much better job of conveying information and relationships to us mortal humans but they struggle to keep up with the never ending changes as they are often hand drawn and a snapshot somewhere in time. Who knows where the master file for that one Visio diagram is, right?!

What if we could generate our diagrams in the same way as we generate our infrastructure?

This post aims to give an overview of the current state of diagram tools that support some kind of Diagram as Code functionality. Lets dig in! 😄

## Overview

Bellow is a table over the most popular tools with support for Diagram as Code. We have included some popular proprietary tools in the list but will leave it as an exercise to the reader to explore those as this post will focus on those that are free and open source.

| Tool                                               | Diagram Language                                            | License                                                                                | Local | Online |
| -------------------------------------------------- | ----------------------------------------------------------- | -------------------------------------------------------------------------------------- | ----- | ------ |
| [Graphviz](https://graphviz.org/)                  | [DOT](https://graphviz.gitlab.io/_pages/doc/info/lang.html) | [Eclipse Public License 1.0](https://gitlab.com/graphviz/graphviz/blob/master/LICENSE) | ✅     | (✅)    |
| [PlantUML](https://plantuml.com/)                  | Text                                                        | [GPL-3.0](https://sourceforge.net/p/plantuml/code/HEAD/tree/trunk/src/)                | ✅     | ✅      |
| [Mermaid](https://mermaid-js.github.io/mermaid/#/) | Text                                                        | [MIT License](https://github.com/mermaid-js/mermaid/blob/develop/LICENSE)              | ✅     | ✅      |
| [Ditaa](http://ditaa.sourceforge.net/)             | ASCII                                                       | [LGPL-3.0](https://github.com/stathissideris/ditaa/blob/master/LICENSE)                | ✅     | ❌      |
| [WSD](https://www.websequencediagrams.com/)        | Text                                                        | ❌                                                                                      | ❌     | ✅      |
| [code2flow](https://code2flow.com/)                | Text                                                        | ❌                                                                                      | ❌     | ✅      |
| [Structurizr](https://structurizr.com/)            | Java, .NET                                                  | ❌                                                                                      | ❌     | ✅      |

## Graphviz ([Live Demo](https://dreampuf.github.io/GraphvizOnline/#digraph%20G%20%7B%0A%0A%20%20subgraph%20cluster_0%20%7B%0A%20%20%20%20style%3Dfilled%3B%0A%20%20%20%20color%3Dlightgrey%3B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%2Ccolor%3Dwhite%5D%3B%0A%20%20%20%20a0%20-%3E%20a1%20-%3E%20a2%20-%3E%20a3%3B%0A%20%20%20%20label%20%3D%20%22process%20%231%22%3B%0A%20%20%7D%0A%0A%20%20subgraph%20cluster_1%20%7B%0A%20%20%20%20node%20%5Bstyle%3Dfilled%5D%3B%0A%20%20%20%20b0%20-%3E%20b1%20-%3E%20b2%20-%3E%20b3%3B%0A%20%20%20%20label%20%3D%20%22process%20%232%22%3B%0A%20%20%20%20color%3Dblue%0A%20%20%7D%0A%20%20start%20-%3E%20a0%3B%0A%20%20start%20-%3E%20b0%3B%0A%20%20a1%20-%3E%20b3%3B%0A%20%20b2%20-%3E%20a3%3B%0A%20%20a3%20-%3E%20a0%3B%0A%20%20a3%20-%3E%20end%3B%0A%20%20b3%20-%3E%20end%3B%0A%0A%20%20start%20%5Bshape%3DMdiamond%5D%3B%0A%20%20end%20%5Bshape%3DMsquare%5D%3B%0A%7D))

> Rock solid, and bindings for just about every language!

![graphviz.png](./assets/2020/01/05/graphviz.png)

Graphviz is an open source graph visualisation software. It has several main layout programs. It also has web and interactive graphical interfaces, and auxiliary tools, libraries, and a rich set of language bindings. The main project itself is not investing in graphical user interface editors, but leaving that up to the community to incorporate Graphviz. This results in Graphviz often being percived as a little more low-level compared with the others.

The Graphviz layout programs take descriptions of graphs in a simple text language named DOT, and make diagrams in useful formats, such as images and SVG for web pages; PDF or Postscript for inclusion in other documents; or display in an interactive graph browser.

Graphviz has many useful features for concrete diagrams, such as options for colors, fonts, tabular node layouts, line styles, hyperlinks, and custom shapes.

In practice, graphs are usually generated from an external data sources, but they can also be created and edited manually, either as raw text files or within a graphical editor. (Graphviz was not intended to be a Visio replacement, so it is probably frustrating to try to use it that way.)

## PlantUML

> Plantuml is pretty good because it like a mini programming language for diagrams, and not just UML!

PlantUML is another trued and true tiol usef to draw primarily UML diagrams, using a simple and human readable text description. Be careful, because it does not prevent you from drawing inconsistent diagrams (such as having two classes inheriting from each other, for example). So it's more a *drawing* tool than a *modeling* tool.

PlantUML has its own simple, but powerfull, domain specific language (DSL) that allows for a lot of deifferent types of UML diagrams:

* Sequence diagrams;
* Usecase diagrams;
* Class diagrams;
* Activity diagrams;
* Component diagrams;
* State diagrams;
* Object diagrams;
* Deployment diagrams;
*Timing diagram.

Also, it supports some non-UML diagrams which are pretty cool, for example the Wireframe diagrams for UI design.


https://mchernyavska.wordpress.com/2019/02/23/plantuml-diagrams-as-code/

http://plantuml.com/guide

## Mermaid ([Live Editor](https://mermaidjs.github.io/mermaid-live-editor/#/edit/eyJjb2RlIjoiZ3JhcGggVERcbkFbQ2hyaXN0bWFzXSAtLT58R2V0IG1vbmV5fCBCKEdvIHNob3BwaW5nKVxuQiAtLT4gQ3tMZXQgbWUgdGhpbmt9XG5DIC0tPnxPbmV8IERbTGFwdG9wXVxuQyAtLT58VHdvfCBFW2lQaG9uZV1cbkMgLS0-fFRocmVlfCBGW2ZhOmZhLWNhciBDYXJdXG4iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9fQ))

![mermaid.png](/Users/hans/Desktop/mermaid.png)



Synax Guide:; https://github.com/mermaidjs/mermaid-gitbook/blob/master/content/sequenceDiagram.md

VS Code extension: https://marketplace.visualstudio.com/items?itemName=vstirbu.vscode-mermaid-preview

typora.io: marmaid support

> This is integrated with Gitlab, you can use it in every textbox.
> 
> We used mermaid.js for a while. I didn't like the fact that it worked only in a browser, and installing phantom.js (which installed a whole pre-compiled binary of Chromium) was painful.

## Reference

* https://news.ycombinator.com/item?id=19066366
