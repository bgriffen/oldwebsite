---
layout: post
title: "Chess, A Study"
description: "What can you learn from just the games played?"
tags: [python, chess, network, cluster, gephi, modularity, community]
---

Chess is a very old game dating back to a 4 sided variant in India played in the 8th century. I happen to be quite fond of the game and wondered if anything could be gained by studying the game database of famous players throughout history. 

Records of chess date back much further than you might think. For instance, the exact moves played by Napoleon against General Bertrand in the early 19th century are known. Even further still, the oldest surviving game is between Gioachino Greco and an anonymous player in 1619 ([a nice queen sacrifice](http://www.chessgames.com/perl/chessgame?gid=1243022)).

With the advent of the computer we now have all of these games neatly [stored in databases](http://www.chessgames.com/index.html) often with annotations about each move, the time they took to make the move and a whole heap of other ancillary information (e.g. type of opening, tournament etc.).

[PGN mentor](http://www.pgnmentor.com/files.html) is a nice site which has broken down each of the strongest players througout history into easily readably "pgn" files (the standard  format for storing chess games).

In the first instance I wanted to build a network of all of the players who were available in the database. I simply connect players that have played one another and weight the connection by the number of times they have played. The thicker the line the more games those two have played against one another. Here is the full graph.

<br/>
![Full graph of chess players]({{ site.url }}/images/chess/fullgraphofchessplayers.png)
<br/>

I've used a modularity algorithm to color code the data according to the communities in which each player finds themselves. The spatial information is based on a different algorithm based on force. More closely clustered players are more likely to have played one another (e.g. you find Kasparov and Karpov very close to one another with an extremely thick line indicating many, many games). It must be emphasized that he communities arise *organically* from the underlying algorithm. 

### Late 19th century, Early 20th Century

There are three major communities represented in the graph largely divided by "chess eras". Starting on the right, in green, we have the 19th century players and we see all the usual suspects (e.g. Paul Morphy, Nimzowitsch, Tarrasch). You'll notice that the size of these nodes are quite small relative to the rest of the graph. This means that the these gentlemen played one another far less than their modern counterparts. This just a reflection of the fact that in those times, it was hard to arrange tournaments and matches amongst top players due to lack of financial support and logistical challenges of meeting one another. Most if, not all of them had actual jobs and chess was more of a "serious hobby" in those days. Now world champions can enjoy $1,200,000+ Euros for just 12 games!

The neat thing about this community is that you can clearly see (without consulting a table) who played who the most. Can you spot these players in a glance? Some include Paulsen-Anderssen, Tarrash-Janowsky, Steinitz-Chigorin, Euwe-Alekhine. If you have a little background knowledge of the history of chess, the size of the edges between these players reflects particular historical events (e.g. world championship matches).

Interestingly, Paul Morphy sits off by himself in the corner despite him being regarded as arguably the best chess player of his era. Lastly I want to highlight that the larger the node does not imply that they are stronger players. It just means that they have played more people in the network than someone occupying a smaller node. We'll see this in action soon.

### Mid-20th century

As we move to the left, we find more players who tie in to the next generation of chess players (e.g. Capablanca, Reti, Tartakower, Gligoric, Keres). Again here we see some large connections between certain players (e.g. Botvinnik-Symslov, Petrosian-Spassky). As we move further to the left we see an almost timeline of the history of chess. You're eyes might set on one very large node in the network: Viktor Korchnoi. Korchnoi is arguably one of the strongest players never to become world champion (there is poor old Keres as well). He is almost X years old and doesn't look like he is going to retire any time soon. Give his age and his extrodinarily large time span of activity, it is no suprise that his is both the largest node and smack bang in the middle of the network. He has connections to nearly all other major players of the 19th and 20th century.

Right at the end of the "red community" we have the famous Kasparov and Karpov duo who have the thickest connection in the whole network. This again highlights the history of these two players. They played each other hundreds of times (Karpov supposely lost 12 lbs playing him in the 1985 world championship because they played so many games over the course of a few months).

### Late 19th century, 20th century

We then arrive at the edge of the next community which encloses most of the top players active today. As expected, this is a bit of a mess because of just how often these players face-off against one another. A few larger Kasparov-Kramnik, Anand-Ivanchuck, Karjakin-Mamedyarov. I didn't actually know Gelfand-Leko had such strong ties but there you have it. Also, a lot of the bigger nodes are also the oldest players which makes sense since they have had more time on earth player against one another. Carlsen, the current world champion is situated between Radjabov, Aronian, Bacrot and Caruana, all relatively young players.


### Quanitfying The Clustering: Degree & Centrality

If we now run a few extra codes on the network we can generate the graph measures such as betweenness centrality, closeness centrality, clustering coefficients and the basic degree.

If you aren't familiar with these measures, I suggest you quickly have a look at these examples. If you want an *extremely* brief introduction (paraphrased from Wikipedia for brevity):

**Degree**

The degree (or valency) of a vertex of a graph is the number of edges incident to the vertex, with loops counted twice. 

**In-betweenness centrality**

Betweenness centrality is an indicator of a node's centrality in a network. It is equal to the number of shortest paths from all vertices to all others that pass through that node. A node with high betweenness centrality has large influence to the transfer of items through the network, under the assumption that transfer follows shortest paths.

**Closeness Centrality**

In connected graphs there is a natural distance metric between all pairs of nodes, defined by the length of their shortest paths. The farness of a node s is defined as the sum of its distances to all other nodes, and its closeness is defined as the inverse of the farness.

**Clustering Coefficient**

A clustering coefficient is a measure of the degree to which nodes in a graph tend to cluster together. There is a global clustering coefficient and a local clustering coefficient. The local clustering coefficient of a vertex (node) in a graph quantifies how close its neighbors are to being a clique (complete graph).

Two versions of this measure exist: the global and the local. The global version was designed to give an overall indication of the clustering in the network, whereas the local gives an indication of the embeddedness of single nodes.

With these definitions out of the way, lets take a look at some data.

If we order by degree, we confirm what we already saw in the original image: Korchnoi is top dog with 156 connections with others in the network. Though if you look at the weighted degree, Karpov is in fact higher but this is likely inflated by the games he played with Kasparov during the world championship. Closely behind Korchnoi we have Beliavsky and several others hovering around the same number (~130) further down.

If we now take a look at more interesting columns like closeness centrality we find that Michael Adams occupies the top spot (1.583). This means...

Rank ordering by closeness centrality we find Philidor at the top spot with 0! OK, he technically should be removed from the plot since the only games I have of his in the database are those with players who aren't in the network. He appears as an isolated node as his degree is zero. If you go one notch down we see Korchnoi again whose closeness is far below everyone elses at 1.381 (~0.01 lower than the Beliavsky who occupies the number two spot).

If we now order by clustering coefficient we find Korchnoi again at the top (0.498).

We learn that Korchnoi is again ahead of the pack.

All of this is just to show you that graphs of this kind are not just pretty pictures but contain interesting metrics which the human eye can not ordinarily quantify. The simple task of identifying the "key node" in a graphs of this kind is a non-trivial task if you don't carry out the proper analysis -- even I am only going skin deep.

I also wanted to reproduce the same plots recently done by Visualst.ix showing which players prefered which sort of positions, frequency of queensize vs. kingside castling and the average number of moves per game.

I also wanted to extend this a little bit and see how these preferences have changed over time. 

[x] Average rating over time.
[] Location map with nodes connected.
[] ECO codes over time (+general histogram).
[] Length of game vs. average elo.
[] Draw/win/loss percentage vs average elo.
[x] Average number of king moves.
[x] Average number of knight moves.
[x] Average number of pawn moves etc. (+over time).
[x] Average number of captures (+over time).
[x] Average number of checks (+over time).
^ Players rank ordered by each of these metrics.

Players with most number of decisive games.
Biggest draw percentage (between two opponents and in general + over time).
Frequency of white wins, black wins, draws.
