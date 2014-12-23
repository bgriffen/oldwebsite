---
layout: post
title: "Making Spotify Playlists In Python Based On Upcoming Local Bands"
description: "Want to see some live music, but not sure if you'll like the band? Try this out."
tags: [python, spotify, music]
---

Today it is extremely easy to access music. The days of celebrating 4kb/s via Napster are long over. For a small fee or even for free, you can access large quantities of music, on demand. If you know of a band or song, it is easy to find it on Youtube or Spotify (or any of the other services). This is great if you *already know* the music you are looking for. What about for bands you haven't heard? Yes, there are ways to [discover new music based on current tastes](http://techcrunch.com/2012/12/06/spotify-following/) but what about something a little closer to home - what about finding new music which happens to be playing in your local area? 

# The Problem

Ordinarily you might go to a website which lists local bands. Then the process of copying and pasting the band names into your music provider manually to hear what sort of music they offered would follow. Many hours of human life are wasted all over the world in carrying out this monotonous process. Continually doing this led to my idea of automating this tedious process via a short and simple program.

# A Solution

A short program which finds bands with upcoming shows in your area and loads them into a Spotify playlist, auto-magically.

# The Data

My data source for bands is the well known, [Bowery Boston](http://www.boweryboston.com/see-all-shows/) which  promotes upcoming bands playing in the Boston area.

[![spotify-playlists](/assets/spotifylocalbands/bowery_boston.png)](/assets/spotifylocalbands/bowery_boston.png)

Using this list I created a Spotify playlist containing the top few tracks (as judged by Spotify) of each of the bands about to have a show. I now run this once a month and sync it to my phone. If there is any new music which catches my ear and the price is right, I'll go ahead and buy a ticket.

My only point of reference was [this nice post](https://mborgerson.com/creating-a-playlist-in-spotify-using-python/) by [Matt Borgerson](https://twitter.com/MattBorgerson). His [original code](https://github.com/mborgerson/spotify-playlist-from-csv) converted a csv file into a playlist - which is a great tool if you already know what bands *and* tracks you want to listen to. I had to design something slightly different.

# The Code

For this code to work you require Python, libspotify, the pyspotify bindings and critically a Premium Spotify account ($9.99/month, sorry). Alternatively, you can just scroll to the end of this post to get the Spotify link and I can updated the playlist for you. This code can be found at [this Github repository](https://github.com/bgriffen/spotifylocalbands).

{% highlight Python %}
import requests
import bs4
import spotify
import sys

"""
Contact: Brendan Griffen brendan.f.griffen@gmail.com @brendangriffen

Code will scrape bands playing in next few months in Boston area.

Requirements:

libspotify:     https://developer.spotify.com/technologies/libspotify/
pyspotify:      https://github.com/mopidy/pyspotify
developer key:  https://devaccount.spotify.com/my-account/keys/

Enjoy the tunes!

"""

session = spotify.Session()
session.login('username', 'password')

url_name = "http://www.boweryboston.com/see-all-shows/"

response = requests.get(url_name)
soup = bs4.BeautifulSoup(response.text)

# scrape the relevant information and put into a list
bands_split = [elm.a.text.split(",") for elm in soup.find_all('h1',class_='headliners summary')]

# don't forget the support acts!
supports = [elm.a.text for elm in soup.find_all('h2',class_='supports')]

bands = sum(bands_split,[])

all_bands = list(set(bands+supports))

print "Upcoming bands playing around Boston."

for band in all_bands:
    print band

# try a few times to connect
for i in xrange(0,5):
    session.process_events()

print
print "Adding TOP 3 songs of each band to a Spotify playlist..."
all_tracks = []
for band in all_bands:
    search = session.search(str(band))
    search.load()
    if len(search.tracks) > 0:
        # take the top 5 tracks
        for i,track in enumerate(search.tracks):
            if i <= 2: all_tracks.append(track)

print "Adding %i band, totaling %i tracks!" % (len(all_bands),len(all_tracks))
session.playlist_container.add_new_playlist("Upcoming LIVE Boston Music")
playlist = session.playlist_container[-1]

print "Adding tracks to:",playlist.name
playlist.add_tracks(all_tracks)
playlist.load()

print
print "Check your new playlist soon!"

# check the playlist now exists!
for playlisti in session.playlist_container:
    print playlisti.name

session.logout()

print
print "Check your new playlist soon!"

{% endhighlight %}

The terminal output will be something like the following:

{% highlight bash %}
Upcoming bands playing around Boston....
Wild Child
Future Islands
Caroline Smith
Rohan Padhye
Sylvan Esso
The Green
Sidewalk Driver
The Fagettes
Abhishek Shah
Front Porch Step
Cropduster
Preservation Hall Jazz Band
Sorority Noise
The Beautiful Ones
...
Adding TOP 3 songs of each band to a Spotify playlist...
Adding 222 band, totaling 541 tracks!
Adding tracks to playlist: Upcoming LIVE Boston Music
{% endhighlight %}

*small caveat: rarely the program will scrape the wrong band - please forgive me if it does.*

# The Result

Here is an example of what one of these playlists might look like inside Spotify. It may take a short while for it to show up (a restart sometimes helps).

[![spotify-playlists](/assets/spotifylocalbands/spotify_playlist.png)](/assets/spotifylocalbands/spotify_playlist.png)

# Take Home Message

I made [this playlist public](http://open.spotify.com/user/1254170771/playlist/5QKiOM9egThI6u6oXgkTNh) for those that aren't too comfortable programming so feel free to follow it if you are in the Boston area (this will work if you have a free account). I'll use my Premium account update it periodically =)

If you know of a website which compiles local music in your area perhaps you could try to modify what I've done and make your own playlist. In any case, I hope you enjoy the new music, soon to be playing live, somewhere near you.