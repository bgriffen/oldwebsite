---
layout: post
title: "Music and Mortality"
description: "Does the genre of an artist determine their lifespan?"
tags: [python, SPARQL, DBpedia, wikipedia, musicians]
---
The [27-club](https://en.wikipedia.org/wiki/27_Club) is a group of musicians who have died at the age of 27. Charles Cross, a Hendrix biographer put it as follows:

> The number of musicians who died at 27 is truly remarkable by any standard. [Although] humans die regularly at all ages, there is a statistical spike for musicians who die at 27.

There are a large number of famous musicians who have all died at the age of 27 (e.g. Brian Jones, Alan Wilson, Jimi Hendrix, Janis Joplin and Jim Morrison to name a few). Though, is this just bias on our part or does the data stack up? I decided to investigate the validity of this claim and dig a little deeper combining lifespan statistics within the classic genres, jazz, blues, rock, metal, folk and classical. I then combined the artist information with Last.fm's API to get a proxy for the popularity of an artist now and see if there is any correlation with their lifespan. Do you tend to live a shorter life if you're more popular? If you don't have the time to read the whole post, here are my main conclusions:

* classical musicians live the longest on average
* metal musicians live the shortest on average
* all musicians seem to live shorter lives than average
* there is a very weak trend indicating that the more populare you are, the shorter your life span.
* there is no "27-club" when looking at the entire lifespans of the musicians in any genre (possible origins: romanticism)

Read on if you would like to learn how I came to these conclusions.

## The Data

I needed a few things to tackle this problem. A list of artists and their known life spans and also some information about their popularity. I had already mined Wikipedia via DBpedia so I went back through some old code. You can execute it in the [Virtuoso SPARQL Query Editory](http://dbpedia.org/sparql).

{% highlight SQL %}

SELECT DISTINCT str(?plabel) str(?glabel) 
       ?died ?born
       ( 
         IF 
           (
             ( datatype (?born) in (xsd:dateTime, xsd:date) )
             and
             ( datatype (?died) in (xsd:dateTime, xsd:date) ),
             bif:datediff('year',xsd:dateTime(str(?born)),xsd:dateTime(str(?died))),
             "error" 
           ) 
       ) AS ?age
WHERE 
  { 
    {
      SELECT DISTINCT ?person ?plabel ?genre ?glabel ?died ?born 
      FROM <http://dbpedia.org> 
      WHERE 
        { 
          ?person a <http://dbpedia.org/ontology/MusicalArtist> ;
                     <http://dbpedia.org/ontology/genre> ?genre ;
                  <http://dbpedia.org/ontology/deathDate> ?died ;
                                             rdfs:label ?plabel ;
                  <http://dbpedia.org/ontology/birthDate> ?born .
          ?genre rdfs:label ?glabel .
          FILTER ( lang(?plabel) = "en" )
          FILTER ( lang(?glabel) = "en" )
        }
      ORDER BY DESC ( <LONG::IRI_RANK> (?person) )
    }
  }

{% endhighlight %}

This code extracts our the known information about the musicians and results in the following output:

{% highlight Text %}
...
"Michael Jackson","Pop music",2009-06-25,1958-08-29
"Michael Jackson","Rock music",2009-06-25,1958-08-29
"Michael Jackson","Soul music",2009-06-25,1958-08-29
"Michael Jackson","Disco",2009-06-25,1958-08-29
"Michael Jackson","Funk",2009-06-25,1958-08-29
"Michael Jackson","Rhythm and blues",2009-06-25,1958-08-29
"Michael Jackson","New jack swing",2009-06-25,1958-08-29
"Elvis Presley","Gospel music",1977-08-16,1935-01-08
"Elvis Presley","Pop music",1977-08-16,1935-01-08
"Elvis Presley","Blues",1977-08-16,1935-01-08
"Elvis Presley","Country music",1977-08-16,1935-01-08
"Elvis Presley","Rhythm and blues",1977-08-16,1935-01-08
"Elvis Presley","Rock and roll",1977-08-16,1935-01-08
"Elvis Presley","Rockabilly",1977-08-16,1935-01-08
"George Harrison","Pop music",2001-11-29,1943-02-25
...
9209 rows
{% endhighlight %}

I then had to get a way to find out how popular they were so I just searched through Spotify, Last.fm etc. until I found a convenient API and companion module which allowed me to get play counts. It turns out [Last.fm](http://www.last.fm/) has [a nice API](http://www.last.fm/api) which has been made digestible in a [python-lastfm module](https://code.google.com/p/python-lastfm/). First I had to load them into Python which was slightly non-trivial as there are some odd names to deal with. The following code got most of them except for 79 artists which had birth/death dates which were incomplete.

{% highlight Python %}
with open(filename) as f:
    for line in f:
        line_split = line.split(',')
        # this dirty code correctly splits unusual names (e.g. Jr.)
        if "Jr." in line_split[1] or "Sr." in line_split[1]:
            nametmp = line_split[0]+''+line_split[1]
            inx = 1
        else:
            nametmp = line_split[0].replace("(musician)","")
            nametmp = line_split[0].replace("(singer)","")
            
        inx = 0

        byear = line_split[2+inx].replace('"',"")
        dyear = line_split[3+inx].replace('"',"")
        try:
            # datetime module only works with dates later than 1900 :(
            if int(dyear.split('-')[0]) > 1900 and int(dyear.split('-')[0]) > 1900:
                birthdate = datetime.strptime(byear,"%Y-%m-%d")
                deathdate = datetime.strptime(dyear,"%Y-%m-%d")
                lifespan = relativedelta(birthdate,deathdate).years

                # check they are not still alive
                if birthdate.year  == deathdate.year  or lifespan < 5:
                    print "STILL ALIVE!", nametmp,lifespan
                elif birthdate.year != deathdate.year and lifespan > 7:
                    # now we are just dealing with the dead ones
                    lifespan = relativedelta(birthdate,deathdate).years
                    birthdates.append(birthdate)
                    deathdates.append(deathdate)
    
                    ages.append(lifespan)
                    birth.append(line_split[2+inx])
                    death.append(line_split[3+inx])

                    # I also checked month/day distributions for fun
                    deathday.append(deathdate.strftime("%A"))
                    birthday.append(birthdate.strftime("%A"))
                    deathmonth.append(deathdate.strftime("%B"))
                    birthmonth.append(birthdate.strftime("%B"))

                    artists.append(nametmp.strip('"'))
                    genres.append(line_split[1+inx].strip('"'))
                    
        except ValueError:
            print "VALUE ERROR"
            missed += 1
{% endhighlight %}

I then built a dictionary over each genre I wanted to include. I'm absolutely certain there are better ways of doing this but I didn't have time to try more intelligent methods (suggestions welcome).

{% highlight Python %}
# initialize dictionary
averageagelist = {'rock':[],'jazz':[],'folk':[],'metal':[],'blues':[],'classical':[],'pop':[],'misc':[]}
#... and other dictionaries to be used

for i in xrange(0,len(artists)):
    found = False
    # get list of genres a given artist belongs to
    catuselist = []
    for cat in categories:
        if cat in genres[i]:
            found = True
            catuselist.append(cat)

    if not found:
        catuselist = ['misc']

    # update respective list in dictionary
    for shortname in catuselist:        
        averageagelist[shortname].append(ages[i])
        birthlist[shortname].append(birth[i])
        birthmonthlist[shortname].append(birthmonth[i])
        birthdaylist[shortname].append(birthday[i])
        deathlist[shortname].append(death[i])
        deathmonthlist[shortname].append(deathmonth[i])
        deathdaylist[shortname].append(deathday[i])


{% endhighlight %}

Now all that remains is to connect each artist to Last.fm to get our final piece of data. I also had to feed in appropriate names which required a minor modification to their default names which come from DBpedia.

{% highlight Python %}
import lastfm
api_key='insert-your-key-here'
api = lastfm.Api(api_key)

fmnotcount = 0
uniqueartists = list(set(artists))
f = open('fmartistcount','w')
for artist in uniqueartists:
    try:
        fmartist = api.get_artist(artist.split('(')[0])
        f.write(artist+','+str(fmartist.stats.playcount)+','+str(fmartist.top_tag)+','+fmartist.image['large']+'\n')
        print artist.split('(')[0],"playcount",fmartist.stats.playcount
    except:
        fmnotcount+=1
        print "- could not find:",artist

f.close()
{% endhighlight %}

Then we just need to plot the relevant dictionary.

{% highlight Python %}
fig = plt.figure()
ax = fig.add_subplot(111)
ax.bar(*zip(*zip(count(), averageage.values())))
plt.xticks(*zip(*zip(count(0.4), averageage)))
plt.show()
{% endhighlight %}

## Results

### Average Lifespan For Artists In Each Genre

Let's just have a look at the life span for each genre with the associated standard deviation.

<table style="font-size: 90%; text-align: center">
<tr>
<th>Genre</th><th>Sample</th><th>Median</th><th>Mean</th><th>STD</th>
</tr>
<tr>
<td>classical</td><td>76</td><td>71</td><td>68</td><td>19</td>
</tr>
<tr>
<td>jazz</td><td>268</td><td>67</td><td>65</td><td>16</td>
</tr>
<tr>
<td>blues</td><td>774</td><td>64</td><td>61</td><td>16</td>
</tr>
<tr>
<td>pop</td><td>178</td><td>63</td><td>59</td><td>20</td>
</tr>
<tr>
<td>folk</td><td>20</td><td>53</td><td>53</td><td>13</td>
</tr>
<tr>
<td>rock</td><td>794</td><td>47</td><td>46</td><td>13</td>
</tr>
<tr>
<td>metal</td><td>196</td><td>39</td><td>39</td><td>9</td>
</tr>
<tr>
<td>misc</td><td>6190</td><td>61</td><td>59</td><td>18</td>
</tr>
</table>

Clearly the majority of the artists are in the 'misc' or miscellaneous category. I don't have the time to parse these into the appropriate category so what I present is only a sample dataset of the whole. Feel free to play with the dataset and improve on the classification mechanisms I employed. 

Interestingly, **classical musicians live the longest** on average and **metal musicians live the shortest** on average (the difference between a whopping 29 years longer!). Genres like jazz and blues also have longer life-spans compared to the more 'labor intensive' rock and metal (which somewhat confirms the ["better to burn out than fade away"](http://youtu.be/LQ123T3zD2k) mantra). Conversely it seems the lifestyle of jazz and classical musicians lead to longer lives. It is important to note that there are only 20 folk musicians in this sample.

<br/>
![Average Lifespan For All Genres](/assets/musicianmortality/all-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Blues](/assets/musicianmortality/blues-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Classical](/assets/musicianmortality/classical-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Folk](/assets/musicianmortality/folk-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Jazz](/assets/musicianmortality/jazz-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Metal](/assets/musicianmortality/metal-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Pop](/assets/musicianmortality/pop-lifespan-hist.png)
<br/>

<br/>
![Average Lifespan For Rock](/assets/musicianmortality/rock-lifespan-hist.png)
<br/>

### Youngest and Oldest
Rank ordering the youngest and oldest musicians in the list we find:

{% highlight Text %}
TOP 5 YOUNGEST MUSICIANS
Ritchie Valens 17
Yukiko Okada 18
John Spence 18
Nick Traina 19
Yaki Kadafi 19
{% endhighlight %}

{% highlight Text %}
TOP 5 OLDEST MUSICIANS
Huey Long  105
Wade Mainer 104
Bill Tapia 103
Orlando Cole 101
Roman Totenberg 101
{% endhighlight %}

I noticed some tragic names in the youngest list: John Spence was the front man for *No Doubt* before he took his own life due to the pressure he put on himself. Yukiko Okada was a Japanese pop singer who also took her own life. Ritchie Valens also died tragically in a plane crash in 1959 with the Big Bopper, Buddy Holly and Roger Peterson (aka *the day the music died*). Nick Traina, the lead singer for the punk band *Link 80* also died of a self-induced morphine overdose. From the longer list of young artists I generated my own little play-list to see what these youngsters produced before they shuffled off this mortal coil.

### 27 club?
Using this data we can immediately answer the original question. If 27 was indeed a significant age, one of the histograms above should have shown this when clearly they don't. When I do extract out all musicians who had a life span equal to 27 I get the following list of 34 names:

{% highlight Text %}
Rudy Lewis
Freaky Tah
D. Boon
Kurt Cobain
Janis Joplin
Valentín Elizalde
Fat Pat
Louis Chauvin
Leslie Harvey
Mia Zapata
Jim Morrison
Ron "Pigpen" McKernan
Rockin' Robin Roberts
Alan Wilson 
Chris Austin
Jeremy Michael Ward
Richey Edwards
Chris Bell 
Brian Jones
Seagram
Dave Alexander 
Bob Gordon
Kami 
André Pretorius 
Jacob Miller
Rodrigo 
Amy Winehouse
Doug Watkins
Amar Singh Chamkila
Pete Ham
Kristen Pfaff
Jimi Hendrix
Alexander Bashlachev
Robert Johnson
{% endhighlight %}

If we compare it to the [actual list on Wikipedia](https://en.wikipedia.org/wiki/27_Club) (which the data should reflect) we find most of the names in that list. The astute reader will note that there are 44 names on Wikipedia and this is in fact larger than what I get via DBpedia. One example is the not so successful rock and roll singer [Dickie Pride](https://en.wikipedia.org/wiki/Dickie_Pride) -- I checked my raw data and indeed, he does not exist. This is most likely because DBpedia hasn't indexed his page yet (though the page has been [around since 2008](https://en.wikipedia.org/w/index.php?title=Dickie_Pride&dir=prev&action=history)). This highlights the fact that the data, whilst rich, is ultimately incomplete. In time, DBpedia will fill out most of these darker corners.

### Popularity and Lifespan
I connected up the names to the Last.fm database and used the number of unique plays as a proxy for *popularity*. Doing this for the matches it could find I generated a scatter plot of how popularity relates to lifespan. Note that the y-axis is log meaning it goes between 1 count on last.fm up to 1,000,000,000 (10^9).

<br/>
![last-fm popularity](/assets/musicianmortality/lastfm-popularity-lifespan.png)
<br/>

Feeding this into scikits.statsmodels in Python we get a bunch of statistics about the regression.

{% highlight Bash %}
                            OLS Regression Results                            
==============================================================================
Dep. Variable:                      y   R-squared:                       0.005
Model:                            OLS   Adj. R-squared:                  0.005
Method:                 Least Squares   F-statistic:                     13.78
Date:                Fri, 28 Mar 2014   Prob (F-statistic):           0.000210
Time:                        01:40:22   Log-Likelihood:                -4270.3
No. Observations:                2557   AIC:                             8545.
Df Residuals:                    2555   BIC:                             8556.
Df Model:                           1                                         
==============================================================================
                 coef    std err          t      P>|t|      [95.0% Conf. Int.]
------------------------------------------------------------------------------
x1            -0.0052      0.001     -3.712      0.000        -0.008    -0.002
const          4.6365      0.088     52.401      0.000         4.463     4.810
==============================================================================
Omnibus:                       11.340   Durbin-Watson:                   1.929
Prob(Omnibus):                  0.003   Jarque-Bera (JB):               11.349
Skew:                          -0.161   Prob(JB):                      0.00343
Kurtosis:                       3.050   Cond. No.                         218.
==============================================================================
{% endhighlight %}

There is a weak trend indicating that **the more popular you are, the shorter your lifespan**. Does this line up with stereotypes about the hard and fast lifestyle of many musicians? Maybe.

### Average Lifespan For Musicians Over The Last 50 Years
If we look at average lifespan for each genre broken up into bins of birth, we see how the average lifespan has changed over time for each genre.

<br/>
![Average lifespan in each genre over time.](/assets/musicianmortality/year-lifespan.png)
<br/>

The data fluctuates quite a bit but the overall trend is upwards though still below the mean of the U.S. population average (dashed). It seems any way you cut it, musicians live shorter lives than the average population. Clearly the metal heads aren't doing so well.

## Findings
* classical musicians live the longest on average
* metal musicians live the shortest on average
* all musicians seem to live shorter lives than average
* there is a small trend when comparing popularity to lifespan; the the more popular you are, the shorter your lifespan (highly speculative)
* there is no "27-club" when looking at the entire lifespans of the musicians in any genre (possible origins: romanticism)

## Caveats

### Last.fm
Popularity is not just the *play counts* on Last.fm. Things that are popular now are by no means how they were in the previous decades. Can you guess what song was at number 1 in 1969? The Beatles? James Brown? Beach Boys? It was *Archies - Sugar Sugar*. Clearly this demonstrates 'good music' now is not what was on everyone's mind during the X0s. I don't really have a way to measure popularity as a function of time via any other source so I just went with what I had. Please suggest other datasets which might help me out here.

### Wikipedia 
Wikipedia's content is made by people like you and me. It has its own biases but I would think for this project they don't really effect the dataset. With zero data to back me up, Wikipedia probably has 90% of all well known musicians in its database (lower limit). Since music is a relatively integral part of people's lives, it stands to reason that the dataset is quite comprehensive and accurate (remember I'm only getting the birth/death dates here).

### Transcription

Transcription errors could be introduced when trying to get the play count from Last.fm. I simply don't have the time to check every single name being fed through the API. Strange names may not return a result and so if you find some people missing then feel free to improve my code. I'm also not sure how it deals with the fact that some artists belong to certain bands and may not have a solo career. This will undervalue their popularity on the whole if Last.fm doesn't interpret it correctly. Also Last.fm may have only recently added certain artists and different artists get added at different times. Over sufficiently long time scales this would be washed out but who knows when each of the artists I'm using were added and so this must be taken into consideration.

### Genre Inception Offset

Since classical music has been around much longer than say folk or metal, lifespans will be more accurate and will be with less selection bias. Metal music has been around since the early 1970s and so the maximum the lifespan of a deceased metal musician will be (~40 + maturation age, ~18?) ~58 years. Indeed this is what we see in the distribution of ages. The same goes for all of the other genres (folk slightly older and jazz slightly older still).
<br>
<br>
The overall message I want to convey is that you can test certain cultural sentiments using online data. This is obviously the tip of the iceberg so feel free to take what I have done and build on it - who knows what you might find out.


