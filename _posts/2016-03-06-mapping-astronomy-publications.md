---
layout: post
title: "Mapping Astronomy Publications"
description: "Turning the affiliation list on papers into a map."
tags: [python, folium, maps, ads]
---

A casual Google search of the most recent earthquake events provides a long list of [interactive maps]((http://quakes.globalincidentmap.com/)). Generally they look like this:

![Quake Map](/assets/paperquake/quakemap.png "Quake Map")

This is a great way to get an instant understanding of the recent earthquake activity. Browsing one of these sites got me thinking about how I could connect this to astronomy papers. See, each paper in the literature has the following list of author affiliations:

[<img src="http://www.brendangriffen.com/assets/paperquake/aff.png">](http://adsabs.harvard.edu/cgi-bin/bib_query?arXiv:1509.01255)

This [Python ADS tool](https://github.com/andycasey/ads) enables me to scrape this affiliation information for all papers in the ADS database. I wanted to see if I could turn this list into a similar map to visualize the "pulse of research". My goal here is to create a map akin to the seismic map above where the size of the node is proportional to the number of papers from a given institute or university. 

I didn't want to spend much time developing the mapping library so I went searching for a library which has already taken care of the dirty work. I discovered [Folium](https://github.com/python-visualization/folium) which is fantastic for mapping and extremely versatile. It uses other tools such as [Leaflet](http://leafletjs.com/), [MapBox](https://www.mapbox.com/) and [OpenStreetMap](https://www.openstreetmap.org/#map=5/51.500/-0.100). I highly recommend you check them out if you're interested in digital cartography.

First I needed to get the papers that have been published last month.

```python
import ads
from datetime import date
today = date.today()
papers = list(ads.SearchQuery(pubdate='{0}-{1}'.format(today.year, today.month-1), 
                              property="refereed",
                              fl=['first_author','aff','author','bibcode'],
                              database='astronomy'))
```

I then cycled through all of the paper's affiliations and used [Geopy](https://github.com/geopy/geopy.git) to get the latitude and longitude. The tricky part is converting what are normally very specific affiliations into more general affiliations which can be parsed by Geopy. This took a lot more time than I would have liked. To clean the affiliations, I selected only those which contained the following strings: "depart", "instit", "observ", "laboratory", "univers". This limits the search to primarily universities, institutes, departments and observatories. Please let me know in the comments if there is a tag which might discount an important part of the dataset.

```python
for paper in papers:
    # use only unique affiliations
    unique_affiliations = list(set(paper.aff))
    for affiliation in unique_affiliations:
        addresses = affiliation.strip().split('; ')
        for address in addresses:
            clean_address = generate_searchname(address)
            location = geolocator.geocode(clean_address)
            print "Affiliation:      ",address
            print "Clean Affiliation:",clean_address[:80]
            print "Google Output:    ",location.address[:80]
            print "Lat/Long:           %.5f, %.5f" % (location.latitude, location.longitude)
```

This outputs the specific addresses into something which could be understood by [Geopy](https://github.com/geopy/geopy.git). Here are some examples of successful outputs:

```text
# successful addresses
Paper Aff:      Department of Geological Sciences, University of Michigan, Ann Arbor, MI, USA
After Cleaning: University of Michigan,  USA
Google Output:  University of Michigan, 500, Hayward Street, Old West Side, Ann Arbor, Washtenaw
Lat/Long:       42.29421, -83.71003
Paper Aff:      School of Earth Sciences and Resources, China University of Geosciences Beijing, Beijing 100083, China
After Cleaning: China University of Geosciences Beijing, China
Google Output:  中国地质大学（北京）, 29, 学院路, 海淀区, 后八家, 海淀区, 北京市, 100083, 中国
Lat/Long:       39.98851, 116.34070
Affiliation:    Université de Montpellier, France
Google Output:  UM2, Place Eugène Bataillon, Hôpitaux-Facultés, Montpellier, Hérault, Languedoc-
Latitude:       43.63230, 3.86427
Paper Aff:      Instituto de Astrofísica de Canarias, Vía Lácteas/n E-38205 La Laguna, Spain
After Cleaning: Instituto de Astrofísica de Canarias, Spain
Google Output:  IAC, Avenida de los Menceyes, La Hornera, Cercado Mesa, San Cristóbal de La Lagu
Lat/Long:       28.47470, -16.30822
Paper Aff:      Instituto de Astronomía,Universidad Nacional Autonóma de México, A.P. 70-264, 04510 México D.F., México
After Cleaning: Universidad Nacional Autonóma de México
Google Output:  UNAM, Circuito Exterior, Universidad Nacional Autónoma de México, Coyoacán, D.F.
Lat/Long:       19.32160, -99.18490
Paper Aff:      Institut für Raumfahrttechnik und Weltraumnutzung, Universität der Bundeswehr München, 85577 Neubiberg, Germany
After Cleaning: Universität der Bundeswehr München, Germany
Google Output:  Universitätsbibliothek der Universität der Bundeswehr München, 39, Werner-Heisen
Lat/Long:       48.08041, 11.63819
```

As you can see, there is quite a bit of success with the non-english institution names. I was worried it wouldn't return anything due to encoding issues. It didn't always work and I often couldn't find ways to code around the edge cases. There were a number of institutes/departments/centers which didn't always work as well.

```text
# failed addresses
Input: Variable Star Observers League in Japan (VSOLJ), Japan
Address: Variable Star Observers League in Japan (VSOLJ), 7-1 Kitahatsutomi, Kamagaya, Chiba 273-0126, Japan
Input: Netherlands Institute for Radio Astronomy (ASTRON)
Address: Research and Development, Netherlands Institute for Radio Astronomy (ASTRON)
Input: National Astronomical Observatory of Japan, Japan
Address: National Astronomical Observatory of Japan, 2-21-1 Osawa, Mitaka-shi, Tokyo 181-8588, Japan
Input: Woods Hole Oceanographic Institute,  USA
Address: Geology and Geophysics Department, Woods Hole Oceanographic Institute, Woods Hole, MA, USA
```

My code didn't every institute so I apologize if your paper isn't on the map. Some of the addresses seem perfectly normal and return good Google results but alas, they weren't picked up. The only hurdle remaining was to make sure that I didn't double count papers and collected papers which came from the same institute across multiple papers. Take for instance the Max Planck Institute papers from Garching:

```text
Max Planck Institut for Astrophysics, Garching,  Germany
Max Planck Institute for extraterrestrische Physik, Garching,  Germany
Max Planck Institute for Extraterrestrial Physics, Garching,  Germany
Max Planck Institute for extraterrestrische Physik, Garching,  Germany
Max Planck Institute fur Astrophysics, Garching,  Germany
Max Planck Institute for extraterrestrische Physik (MPE), Garching,  Germany
```

All of these had to be put in the same bin otherwise they would be separate institutes.  Once I took care of these edge cases and had the unique self-similar keys for each location I simply populated them with the relevant paper information (e.g. authors). Again, a little coding gymnastics was required to get this all in working order.

Once all the time-consuming part is finished, it was trivial to populate a map with the relevant information using Folium.

```python
import folium
# center the map above the equator.
pub_map = folium.Map(location=[30,0],zoom_start=2)
# set base circle
base = 1500
# loop through all the institutions and set the size of the circle based on the number of papers
for key in data.keys():    
    npapers = len(data[key]['bibcode'])
    marker_name = key.title() + " published " + str(npapers) + " paper(s)"
    folium.CircleMarker([data[key]['lat'], data[key]['long']],
                         radius=base*npapers,
                         color='#3186cc',
                         popup=marker_name,
                         fill_color='#3186cc').add_to(pub_map)

# write to file
pub_map.save('map.html') 
```
Click for either the [PNG](http://www.brendangriffen.com/assets/paperquake/paperquake.png) or [interactive](/assets/paperquake/map.html "Paper Quake!") versions. The image below also links to the interactive version.

[<img src="http://www.brendangriffen.com/assets/paperquake/paperquake.png">](http://brendangriffen.com/assets/paperquake/map.html)

It is kind of exciting to think that every one of these dots represents a new type of inquiry into how the Universe works. Unfortunately, I often narrow my sights to institutes that have people publishing in my own field. This map clearly shows my own ignorance -- there is a vast hive of activity happening all over the world.

I must confess this map only shows ~200 paper's worth of affiliations. I maxed out my API request to Google and couldn't get the full list for the month of February. Also, if the name isn't associated with the institute you select that is because it is the *first author* of the paper, not necessarily the person attached to that institute. Shoutout to [Jonny Elliot](mailto:jonathan.elliott@cfa.harvard.edu) at ADS for assisting with some query optimization.

This is all I had time for last night but one could in principle extend this map to look at the publications over different time periods (e.g. one year) or perhaps geographic clusterings of co-authorship (e.g. highlight nodes which are collaborating together). One could also improve the GUI to better investigate papers or institutes of particular interest. I leave these as an exercise to the reader. 
