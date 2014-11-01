---
layout: post
title: "Graphs of Wikipedia: Influential Thinkers [Round II]"
description: "An investigation in to the relationship between people on Wikipedia this time with upstream influence included."
tags: [SPARQL, snorql, python, Gephi, graphs, semantic, inluence, influential]
---

First of all, thanks very much for the feedback you all gave me on my graph of ideas. I wasn’t quite aware of how many people are interested in this sort of stuff. I now have lots of great ideas for new projects which will keep me busy for a long while. I must say making the graphs is the easy part – it is obtaining the data which takes time. I’ve made a note of all of your suggestions and will try to create something out of them soon. If you haven’t already, you can submit an idea here. I read them all.

## Housekeeping

There were a great number of comments about my last graph and so I’ll try to answer the main questions here. I think many of them were from people who hadn’t actually read the post at all but went straight to the graphic with whatever catch line someone shared along with it. It was reposted at Gizmodo, Spiegel.de, Business Insider and FlowingData.com all of whom omitted the very important caveats I listed in the post. Please read the original post for the full discussion. These were a few of the common themes to the criticisms I saw floating around the interwebs:

* ***It is way too biased towards Western ideas***
– Yes, see point one of the original blog post. I simply plotted what Wikipedia (dbpedia) gave me – of course it is biased, like any dataset and this was stated.

* ***Where are all of the musicians and artists?***
– See original blog post. Artists don’t have the available information in the Wiki info-boxes (well except some, see bottom left, green part). I hope to make a musician/artist graph soon!

* ***The title is very misleading.***
– The original post had an asterisk on the word ‘every’ which was meant to highlight the fact the graph had caveats. I didn’t anticipate people leaving this out when they shared it with their friends. I changed it to be simply ‘The Graph Of Ideas’. I’ll be more careful in future.

Now that is out-of-the-way, I’d like to present my latest work. I’ve broken this post down into two sections: the network and the method. In order to fully understand the network, I suggest you also read the method. If you have better things to be doing with your life, quickly check out the plot below, glance at the caveats and then move on – I’ll catch up with you soon enough.

## Version I vs. II

The first graph connected people via a single connection. That is to say, if Socrates influenced Plato and Plato influenced Aristotle then the following connections were made:

Socrates –> Plato –> Aristotle

Easy, right?

However, as I briefly mentioned in the previous post, each individual in time represents the sum of their ancestors. This means that Socrates should technically be linked to Aristotle too! Whether we like to think about it or not — Socrates’ contribution to our body of understanding of the world is embodied in the way we speak and interact on a daily basis. Sure there is some dilution, but Socrates’ philosophies are for better or for worse, buried deep within you somewhere . This isn’t just true for Socrates either – it is true for everyone who has ever existed.

On the September 5th, 1948, Jiddu Krishnamurti gave a public talk in Poona, India. It it he stated:

> You and I are not isolated; we are the result of the total process, the outcome of the whole human struggle, whether we live in India, Japan, or America. The sum total of humanity is you and me. Either we are conscious of that, or we are unconscious of it.

Welcome back… so with all of this in mind I went ahead and made a little program which calculates these upstream connections which were missing in the first graph: hence the 'Version II'.

Here is the resulting graph (~20% of the total ~4,200 nodes available):

<br/>
<br/>
![Comedians](/assets/wikipedia/Comedians.png)
<br/>
<br/>
![Authors](/assets/wikipedia/Authors.png)
<br/>
<br/>
![Writers](/assets/wikipedia/Writers.png)
<br/>
<br/>
![Philosophers](/assets/wikipedia/Philosophers.png)
<br/>
<br/>


|Name|     Connections|    Nationality| Born (B.C.) |
|:-------------:|:-----:|:-----:|:-----:|
|Thales|  3390|    Greek|   624|
|Pythagoras|  3386|    Greek|   570|
|Zeno| of| Elea|    3378|    Greek   490|
|Socrates|    3376|    Greek|   469|
|Parmenides|  3368|    Greek|   5th cent.|
|Protagoras|  3352|    Greek|   490|
|Plato|   3351|    Greek|   423|
|Melissus of Samos|   3332|    Greek|   5th cent.
|Leucippus|   3329|    Greek|   5th cent.|
|Zeno of Citium|  3306|    Greek  | 334|
|Pyrrho|  3306|    Greek|   360|
|Stilpo|  3300|    Greek|   360|
|Posidonius|  3288|    Greek|   135|
|Panaetius|   3286|    Greek|   185|
|Lucretius|   3275|    Roman|   99|

