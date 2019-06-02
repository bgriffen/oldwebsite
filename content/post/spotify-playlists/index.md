---
title: "Spotify Playlist Based On Local Gigs"
subtitle: 'Listen to bands playing in your area soon.'
summary: 
authors:
- admin
tags:
- python
- music
categories: []
date: "2014-12-22T00:00:00Z"
lastmod: "2014-12-22T00:00:00Z"
featured: false
draft: false
---


Today it is extremely easy to access music. The days of celebrating 4kb/s via Napster are long over. For a small fee or even for free, you can access large quantities of music, on demand. If you know of a band or song, it is easy to find it on Youtube or Spotify (or any of the other services). This is great if you *already know* the music you are looking for. What about for bands you haven't heard? Yes, there are ways to [discover new music based on current tastes](http://techcrunch.com/2012/12/06/spotify-following/) but what about something a little closer to home - what about finding new music which happens to be playing in your local area? 

Ordinarily you might go to a website which lists local bands. Then the process of copying and pasting the band names into your music provider manually to hear what sort of music they offered would follow. Many hours of human life are wasted all over the world in carrying out this monotonous process. Continually doing this led to my idea of automating this tedious process via a short and simple program.

I set about making a short program which finds bands with upcoming shows in your area and loads them into a Spotify playlist, auto-magically. My data source for bands is the well known, [Bowery Boston](http://www.boweryboston.com/see-all-shows/) which promotes upcoming bands playing in the Boston area. Skip to the bottom of the post for playlists for other cities.

[![spotify-playlists](./images/bowery_boston.png)](./images/bowery_boston.png)

Using this list I created a Spotify playlist containing the top few tracks (as judged by Spotify) of each of the bands about to have a show. I now run this once a month and sync it to my phone. If there is any new music which catches my ear and the price is right, I'll go ahead and buy a ticket.

My only point of reference was [this nice post](https://mborgerson.com/creating-a-playlist-in-spotify-using-python/) by [Matt Borgerson](https://twitter.com/MattBorgerson). His [original code](https://github.com/mborgerson/spotify-playlist-from-csv) converted a csv file into a playlist - which is a great tool if you already know what bands *and* tracks you want to listen to. I had to design something slightly different.

For the following code to work you require Python, libspotify, the pyspotify bindings and critically a Premium Spotify account (sorry!). Alternatively, if you have the free service but still want this content, you can just add this [Spotify link](https://open.spotify.com/user/1254170771/playlist/1Shh4ljWPQrcsvpTKtppm5) which will updated the playlist for you. This code can be found at [this Github repository](https://github.com/bgriffen/spotifylocalbands).

```python
import requests
import bs4
import spotify
import sys
import threading

"""
Requirements:
libspotify:     https://developer.spotify.com/technologies/libspotify/
pyspotify:      https://github.com/mopidy/pyspotify
developer key:  https://devaccount.spotify.com/my-account/keys/

Enjoy the tunes!

"""

logged_in_event = threading.Event()

def connection_state_listener(session):
    if session.connection.state is spotify.ConnectionState.LOGGED_IN:
        logged_in_event.set()
        session = spotify.Session()
        
session = spotify.Session()
session.on(spotify.SessionEvent.CONNECTION_STATE_UPDATED,connection_state_listener)
session.login('username','password')

# somtimes you won't be able to be logged in the first item around
while not logged_in_event.wait(0.1):
     session.process_events()

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
print
print "Adding TOP 3 songs of each band to a Spotify playlist..."
all_tracks = []
for band in all_bands:
    try:
        if len(band) != 0:
            search = session.search(str(band))
            search.load()
            if len(search.tracks) > 0:
                # take the top 5 tracks
                for i,track in enumerate(search.tracks):
                    if i <= 2:
                        all_tracks.append(track)
    except UnicodeEncodeError:
        print "UNICODE ERROR - Can't add:",band

# check if you've already made this playlist before
exists = False
for i,playlisti in enumerate(session.playlist_container):
    print playlisti.name
    if "Upcoming LIVE Boston Music" in playlisti.name:
        exists = True
        playlisti.remove_tracks(np.arange(len(playlisti.tracks)))
        print "Playlist already exists - removing and updating with new tracks!"

if not exists:
    print "Adding %i band, totalling %i tracks!" % (len(all_bands),len(all_tracks))
    session.playlist_container.add_new_playlist("Upcoming LIVE Boston Music")
    playlisti = session.playlist_container[-1]

print "Adding tracks to:",playlisti.name
playlisti.add_tracks(all_tracks)
playlisti.load()
session.logout()

print "Check your new playlist soon!"

```

The terminal output will be something like the following:

```bash
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
```

*small caveat: rarely the program will scrape the wrong band - please forgive me if it does.*

Here is an example of what one of these playlists might look like inside Spotify. It may take a short while for it to show up (a restart sometimes helps).

[![spotify-playlists](./images/spotify_playlist.png)](./images/spotify_playlist.png)

Or you can try it out directly!

<center>
<iframe src="https://embed.spotify.com/?uri=spotify%3Auser%3A1254170771%3Aplaylist%3A1Shh4ljWPQrcsvpTKtppm5" width="300" height="380" frameborder="0" allowtransparency="true"></iframe>  
</center>  

Again, if you don't have any programming experience or don't have a Premium Spotify account [this playlist](https://open.spotify.com/user/1254170771/playlist/1Shh4ljWPQrcsvpTKtppm5) is public if you still want the playlist for the Boston area (this will work if you have a free account). I'll use my account update it at the start of the every month.

If you know of a website which compiles local music in your area perhaps you could try to modify what I've done and make your own playlist. In any case, I hope you enjoy the new music, soon to be playing live, somewhere near you.

By popular requests, I've added playlists here for:

**Australia**

* [Brisbane](https://open.spotify.com/user/1254170771/playlist/2NTVU0043xOyybaDw65rPl)  
* [Sydney](https://open.spotify.com/user/1254170771/playlist/0xjEwsrg3BWL97kNR5JWny)  
* [Melbourne](https://open.spotify.com/user/1254170771/playlist/5AUh7aL3Wvjgi85a6lyVD9)

**USA**  

* [Madison](https://open.spotify.com/user/1254170771/playlist/2DaCoNbB5ICwXaVGBX7mvo)
* [Boston](https://open.spotify.com/user/1254170771/playlist/1Shh4ljWPQrcsvpTKtppm5)  

Unlike the Bowery Boston playlist generated above, these are based on [Songkick](https://www.songkick.com/home) bands. Let me know below if you want some other cities.
