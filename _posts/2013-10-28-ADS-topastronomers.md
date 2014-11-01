---
layout: post
title: "Top 100 Astronomers By Citation"
description: "An investigation into ADS astronomers."
tags: [python, astronomy, ADS, citation, API]
---

<figure class="full">
    <a href="/assets/astronomers-940x390.jpg" title="How many do you know?"><img src="/assets/astronomers-940x390.jpg"></a>
</figure>  
<br/>
How many of the faces above do you recognise? For a long time, I’ve wanted to investigate the the “who’s who” of astronomy via the literature but haven’t had the means. I’ve teamed up with [Andy Casey](http://astrowizici.st/) who has written a neat [ADS-python](https://github.com/andycasey/ads-python) module to access the [ADS publication database](http://adsabs.harvard.edu/abstract_service.html). It is still early days at the moment so we’ve mainly been brainstorming but we figured we should start with the basics: most cited astronomers. There will be more posts to follow as we have more ideas under construction.

For this exercise I just needed the raw list of names and citation count. If you aren’t familiar with the citation system, I suggest you head [here](https://en.wikipedia.org/wiki/Citation). To make it a bit more interesting I’ve piped my results through Google images and used some face recognition code to extract out the astronomer profiles. The [initial script](https://github.com/andycasey/ads-python/blob/master/examples/top-cited-astronomers.py) to get the list was constructed by Andy and I added some extra features (i.e. face recognition and downloading images from Google). I also wrote some code which scrapes meta-information from websites in which the astronomer appears to get more information than ADS provides. I’ll discuss this more in future posts.

If you clone the initial script and simply add the following snippet, it will search Google images and download the first 3 images it finds.

{% highlight Python %}
for j, astronomer in enumerate(most_successful_astronomers, 1):
    searchTerm = astronomer
    searchTerm = searchTerm.replace(' ','%20')
 
    count= 0
 
    for i in range(0,1):
        url = ('https://ajax.googleapis.com/ajax/services/search/images?' +
               'v=1.0&q='+searchTerm+'&start='+str(i*4)+'&userip=MyIP')
        #print url
        request = urllib2.Request(url, None, {'Referer': 'testing'})
        response = urllib2.urlopen(request)
 
        # Get results using JSON
        try:
            results = simplejson.load(response)
            data = results['responseData']
            dataInfo = data['results']
        except TypeError:
            pass
 
        for i,myUrl in enumerate(dataInfo):
            if i < numwanted:
                count = count + 1
                try:
                    myopener.retrieve(myUrl['unescapedUrl'],\
                        './astronomers/'+str(j)+'-'+astronomer.split(',')[0]+'-'+str(i)+'.jpg')
                except IOError or TypeError:
                    pass
 
        time.sleep(1.5)
{% endhighlight %}

Without further ado, I present the Top 100 most cited astronomers as of today. Images are shown where Google could find them.

<table style="font-size: 90%; text-align: center">
<tr>
<th>Rank</th><th>Astronomer</th><th>Image</th><th>Citations</th>
</tr>
<tr>
<td>1</td><td>Spitzer, Lyman</td><td><img src="/assets/astronomers/Spitzer.jpg " alt="Spitzer, Lyman" style="width:75px"></td><td>23096</td>
</tr>
<tr>
<td>2</td><td>Chandrasekhar, Subrahmanyan</td><td><img src="/assets/astronomers/Chandrasekhar.jpg " alt="Chandrasekhar, Subrahmanyan" style="width:75px"></td><td>21467</td>
</tr>
<tr>
<td>3</td><td>Hawking, S. W.</td><td><img src="/assets/astronomers/Hawking.jpg " alt="Hawking, S. W." style="width:75px"></td><td>20456</td>
</tr>
<tr>
<td>4</td><td>Press, William H.</td><td><img src="/assets/astronomers/Press.jpg " alt="Press, William H." style="width:75px"></td><td>20431</td>
</tr>
<tr>
<td>5</td><td>Abramowitz, Milton</td><td><img src="/assets/astronomers/Abramowitz.jpg " alt="Abramowitz, Milton" style="width:75px"></td><td>18802</td>
</tr>
<tr>
<td>6</td><td>Peebles, Phillip James Edwin</td><td><img src="/assets/astronomers/Peebles.jpg " alt="Peebles, Phillip James Edwin" style="width:75px"></td><td>15891</td>
</tr>
<tr>
<td>7</td><td>Parker, E. N.</td><td><img src="/assets/astronomers/Parker.jpg " alt="Parker, E. N." style="width:75px"></td><td>15649</td>
</tr>
<tr>
<td>8</td><td>Riess, Adam G.</td><td><img src="/assets/astronomers/Riess.jpg " alt="Riess, Adam G." style="width:75px"></td><td>14557</td>
</tr>
<tr>
<td>9</td><td>Weinberg, Steven</td><td><img src="/assets/astronomers/Weinberg.jpg " alt="Weinberg, Steven" style="width:75px"></td><td>13431</td>
</tr>
<tr>
<td>10</td><td>Draine, B. T.</td><td><img src="/assets/astronomers/Draine.jpg " alt="Draine, B. T." style="width:75px"></td><td>13385</td>
</tr>
<tr>
<td>11</td><td>Spergel, D. N.</td><td><img src="/assets/astronomers/Spergel.jpg " alt="Spergel, D. N." style="width:75px"></td><td>13225</td>
</tr>
<tr>
<td>12</td><td>Goldreich, Peter</td><td><img src="/assets/astronomers/Goldreich.jpg " alt="Goldreich, Peter" style="width:75px"></td><td>13174</td>
</tr>
<tr>
<td>13</td><td>Landau, Lev Davidovich</td><td><img src="/assets/astronomers/Landau.jpg " alt="Landau, Lev Davidovich" style="width:75px"></td><td>12915</td>
</tr>
<tr>
<td>14</td><td>Toomre, Alar</td><td><img src="/assets/astronomers/Toomre.jpg " alt="Toomre, Alar" style="width:75px"></td><td>12894</td>
</tr>
<tr>
<td>15</td><td>Woosley, S. E.</td><td><img src="/assets/astronomers/Woosley.jpg " alt="Woosley, S. E." style="width:75px"></td><td>12684</td>
</tr>
<tr>
<td>16</td><td>Blandford, R. D.</td><td><img src="/assets/astronomers/Blandford.jpg " alt="Blandford, R. D." style="width:75px"></td><td>12092</td>
</tr>
<tr>
<td>17</td><td>Osterbrock, Donald E.</td><td><img src="/assets/astronomers/Osterbrock.jpg " alt="Osterbrock, Donald E." style="width:75px"></td><td>11707</td>
</tr>
<tr>
<td>18</td><td>Binney, James</td><td><img src="/assets/astronomers/Binney.jpg " alt="Binney, James" style="width:75px"></td><td>11567</td>
</tr>
<tr>
<td>19</td><td>Navarro, Julio F.</td><td><img src="/assets/astronomers/Navarro.jpg " alt="Navarro, Julio F." style="width:75px"></td><td>11525</td>
</tr>
<tr>
<td>20</td><td>Johnson, Harold L.</td><td><img src="/assets/astronomers/Johnson.jpg " alt="Johnson, Harold L." style="width:75px"></td><td>10876</td>
</tr>
<tr>
<td>21</td><td>Perlmutter, S.</td><td><img src="/assets/astronomers/Perlmutter.jpg " alt="Perlmutter, S." style="width:75px"></td><td>10332</td>
</tr>
<tr>
<td>22</td><td>Komatsu, E.</td><td><img src="/assets/astronomers/Komatsu.jpg " alt="Komatsu, E." style="width:75px"></td><td>10157</td>
</tr>
<tr>
<td>23</td><td>Springel, Volker</td><td><img src="/assets/astronomers/Springel.jpg " alt="Springel, Volker" style="width:75px"></td><td>9690</td>
</tr>
<tr>
<td>24</td><td>Randall, Lisa</td><td><img src="/assets/astronomers/Randall.jpg " alt="Randall, Lisa" style="width:75px"></td><td>9530</td>
</tr>
<tr>
<td>25</td><td>Dressler, A.</td><td><img src="/assets/astronomers/Dressler.jpg " alt="Dressler, A." style="width:75px"></td><td>9460</td>
</tr>
<tr>
<td>26</td><td>Condon, J. J.</td><td><img src="/assets/astronomers/Condon.jpg " alt="Condon, J. J." style="width:75px"></td><td>9211</td>
</tr>
<tr>
<td>27</td><td>Tegmark, Max</td><td><img src="/assets/astronomers/Tegmark.jpg " alt="Tegmark, Max" style="width:75px"></td><td>9142</td>
</tr>
<tr>
<td>28</td><td>Anders, Edward</td><td><img src="/assets/astronomers/Anders.jpg " alt="Anders, Edward" style="width:75px"></td><td>8973</td>
</tr>
<tr>
<td>29</td><td>Eggen, O. J.</td><td><img src="/assets/astronomers/Eggen.jpg " alt="Eggen, O. J." style="width:75px"></td><td>8723</td>
</tr>
<tr>
<td>30</td><td>Bekenstein, Jacob D.</td><td><img src="/assets/astronomers/Bekenstein.jpg " alt="Bekenstein, Jacob D." style="width:75px"></td><td>8624</td>
</tr>
<tr>
<td>31</td><td>Schlegel, David J.</td><td><img src="/assets/astronomers/Schlegel.jpg " alt="Schlegel, David J." style="width:75px"></td><td>8576</td>
</tr>
<tr>
<td>32</td><td>Linde, A. D.</td><td><img src="/assets/astronomers/Linde.jpg " alt="Linde, A. D." style="width:75px"></td><td>8343</td>
</tr>
<tr>
<td>33</td><td>Gunn, James E.</td><td><img src="/assets/astronomers/Gunn.jpg " alt="Gunn, James E." style="width:75px"></td><td>8296</td>
</tr>
<tr>
<td>34</td><td>Fukugita, M.</td><td><img src="/assets/astronomers/Fukugita.jpg " alt="Fukugita, M." style="width:75px"></td><td>8205</td>
</tr>
<tr>
<td>35</td><td>Rieke, G. H.</td><td><img src="/assets/astronomers/Rieke.jpg " alt="Rieke, G. H." style="width:75px"></td><td>8200</td>
</tr>
<tr>
<td>36</td><td>Bessell, M. S.</td><td><img src="/assets/astronomers/Bessell.jpg " alt="Bessell, M. S." style="width:75px"></td><td>7824</td>
</tr>
<tr>
<td>37</td><td>Kurucz, R. L.</td><td><img src="/assets/astronomers/Kurucz.jpg " alt="Kurucz, R. L." style="width:75px"></td><td>7818</td>
</tr>
<tr>
<td>38</td><td>Allen, C. W.</td><td><img src="/assets/astronomers/Allen.jpg " alt="Allen, C. W." style="width:75px"></td><td>7789</td>
</tr>
<tr>
<td>39</td><td>Stetson, Peter B.</td><td><img src="/assets/astronomers/Stetson.jpg " alt="Stetson, Peter B." style="width:75px"></td><td>7736</td>
</tr>
<tr>
<td>40</td><td>Gradshteyn, I. S.</td><td><img src="/assets/astronomers/Gradshteyn.jpg " alt="Gradshteyn, I. S." style="width:75px"></td><td>7667</td>
</tr>
<tr>
<td>41</td><td>Dziewonski, Adam M.</td><td><img src="/assets/astronomers/Dziewonski.jpg " alt="Dziewonski, Adam M." style="width:75px"></td><td>7662</td>
</tr>
<tr>
<td>42</td><td>Davis, M.</td><td><img src="/assets/astronomers/Davis.jpg " alt="Davis, M." style="width:75px"></td><td>7352</td>
</tr>
<tr>
<td>43</td><td>Perdew, J. P.</td><td><img src="/assets/astronomers/Perdew.jpg " alt="Perdew, J. P." style="width:75px"></td><td>7337</td>
</tr>
<tr>
<td>44</td><td>Mihalas, Dimitri</td><td><img src="/assets/astronomers/Mihalas.jpg " alt="Mihalas, Dimitri" style="width:75px"></td><td>7284</td>
</tr>
<tr>
<td>45</td><td>Guth, Alan H.</td><td><img src="/assets/astronomers/Guth.jpg " alt="Guth, Alan H." style="width:75px"></td><td>7275</td>
</tr>
<tr>
<td>46</td><td>Padmanabhan, T.</td><td><img src="/assets/astronomers/Padmanabhan.jpg " alt="Padmanabhan, T." style="width:75px"></td><td>7268</td>
</tr>
<tr>
<td>47</td><td>Harris, William E.</td><td><img src="/assets/astronomers/Harris.jpg " alt="Harris, William E." style="width:75px"></td><td>7226</td>
</tr>
<tr>
<td>48</td><td>Sanders, D. B.</td><td><img src="/assets/astronomers/Sanders.jpg " alt="Sanders, D. B." style="width:75px"></td><td>7215</td>
</tr>
<tr>
<td>49</td><td>Shakura, N. I.</td><td><img src="/assets/astronomers/Shakura.jpg " alt="Shakura, N. I." style="width:75px"></td><td>7162</td>
</tr>
<tr>
<td>50</td><td>Salpeter, Edwin E.</td><td><img src="/assets/astronomers/Salpeter.jpg " alt="Salpeter, Edwin E." style="width:75px"></td><td>7109</td>
</tr>
<tr>
<td>51</td><td>Landolt, Arlo U.</td><td><img src="/assets/astronomers/Landolt.jpg " alt="Landolt, Arlo U." style="width:75px"></td><td>6841</td>
</tr>
<tr>
<td>52</td><td>Madau, Piero</td><td><img src="/assets/astronomers/Madau.jpg " alt="Madau, Piero" style="width:75px"></td><td>6779</td>
</tr>
<tr>
<td>53</td><td>Misner, Charles W.</td><td><img src="/assets/astronomers/Misner.jpg " alt="Misner, Charles W." style="width:75px"></td><td>6768</td>
</tr>
<tr>
<td>54</td><td>Bruzual, G.</td><td><img src="/assets/astronomers/Bruzual.jpg " alt="Bruzual, G." style="width:75px"></td><td>6712</td>
</tr>
<tr>
<td>55</td><td>Burstein, D.</td><td><img src="/assets/astronomers/Burstein.jpg " alt="Burstein, D." style="width:75px"></td><td>6686</td>
</tr>
<tr>
<td>56</td><td>Bennett, C. L.</td><td><img src="/assets/astronomers/Bennett.jpg " alt="Bennett, C. L." style="width:75px"></td><td>6630</td>
</tr>
<tr>
<td>57</td><td>Cardelli, Jason A.</td><td><img src="/assets/astronomers/Cardelli.jpg " alt="Cardelli, Jason A." style="width:75px"></td><td>6581</td>
</tr>
<tr>
<td>58</td><td>Kroupa, Pavel</td><td><img src="/assets/astronomers/Kroupa.jpg " alt="Kroupa, Pavel" style="width:75px"></td><td>6430</td>
</tr>
<tr>
<td>59</td><td>York, Donald G.</td><td><img src="/assets/astronomers/York.jpg " alt="York, Donald G." style="width:75px"></td><td>6357</td>
</tr>
<tr>
<td>60</td><td>Eisenstein, Daniel J.</td><td><img src="/assets/astronomers/Eisenstein.jpg " alt="Eisenstein, Daniel J." style="width:75px"></td><td>6080</td>
</tr>
<tr>
<td>61</td><td>Moore, Ben</td><td><img src="/assets/astronomers/Moore.jpg " alt="Moore, Ben" style="width:75px"></td><td>6048</td>
</tr>
<tr>
<td>62</td><td>Copeland, Edmund J.</td><td><img src="/assets/astronomers/Copeland.jpg " alt="Copeland, Edmund J." style="width:75px"></td><td>5956</td>
</tr>
<tr>
<td>63</td><td>Seljak, Uros</td><td><img src="/assets/astronomers/Seljak.jpg " alt="Seljak, Uros" style="width:75px"></td><td>5925</td>
</tr>
<tr>
<td>64</td><td>Bak, Per</td><td><img src="/assets/astronomers/Bak.jpg " alt="Bak, Per" style="width:75px"></td><td>5675</td>
</tr>
<tr>
<td>65</td><td>Balbus, Steven A.</td><td><img src="/assets/astronomers/Balbus.jpg " alt="Balbus, Steven A." style="width:75px"></td><td>5596</td>
</tr>
<tr>
<td>66</td><td>White, S. D. M.</td><td><img src="/assets/astronomers/White.jpg " alt="White, S. D. M." style="width:75px"></td><td>5419</td>
</tr>
<tr>
<td>67</td><td>Albrecht, Andreas</td><td><img src="/assets/astronomers/Albrecht.jpg " alt="Albrecht, Andreas" style="width:75px"></td><td>5399</td>
</tr>
<tr>
<td>68</td><td>Freedman, Wendy L.</td><td><img src="/assets/astronomers/Freedman.jpg " alt="Freedman, Wendy L." style="width:75px"></td><td>5351</td>
</tr>
<tr>
<td>69</td><td>Herzberg, Gerhard</td><td><img src="/assets/astronomers/Herzberg.jpg " alt="Herzberg, Gerhard" style="width:75px"></td><td>5288</td>
</tr>
<tr>
<td>70</td><td>Abazajian, Kevork N.</td><td><img src="/assets/astronomers/Abazajian.jpg " alt="Abazajian, Kevork N." style="width:75px"></td><td>5261</td>
</tr>
<tr>
<td>71</td><td>Chabrier, Gilles</td><td><img src="/assets/astronomers/Chabrier.jpg " alt="Chabrier, Gilles" style="width:75px"></td><td>5236</td>
</tr>
<tr>
<td>72</td><td>Grevesse, N.</td><td><img src="/assets/astronomers/Grevesse.jpg " alt="Grevesse, N." style="width:75px"></td><td>5235</td>
</tr>
<tr>
<td>73</td><td>King, Ivan R.</td><td><img src="/assets/astronomers/King.jpg " alt="King, Ivan R." style="width:75px"></td><td>5124</td>
</tr>
<tr>
<td>74</td><td>Antonucci, R.</td><td><img src="/assets/astronomers/Antonucci.jpg " alt="Antonucci, R." style="width:75px"></td><td>5091</td>
</tr>
<tr>
<td>75</td><td>Mathis, J. S.</td><td><img src="/assets/astronomers/Mathis.jpg " alt="Mathis, J. S." style="width:75px"></td><td>4986</td>
</tr>
<tr>
<td>76</td><td>Fukuda, Y.</td><td><img src="/assets/astronomers/Fukuda.jpg " alt="Fukuda, Y." style="width:75px"></td><td>4968</td>
</tr>
<tr>
<td>77</td><td>Ferrarese, Laura</td><td><img src="/assets/astronomers/Ferrarese.jpg " alt="Ferrarese, Laura" style="width:75px"></td><td>4954</td>
</tr>
<tr>
<td>78</td><td>Shapiro, Stuart L.</td><td><img src="/assets/astronomers/Shapiro.jpg " alt="Shapiro, Stuart L." style="width:75px"></td><td>4888</td>
</tr>
<tr>
<td>79</td><td>Jackson, John David</td><td><img src="/assets/astronomers/Jackson.jpg " alt="Jackson, John David" style="width:75px"></td><td>4848</td>
</tr>
<tr>
<td>80</td><td>Leitherer, Claus</td><td><img src="/assets/astronomers/Leitherer.jpg " alt="Leitherer, Claus" style="width:75px"></td><td>4819</td>
</tr>
<tr>
<td>81</td><td>Perryman, M. A. C.</td><td><img src="/assets/astronomers/Perryman.jpg " alt="Perryman, M. A. C." style="width:75px"></td><td>4804</td>
</tr>
<tr>
<td>82</td><td>Shu, F. H.</td><td><img src="/assets/astronomers/Shu.jpg " alt="Shu, F. H." style="width:75px"></td><td>4749</td>
</tr>
<tr>
<td>83</td><td>Pringle, J. E.</td><td><img src="/assets/astronomers/Pringle.jpg " alt="Pringle, J. E." style="width:75px"></td><td>4622</td>
</tr>
<tr>
<td>84</td><td>Sze, S. M.</td><td><img src="/assets/astronomers/Sze.jpg " alt="Sze, S. M." style="width:75px"></td><td>4594</td>
</tr>
<tr>
<td>85</td><td>Bertin, E.</td><td><img src="/assets/astronomers/Bertin.jpg " alt="Bertin, E." style="width:75px"></td><td>4505</td>
</tr>
<tr>
<td>86</td><td>Savage, B. D.</td><td><img src="/assets/astronomers/Savage.jpg " alt="Savage, B. D." style="width:75px"></td><td>4372</td>
</tr>
<tr>
<td>87</td><td>Gebhardt, Karl</td><td><img src="/assets/astronomers/Gebhardt.jpg " alt="Gebhardt, Karl" style="width:75px"></td><td>4372</td>
</tr>
<tr>
<td>88</td><td>Caldwell, R. R.</td><td><img src="/assets/astronomers/Caldwell.jpg " alt="Caldwell, R. R." style="width:75px"></td><td>4352</td>
</tr>
<tr>
<td>89</td><td>Bethe, H. A.</td><td><img src="/assets/astronomers/Bethe.jpg " alt="Bethe, H. A." style="width:75px"></td><td>4342</td>
</tr>
<tr>
<td>90</td><td>Mohapatra, Rabindra N.</td><td><img src="/assets/astronomers/Mohapatra.jpg " alt="Mohapatra, Rabindra N." style="width:75px"></td><td>4313</td>
</tr>
<tr>
<td>91</td><td>Raymond, J. C.</td><td><img src="/assets/astronomers/Raymond.jpg " alt="Raymond, J. C." style="width:75px"></td><td>4260</td>
</tr>
<tr>
<td>92</td><td>Novoselov, K. S.</td><td><img src="/assets/astronomers/Novoselov.jpg " alt="Novoselov, K. S." style="width:75px"></td><td>4248</td>
</tr>
<tr>
<td>93</td><td>Skrutskie, M. F.</td><td><img src="/assets/astronomers/Skrutskie.jpg " alt="Skrutskie, M. F." style="width:75px"></td><td>4218</td>
</tr>
<tr>
<td>94</td><td>Schechter, P.</td><td><img src="/assets/astronomers/Schechter.jpg " alt="Schechter, P." style="width:75px"></td><td>4136</td>
</tr>
<tr>
<td>95</td><td>Bohlin, R. C.</td><td><img src="/assets/astronomers/Bohlin.jpg " alt="Bohlin, R. C." style="width:75px"></td><td>4007</td>
</tr>
<tr>
<td>96</td><td>Tremaine, Scott</td><td><img src="/assets/astronomers/Tremaine.jpg " alt="Tremaine, Scott" style="width:75px"></td><td>3999</td>
</tr>
<tr>
<td>97</td><td>Abell, George O.</td><td><img src="/assets/astronomers/Abell.jpg " alt="Abell, George O." style="width:75px"></td><td>3975</td>
</tr>
<tr>
<td>98</td><td>Bardeen, J. M.</td><td><img src="/assets/astronomers/Bardeen.jpg " alt="Bardeen, J. M." style="width:75px"></td><td>3927</td>
</tr>
<tr>
<td>99</td><td>Dickey, J. M.</td><td><img src="/assets/astronomers/Dickey.jpg " alt="Dickey, J. M." style="width:75px"></td><td>3877</td>
</tr>
<tr>
<td>100</td><td>Mayor, Michel</td><td><img src="/assets/astronomers/Mayor.jpg " alt="Mayor, Michel" style="width:75px"></td><td>3847</td>
</tr>
</table>