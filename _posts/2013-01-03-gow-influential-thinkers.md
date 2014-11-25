---
layout: post
title: "Graphs of Wikipedia: Influential Thinkers"
description: "An investigation in to the relationship between people on Wikipedia."
tags: [SPARQL, snorql, python, Gephi, graphs, semantic, inluence, influential]
---

The internet is big — very big. One such way to investigate all of this free online content is through graphs. The network visualisations by Simon Raper in his [fantastic post](http://drunks-and-lampposts.com/2012/06/13/graphing-the-history-of-philosophy/) about graphing the history of philosophy is one example of how to exploit such data. Let’s take this a step further and create a series of graphs using everyone on Wikipedia. Using subsets of this dataset (authors, actors, sports players etc.), we can investigate sub-networks within the larger dataset. In the next series of posts, I will present a few of my findings in each of these sub-networks. For now, I’ll focus on generating the core data set using everyone on Wikipedia as of January 2013. To start, let’s look at how people influenced each other throughout history as entered by the authors of Wikipedia. If you search for a notable figure you might find the following on the sidebar:

<div style="text-align: center;">
<IMG SRC="/assets/wikipedia/aristotle-wikipedia.jpg" ALT="Aristotle">
</div>

Our region of interest is the influence section:

<div style="text-align: center;">
<IMG SRC="/assets/wikipedia/aristotle-wikipedia-zoom.jpg" ALT="Aristotle">
</div>

Here we see a list of people that have been influenced by Aristotle. We also see who Aristotle himself was influenced by. At the moment, these are just names, but this provides a way we can connect Aristotle to other people throughout history. If only we had a way to collect this data using a simple database-like query.

## Collect Data

First we require a database-like, indexed version of Wikipedia to collect our data. [DBpedia]("http://dbpedia.org/About") is one such venture which has been created as part of the [Wikipedia project]("https://en.wikipedia.org/wiki/Wikipedia:WikiProject"). This [structured content]("https://en.wikipedia.org/wiki/Structured_content") is then made available to the public at no cost. DBpedia allows you to query relationships and properties associated with Wikipedia resources, including links to other related datasets. The first graph will connect people via their known influences. The snippet of code entered into the [SPARQL Explorer]("http://dbpedia.org/snorql/") looks something like this:

{% highlight SQL %}
SELECT * WHERE
{ ?p a <http://dbpedia.org/ontology/Person> .
?p <http://dbpedia.org/ontology/influenced> ?influenced.
?influenced a <http://dbpedia.org/ontology/Person>. }
{% endhighlight %}

This essentially translates into something like: collect all people where there is an 'influenced' connection between each person. We must also add that ‘influenced by’ entries by reversing the order of the influence query.

## Clean Data

Once the data has been collected, it will need to be cleaned. Copy this data into a little URL decoder and imported them into Microsoft Excel for further processing (removing things like ‘(Musician)’ and other similar syntax). There are tools to directly query DBpedia and then parse the data into the graphing program but it may be useful to store a clean dataset for future projects. For Gephi, the CSV file needs to have the format: "Person A","Person B".

Duplicates found in the ‘influences’ list were then removed. This ensured a more complete dataset. We then exported these as a csv and imported into Gephi (free on OSX and Windows). Be sure to include the quotations. This is because the blank space confuses Gephi into reading separate names. We want both the first and last names.

## Visualisations

Once you get to this point, it really is a matter of aesthetics as to how you wish to present the data. These CSV files can be directly imported into Gephi and graphed. For those new to these types of graphs: the nodes represent an object or person and the size represents the number of connections to each other node. Around these nodes, cluster other personalities who are similarly related thinkers/authors.The Fruchterman-Reingold algorithm and Force Atlas 2 algorithms were used to cluster the data based on their connections. Communities are then identified using the ‘Modularity’ module. Once you are happy with how the graph looks, you can export it to your favourite format (PNG, PDF etc.).

A full graph I generated was used at the 2013 TEDx at Huntsville, Alabama and can be downloaded [here](/assets/wikipedia/fullgraph-huntsville.png).

![Everyone](/assets/wikipedia/gow_huntsville.png)

To make it blog friendly, I zoomed in on a few areas of interest:

### Authors
![gow-12](/assets/wikipedia/gow_image12.png)
![gow-13](/assets/wikipedia/gow_image13.png)
![gow-14](/assets/wikipedia/gow_image14.png)
![gow-15](/assets/wikipedia/gow_image15.png)
### Artists
![gow-7](/assets/wikipedia/gow_image7.png)
### Comedians
![gow-8](/assets/wikipedia/gow_image8.png)
### Philosphers
![gow-11](/assets/wikipedia/gow_image11.png)

## Interactive Version
I also exported this to a JSON file  to make an interactive version [available soon].

![Interactive Version](/assets/wikipedia/FullInfluenceGraph.png)
![Zoom of Interactive Version](/assets/wikipedia/Nietzsche.png)

If I am interested in one author and want some inspiration as to who to read next I can just search this graph and find a name I don't know but is closely related by influence.

## Analyze Data

The bigger the node, the larger the number of connections i.e. the bigger influence that person had on the rest of the network. In this example, Nietzsche, Kant, Hegel, Hemingway, Shakespeare, Plato, Aristotle, Kafka, and Lovecraft all appear as the largest nodes and so one would think that these are the most influential figures in the network. *This however brings us to one of the largest problems in doing work like this; the graph is intrinsically “wrong”*. 

First we need to take a step back and understand where the data is actually created. Content on Wikipedia is entirely generated by its users. Whilst this user-base spans thousands of individuals with varied backgrounds and cultures, it will still have self-selected biases and these biases will show up in our graph. I personally don’t have any statistics on who exactly enters in the information into Wikipedia but I’m guessing there is an average age, predominant gender and culture (see here and here for some further information). This means that whilst the graph does show some of the influential figures of history, it actually is a reflection of the Wikipedia author’s averaged perception of history. I have no quantifiable data on what the word ‘influence’ actually means and so it is up to the reader to interpret this work with caution. This is just one of the many types of things one can do with the data freely available on the web. I'll be putting future projects, including this one in the following repository:

<center>
<div markdown="0"><a href="https://github.com/bgriffen/griffsgraphs" class="btn">Graph Repository</a></div>
</center>

## Posters

I’ve had specific requests to make my graphs available for print. [Here is a link](http://www.redbubble.com/people/griffsgraphs) to my RedBubble account where you can purchase. It was not my intention from the outset but I don’t have the resources to print them and mail them myself. Feel free to [email me](mailto:brendan.f.griffen@gmail.com) for any further information or ideas you might have for similar type networks.