Bertrand Russell in his History of Western Philosophy (1945) wrote “Western Philosophy begins With Thales”. As we can see, his claim is backed up by this graph. Thales is the most connected individual with 3390 connections. This doesn’t mean he is the most influential or humanity’s biggest asset – it just means that if the data was complete (which it isn’t), then his ideas have influenced (in whatever arbitrary way you define it) the most number of people.

The margin separating top 5 is also quite small. This is presumably because there are only one or two degrees of separation connecting them all. Interestingly there is only one person not of Greek origin in the entire top 10. Again, this is largely due to the incompleteness of the dataset – these gentlemen also have antecedents which are either a) not entered into Wikipedia or b) have been lost in history. As one Redditor wrote: “the group that came up with fire should be in the middle and bigger than everything combined”.

This type of graph just shows how much our perceptions of ideas can change depending on how we present information. This is my take home message for today.

Those in a rush — scroll to the caveats!

As you can see, we tossed around nested do and while loops but they were just too complicated – the solution matrices.  There are a whole host of ugly problems you encounter if you try solve this using nested do and while loops. All this means is that my connection map will look something like this:

{% highlight Text %}
#,A,B,C
A,0,1,0
B,1,0,1
C,1,0,1
{% endhighlight %}

The number 1 represents a connection between the row and corresponding column e.g. A is connected to B, B is connected A and C and C is connected to only A. I wrote a script in Matlab which calculates just this and generates a new list of new connections. Essentially this works by looping through the rows and checking if there is a connection (=1). If there is a connection, it then finds where the person they are connected to in the same group of rows. Once they are found, their entire row is added to the original person’s row. . If you’re a bit of a coding oracle please let me know if there are faster ways of achieving the same result. So for our matrix above, C influenced A but A influenced B so C should also influence B, right? This algorithm turns the above matrix into this (just for the looping component on 3rd row:

{% highlight Text %}
#,A,B,C
A,0,1,0
B,1,0,1
C,1,1,1
{% endhighlight %}

Specifically, row C is added to row A and the dot product of the two is subtracted. This ensures there is always either a ’1′ or a ’0′ in all cells. The algorithm loops over every row in the matrix and carries out this procedure.

The original list had 14,560 connections so it took a reasonable while to do all of the permutations on my laptop (10 minutes). This new list has 4,239 nodes with over ~830,000 connections. Last time I checked, 830,000 > 14,500 so there is a lot more connectivity going on in this graph than the previous one. My code can also contain self-references. This is because two people may be contemporaries of one another and influence one another. If I influence my brother and he influences me, do I not have a slight influence myself through the actions of my brother? I thought I would leave these in just to see where they would turn up. No harm done here.

Once you make the matrix there are a whole heap of interesting things you can do. For example, who has the most connections in the network? Well, you just sum the row of each matrix and sort it in descending order (shown at the start). The last part of my script does this for you. I understand many of you won’t have Matlab and so I apologise in advance for this in advance. I might try to do it in Python next time. In the mean time, you could try a free trial version or use Octave which is a free version of matlab.

For our example above, once you create the matrix, all you need to do now is simply create a .dl file which contains the following:

dl n=3
format = fullmatrix
labels:
Person A, Person B, Person C
data:
0 1 0
0 0 1
1 0 1

This is the information which helped me. Once I obtained this matrix for the Wikipedia network, Gephi was able to import it. Thank-you to whoever made this extension – it is genius.

Sorry to waffle on a bit but last time I had a number of people requesting more detail on how I go about making the graphs. Finally, to save you going back through the text, here are links to the data I used to create my map. I’ve compressed some of them but at most, they will expand to about 100MB.

All of the data:
* You’ll need Gephi (free) and Matlab (or Octave).
* Original list of people and their influences from dbpedia.
* The Matlab script which generated the linking matrix.
* The list of names used in the network.
* The csv matrix of 1′s and 0′s only.
* The final .dl file require to import into Gephi.

## Caveats

* Many important people have been left out of the network. I am limited by the information provided by dbpedia. I mean I had to cut 80% of the network I had available just to make the plot I showed here!
* The communities are coloured by the Modularity module in Gephi – I do not personally colour anything.
* The graph is biased towards Western ideologies. The graph is biased towards Western ideologies. Yes 2x.
* That’s all for now. Let me know in the comments section if you have any questions.
