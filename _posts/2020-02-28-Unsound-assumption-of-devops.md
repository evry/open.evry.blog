---
layout: post
author: Kent Inge Fagerland Simonsen
author_hanndle: kentis
title: The unsound assumption of Devops
date: 2020-02-28
tag: [iac]
published: true
hn_item_id: 21964800
html:
  head: |
    <script src="https://cdnjs.cloudflare.com/ajax/libs/mermaid/8.4.4/mermaid.min.js"></script>
  script: |
    |
      <script>
        var config = {
          startOnLoad: true,
          theme: 'forest',
          flowchart: {
            useMaxWidth: false,
            htmlLabels: true
          }
        };
        mermaid.initialize(config);
        window.mermaid.init(undefined, document.querySelectorAll('.language-mermaid'));
      </script>
---
## Background

The success of devops has been phenomenal. Its techniques has been used to remove bottlenecks in the areas that have traditionally been in the domain of operations while  improving the flow of tasks in domains traditionally thought of as development. By now, many of the assumptions and hypotheses underpinning devops must be said to be confirmed.

At the fringes of devops, however, there is an assumption that may be unsound. If, this assumption is falsely believed to be true it could cause businesses and the software industry to make poor decision on what to do once the bottleneck in it delivery is successfully moved from operations to development.

The assumption discussed here is that Software Engineering techniques are about as good as they are ever going to get. In the Devops Handbook[1] this assumption is evident in several passages where it is indicated that development is the correct place for the bottleneck in a software delivery process. One such passage is quoted below (emphasis mine).

“
After all these constraints have been broken, our constraint will be Development or the product owners. Because our goal is to enable small teams of developers to independently develop, test, and deploy value to customers quickly and reliably, this is where we want our constraint to be. 
“
In Project to Product, this assumption is explicitly stated with the following passage:

"
Technology improvements will be relevant by incremental, yielding productivity gains of less than ten percent to organizations via new programming languages, tools, frameworks and runtimes.
"

The implication of this, when the bottleneck is moved to development from operations and assuming best practice code practices are followed, is that  the only significant improvements will he expected to come from process changes such as adopting the next greatest process. In this there is a risk of missing technical improvements, assuming that they can be made.

This assumption is unsound simply because it is not possible to know what improvements future software engineers will come up with. This, however does not imply that it is false. 

In the next sessions we will discuss some reasons to believe why it may be false  Furthermore, we will speculate on what the implications may be if the assumption is false.

## Why this assumption may be erroneous

The first reason to believe that the assumption that SE can not really be improved much more is the fact that predicting the future in advance is difficult. If lean manufacturing had been perfected in 1800's, the advent of the integrated circuit and all the technological advances it has made possible would still have greatly improved production. In the book “From Project to Product” the author illustrates this nicely by describing the technological marvels he encountered while visiting a BMW production plant.

Another reason why we might see marked improvements of SE is the existence of several existing candidate approaches. These include, model driven software engineering  (MDSE), low- and no-code approaches.

The field of model driven software engineering  has been seen as a promising new paradigm in SE in academic communities for years, if not decades. The idea is to raise the level of abstraction of coding by generating code directly from models. Low-code approaches are often similar, however, currently, somewhat more restricted and lack the formality found in many MDSE approaches.

Aside from the support from the academic community, another reason to think that MDSE based approaches may improve significant on current SE practices historical. Previous significant programming advances such as moving from assembly to high-level programming languages have been made by increasing the level of abstraction.  

Furthermore, low-code approaches and tools utilizing MDSE techniques, have been rising in popularity in recent years. This could be the beginnings of widening adoption of MDSE-like techniques.

## Implications of Unsoundness

If the assumption that there will not be any significant improvements of SE is false, this may have some unfortunate consequences if we, as an industry, take it as true. One consequence is that this may become a self-fulfilling prophecy. This can delay, or prevent, the next great leap in SE performance simply because we, as an industry will not be looking for it. This may already be a factor in the current divide between SE research and practice.

If this assumption is falsely assumed to be true by a subset of businesses, these businesses risk being left behind when the next big improvement comes. This will, in this scenario, give a significant competitive advantage to those businesses that do not follow the word of the devops literature blindly.

A somewhat more subtle consequence of this assumption, if it is false, is that focus is shifted away from improving technical excellence, which after all is assumed to be about as good as it is ever going to get, towards processes. In this case, improvements in software engineering may be assumed to come from changes in processes rather than improved SE techniques. This, in turn may cause the business to focus on having software engineers that follow defined processes rather than using their creativity to improve the way they work. This can create business-process lead agile initiatives that are in conflict with the first principle of agile: “Individuals and interactions over processes and tools”
