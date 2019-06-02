---
title: "Graphs of Wikipedia: Programming Languages"
subtitle: 'How are programming languages connected?'
summary: 
authors:
- admin
tags:
- python
- language
- graphs
categories: []
date: "2014-01-19T00:00:00Z"
lastmod: "2014-01-19T00:00:00Z"
featured: false
draft: false
---

A while back I posted about [a graph of the personalities on Wikipedia]({{ site.url }}/gow-influential-thinkers/). This time I wanted to see which programming languages were linked to one another by user-entered “Influenced” and “Influenced-by” information. Take for instance the functional language Haskell:

[![gow-wikipedia](./images/Haskell-Programming-Wiki.png)](./images/Haskell-Programming-Wiki.png)

In the infobox on the side we find a large list of languages Haskell is connected to in one way or another. [Wikipedia devotes an entire section](https://en.wikipedia.org/wiki/Haskell_(programming_language)#Related_languages) to how it is related to other programming languages for those interested.

<center>
[![gow-wikipedia](./images/Haskell-Programming-Wiki-Zoom.png)](./images/Haskell-Programming-Wiki-Zoom.png)
</center>

It must be emphasized that the links are user-generated and any such comparison is largely subjective in nature (especially when comparing concepts rather than syntax). The following query [executed here](http://dbpedia.org/snorql/) provided me with the bulk of the data:

```sql
SELECT *
WHERE { ?p a <http://dbpedia.org/ontology/ProgrammingLanguage> .
?p <http://dbpedia.org/ontology/influenced> ?influenced . }
```

The output was then decoded using a [nifty URL decoder](http://meyerweb.com/eric/tools/dencoder/). It was then fed through [a Python script](https://github.com/bgriffen/griffsgraphs/blob/master/programminglanguages/proglanguages.py) to arrange it in a format most suitable for Gephi. The graph below represents the connections between all programming languages in Wikipedia. A force algorithm was applied such that closer nodes are more strongly connected in nature. The size of the node indicates how many connections that language has to the others in the network. The colors are achieved by carrying out a modularity algorithm applied by Gephi to highlight subnetworks. The curvature of outgoing edges is clockwise indicating influence direction. Lisp for example has many clockwise edges going out and only few counter-clockwise coming in. I can see some relations in the languages I am familiar with but perhaps you notice a few things that are flat out wrong? Please let me know in the comments as I’d be interested in hearing your thoughts. The raw Gephi graph data (.dl, .dfg, .gephi, .dexf, .gml etc.) can be [found here]({{ site.url }}/data/programminglanguage-data.zip).

The one uses curved edges:
[![gow-wikipedia](./images/programminglanguages-label.png)](./images/programminglanguages-label.png)

This one uses directed edges:
[![gow-wikipedia](./images/programminglanguagesarrows-label.png)](./images/programminglanguagesarrows-label.png)


As one might expect all of the major players are the biggest nodes. C, Haskell, Lisp, Python and Java all feature prominently. Anything strange you notice? Let me know in the comments. I really must commend the designers on their nomenclature. See if you can find one of the more humorous languages by zooming in. I also obtained the designer of each language and connected the people together based on the programming languages they were involved with. This was obtained by the following query:

```sql
SELECT *
WHERE {?p a <http://dbpedia.org/ontology/ProgrammingLanguage> .
?p <http://dbpedia.org/ontology/designer> ?designer . }
```

Large nodes do not represent more influential people but simply the people whose work spawned the most number of languages in the subsequent years. As expected, it is a very homogeneous playing field as many people were involved in multiple languages and at times had many collaborators. It must be stressed that the dataset is incomplete and was the result of my somewhat rudimentary way of parsing the data. Without a doubt, things could be improved. I’ve setup [a GitHub repository](https://github.com/bgriffen/griffsgraphs) for all (Gephi) graph files and images for those interested. Feel free to embed or share the above images. Lastly, please keep in mind where this data is coming from: contributors of Wikipedia. Whilst they are a studious bunch, they aren’t without faults so take up any problems with the graph with the pages themselves as that is all this represents.

**Update!** Since posting there has been some great feedback found on the web. Of particular interest is [this PDF](http://oreilly.com/news/graphics/prog_lang_poster.pdf) of the history of programming languages and [this fantastic interactive version](http://exploringdata.github.io/vis/programming-languages-influence-network/) of what I was trying to present by [Ramiro Gomez](https://twitter.com/yaph). I think it really is time to move to [D3.js](http://d3js.org/) and [sigma.js](http://sigmajs.org/).

