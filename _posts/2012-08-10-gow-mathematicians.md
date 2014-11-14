---
layout: post
title: "Graphs Of Wikipedia: Mathematicians"
description: "Who influenced who in mathematics?"
tags: [python, SPARQL, DBpedia, wikipedia, mathematics,mathematicians]
---

I've recently been digging around the topic of ***influences*** so I thought it would be interesting to examine a few subnetworks within the large network of everyone. This time I set my scopes on mathematicians. There is no primary reason why other than – I can. I’ve long been interested in the history of mathematics and so I wondered what a network of great mathematicians actually looked like? Could there be underlying structures between mathematicians who have influenced each other over history? This time I used Freebase which has an excellent query system which enables you to pull out information on pretty much anything you can think of. I focused on the influence node for this task. I set a few filters such as; everyone in the network has to have `Profession == Mathematician`. This removes a lot of fluff and creates a nice csv file which is usable within Gephi. As usual I had to do a bit of cleaning up in Microsoft Excel but in the end it turned out alright ([download](/assets/wikipedia/mathematicians.png)).

[![Graph of Mathematicians](/assets/wikipedia/mathematicians.png)](/assets/wikipedia/mathematicians.png)

As expected, Aristotle, Newton, Avicenna and Gauss are some of the biggest nodes in the network. Indeed, they were tremendously influential on the course of mathematics.
Interestingly, after applying modularity, a few little subnetworks within the total network appear. Many of the ‘traditional’ mathematicians cluster together; i.e. Gauss, Jacobi, Riemann, Dedekind etc. Some broad categories I found include:

### Astronomers/Physicists/Greeks

[![Mathematicians: Astronomers/Physicists](/assets/wikipedia/astronomers-physicists.png)](/assets/wikipedia/astronomers-physicists.png)

### Geometers
[![Mathematicians: Geometers](/assets/wikipedia/geometers.png)](/assets/wikipedia/geometers.png)

### Logicians
[![Mathematicians: Logicians](/assets/wikipedia/Logicians.png)](/assets/wikipedia/Logicians.png)

### Muslim/Arab Scholars
[![Mathematicians: Muslims/Arabs](/assets/wikipedia/muslim-arab_mathematicians.png)](/assets/wikipedia/muslim-arab_mathematicians.png)

### 19th century
[![Mathematicians: Traditional](/assets/wikipedia/traditional_mathematicians.png)](/assets/wikipedia/traditional_mathematicians.png)
<br/> 
<br/>
There are other networks – see if you can spot why certain names are clustered together (e.g. linguistics, algebra, philosophy etc). I found a bunch of mathematicians I had never heard of which has provided me a list of biographies to read.

## Things To Keep In Mind

* There are only nodes for which there is available data. There are obviously a great number of influential mathematicians missing from the network.

* I had to restrict to `Profession == Mathematician` and this might be too restricting in that if a mathematician in the database doesn't have their profession set, then they would have been excluded.

* There may be some contamination as I found Ralph Emerson, Mohammed and Buddha in amongst the mathematicians which I hate to say it – just aren't. I tried to clear up these contaminants but there still might be residuals. Feel free to point these out.

* It is a graph of influence between people, not a graph of how influential they were on mathematics.

If you like, you can [purchase a poster version of this graph here](http://www.redbubble.com/people/griffsgraphs/works/9087956-the-graph-of-mathematicians?p=poster).