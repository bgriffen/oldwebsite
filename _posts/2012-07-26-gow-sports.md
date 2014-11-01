---
layout: post
title: "Graphs Of Wikipedia: Sports"
description: "How are the various teams and players of different sporting communities connected?"
tags: [python, SPARQL, DBpedia, wikipedia, sport, football, basketball, ice hockey, baseball, soccer]
---

My goal here is to see if I can create a network of famous sports people based on only two pieces of information: their name and their team. I’m going to need your help on this one so keep your wits about you as you read on. The sports I chose for this study were:

* American Football
* Basketball
* Ice Hockey
* Baseball
* Soccer (Football)

I simply went with the sports which had the most data available on Freebase‘s sports section. Just to be clear, I will be calling Association Football `Soccer' to distinguish it from American Football. Sorry Europe.

## The Concept 

To get the core dataset I simply queried Freebase for players which had an available team. I then had to think of a way to connect them. I figured players move around quite a bit over a given career so I thought perhaps I could link both teams and players together based on their affiliations (i.e. two distinct but related graphs). I settled on two types of graphs for each sport:

* The Player Graph: This consists of people only e.g if Diego Maradona played for FC Barcelona and Gary Lineker also played for FC Barcelona, then a connection can be drawn between the players: Maradona and Gary.
* The Team Graph: This consists of teams only e.g. if Diego Maradona played for FC Barcelona and Boca Juniors, then a connection can be drawn between the clubs:  FC Barcelona and Boca Juniors.

In this way, for the player map it sort of builds up a network of affiliation. Sure, the players might not have ever met or had any influence on one another, but their connection to the same club does connect them together via a common strand. Similarly for the club/team maps, clubs which have had similar players move between them will be more closely connected. Writing this just now, I haven’t yet seen the graph I’m about to create so it will be interesting to see if certain club allegiances come out of the woodwork.

Lastly, most of the people and teams will be American (I’m limited by what Freebase gave me). Can American viewers please comment on any interesting features, particularly within Baseball, Basketball and Football! As Freebase’s data becomes more complete, these graphs will become more complete.

## The Method

Again I decided to approach this one using matrices. Please see my post on Wikipedia personalities to see how the datasets are preprocessed. I did however have to design a brief program to connect the various teams and players together. It essentially involves a tripple for loop (there are definitely better ways of doing this!). I chose Matlab since a lot of the code from previous posts can easily be copied across to treat new datasets. I’ve written a few functions now which help process Freebases’ somewhat annoying csv outputs – especially if I have to obtain them from the data dumps and not queries. If there is sufficient interest in how my program works, I can make it available. Most of my time was spent trying to understand unicode and encoding formats to interpret non-english names. I actually learned quite a bit about script blocks and how symbols, east-asian scripts etc. are stored – it was something I had wondered about for a long time. In any case, my little program basically converts this:

{% highlight Text %}

Diego Maradona,Boca Juniors
David Beckham,LA Galaxy
David Beckham,A.C. Milan
David Beckham,Preston North End F.C.
David Beckham,Real Madrid
David Beckham,Manchester United F.C.
Gary Lineker,England national football team
Gary Lineker,Leicester City F.C.
etc. etc.

{% endhighlight %}

into two matrices which connect players with players and teams with teams. I really should get better at Perl for this sort of stuff but alas… I haven’t the time.

## The Graphs

Since I’ve done a number of sports I’ve broken the next section down into the various sports I selected. In each category you’ll find two graphs. One connects players with players and the other connects teams with teams. Rather than commenting on each individual graph in turn, there are a few general observations I’ve made (let me know if I’ve missed something in the comments section):

* Clustered names usually will indicate an entire team. The more central a player name the more likely that person has been in a range of clubs with no major allegiances. People closer to only two or three isolated clusters of people will likely have strong ties to only the neighbouring clubs. The bigger the node, the more people they have played with over the course of their career. Keep in mind many players are still currently active and so their network is still being formed.

* Clustered clubs are a bit more interesting because they bring out some underlying structure. See if you can notice certain club types sticking together. In the more international sports the various colours will represent individual countries e.g. in soccer, English and German football teams cluster together because the players moving between the ranks usually belong to the country the club originates from.

* Knowledge of the players, teams and how they are related will probably allow you to get more out of the graph than I did. My sports knowledge is mediocre at best. Let me know if there is anything peculiar/interesting in the comments.
There may be a few names which look strange. Whenever you see a country, say ‘Italy’, that refers to the national team of that sport. I cut the labels down so it was more manageable. Hopefully they are self-explanatory. That reminds me, please let me know if there are duplicates of anything.

* Lastly, the size of the node in every graph has nothing whatsoever to do with the strength of the team or player in their respective sports! 

Please forgive the load times as some of the graphs are quite large.

### American Football
**Players**
<br/>
[![American Football Players](/assets/wikipedia/americanfootball_players.png)](/assets/wikipedia/americanfootball_players.png)
**Teams**
<br/>
[![American Football Teams](/assets/wikipedia/americanfootball_teams.png)](/assets/wikipedia/americanfootball_teams.png)
<br/>

### Basketball
**Players**
<br/>
[![Basketball Players](/assets/wikipedia/basketball_players.png)](/assets/wikipedia/basketball_players.png)
<br/>
**Teams**
<br/>
[![Basketball Teams](/assets/wikipedia/basketball_teams.png)](/assets/wikipedia/basketball_teams.png)
<br/>

### Ice Hockey
**Players**
<br/>
[![Ice Hockey Players](/assets/wikipedia/icehockey_players.png)](/assets/wikipedia/icehockey_players.png)
<br/>
**Teams**
<br/>
[![Ice Hockey Teams](/assets/wikipedia/icehockey_teams.png)](/assets/wikipedia/icehockey_teams.png)
<br/>

### Baseball
**Players**
<br/>
[![Baseball Players](/assets/wikipedia/baseball_players.png)](/assets/wikipedia/baseball_players.png)
<br/>
**Teams**
<br/>
[![Baseball Teams](/assets/wikipedia/baseball_teams.png)](/assets/wikipedia/baseball_teams.png)
<br/>

### Football (Soccer)
**Players**
<br/>
[![Soccer Players](/assets/wikipedia/soccer_players.png)](/assets/wikipedia/soccer_players.png)
<br/>
**Teams**
[![Soccer Teams](/assets/wikipedia/soccer_teams.png)](/assets/wikipedia/soccer_teams.png)
<br/>

## Caveats

As with all of my graphs, there are a few of important things to keep in mind:

* The datasets are incomplete. Many of your favourite players and teams could very likely be missing from the graph (especially non-Americans) – I’m sorry – I can’t do anything about that. This incompleteness will also lead to slight confusion as to what the various sized nodes actually mean. For the player graphs, the bigger the nodes, the most connections that person has to other people within the network. This essentially just means that the biggest nodes have shared the largest number of clubs with the largest number of people. Similarly for the club maps, the larger nodes are simply clubs which have the greatest reach in terms of the number of connections to other clubs through their current or previously players.

* The network is simply an exploration in connecting information. If you want to read facts or obtain clear cut answers to your questions about sports players and teams: go read Wikipedia or the original Freebase entries. This work, as with most of my others straddle a ground somewhere between entertainment and information. Where these networks fall, I do not know – I am at the mercy of you, the reader.