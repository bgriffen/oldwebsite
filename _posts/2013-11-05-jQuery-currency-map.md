---
layout: post
title: "Basic Interactive Currency Map With jQuery + Python"
description: "A basic exploration of jQuery and Python."
tags: [jQuery, Python, map, interactive]
---

The good people over at [jQuery Vector Maps](http://jqvmap.com/) have a nice little package for making interactive maps. I wanted to go beyond the standard vector map of countries. All it requires is a connection to a currency conversion module and some “find and replace” code. The most time consuming aspect was finding the correct python module which connected to an online currency exchange. I have added the code to a GitHub repository so fee free to improve upon it ([core code](https://github.com/bgriffen/jcurrency/blob/master/code/constructmap.py)).  It is quite poorly coded as I have never actually dealt with json data before and so I had to invoke sed and subprocesses to do some tasks which can most definitely done more cleanly. I welcome constructive criticisms and suggestions.
<center>
<div markdown="0"><a href="https://github.com/bgriffen/jcurrency" class="btn">jQuery Map Repository</a></div>
</center>

{% highlight Python %}
import json
import sys
from translate import Translator
import subprocess
import pycountry as pyc
import json, urllib2
import converter

selectcurrency = "USD"

countryISO = [u'gw',u'gt', u'gr', u'gq', u'gy', u'gf', u'ge', u'gd', u'gb',  \ 
u'ga', u'gn', u'gm', u'gl', u'gh', u'tz', u'lc', u'la', u'tw', u'tt', u'tr', \
u'lk', u'tn', u'tl', u'tm', u'lr', u'ls', u'th', u'tg', u'td', u'ly', u'do', \
u'dm', u'dj', u'dk', u'de', u'ye', u'dz', u'uy', u'qa', u'zm', u'ee', u'eg', \
u'za', u'ec', u'mk', u'et', u'zw', u'es', u'er', u'ru', u'rw', u'rs', u're', \
u'ro', u'bd', u'be', u'bf', u'bg', u'ba', u'bb', u'bn', u'bo', u'jp', u'bi', \
u'bj', u'bt', u'jm', u'bw', u'br', u'bs', u'lb', u'by', u'bz', u'om', u'ua', \
u'jo', u'ci', u'ch', u'co', u'cn',u'cm', u'cl', u'al', u'ca', u'cg', u'cf',  \
u'cd', u'cz', u'cy', u'cr', u'cv', u'cu', u'lv', u'pt', u'py', u'lt', u'pa', \
u'pf', u'pg', u'pe', u'tj', u'ph', u'pl', u'hr', u'ht', u'hu', u'hn', u'vn', \
u'pk', u'md', u'mg', u'ma', u'uz', u'mm', u'ml', u'mn', u'us', u'mu', u'mt', \
u'mw', u'mv', u'mr', u'ug', u'my', u'mx', u'mz', u'ae', u've', u'ag', u'af', \
u'iq', u'is', u'ir', u'am', u'it', u'ao', u'sv', u'ar', u'au', u'vu', u'in', \
u'az', u'ie', u'id', u'ni', u'nl', u'no', u'il', u'na', u'nc', u'ne', u'ng', \
u'nz', u'np', u'kw', u'fr', u'kz', u'fi', u'fj', u'fk', u'sz', u'sy', u'kg', \
u'ke', u'sr', u'kh', u'kn', u'km', u'st', u'sk', u'kr', u'si', u'kp', u'so', \
u'sn', u'sl', u'sc', u'sb', u'sa', u'at', u'se', u'sd']

countries = [u'Guinea-Bissau', u'Guatemala', u'Greece', u'Equatorial Guinea', \
u'Guyana', u'French Guiana', u'Georgia', u'Grenada', u'United Kingdom', u'Gabon', \
u'Guinea', u'Gambia', u'Greenland', u'Ghana', u'Tanzania', u'Saint Lucia', \
u"Lao People's Democratic Republic", u'Taiwan', u'Trinidad and Tobago', u'Turkey', \
u'Sri Lanka', u'Tunisia', u'Timor-Leste', u'Turkmenistan', u'Liberia', u'Lesotho', \
u'Thailand', u'Togo', u'Chad', u'Libya', u'Dominican Republic', u'Dominica', \
u'Djibouti', u'Denmark', u'Germany', u'Yemen', u'Algeria', u'Uruguay', u'Qatar', \
u'Zambia', u'Estonia', u'Egypt', u'South Africa', u'Ecuador', u'Macedonia', u'Ethiopia', \
u'Zimbabwe', u'Spain', u'Eritrea', u'Russian Federation', u'Rwanda', u'Serbia', \
u'Reunion', u'Romania', u'Bangladesh', u'Belgium', u'Burkina Faso', u'Bulgaria', \
u'Bosnia and Herzegovina', u'Barbados', u'Brunei Darussalam', u'Bolivia', u'Japan', \
u'Burundi', u'Benin', u'Bhutan', u'Jamaica', u'Botswana', u'Brazil', u'Bahamas', \
u'Lebanon', u'Belarus', u'Belize', u'Oman', u'Ukraine', u'Jordan', u"Cote d'Ivoire", \
u'Switzerland', u'Colombia', u'China', u'Cameroon', u'Chile', u'Albania', u'Canada', \
u'Congo', u'Central African Republic', u'Congo', u'Czech Republic', u'Cyprus', \
u'Costa Rica', u'Cape Verde', u'Cuba', u'Latvia', u'Portugal', u'Paraguay', \
u'Lithuania', u'Panama', u'French Polynesia', u'Papua New Guinea', u'Peru', \
u'Tajikistan', u'Philippines', u'Poland', u'Croatia', u'Haiti', u'Hungary', \
u'Honduras', u'Vietnam', u'Pakistan', u'Moldova', u'Madagascar', u'Morocco', \
u'Uzbekistan', u'Myanmar', u'Mali', u'Mongolia', u'United States of America', \
u'Mauritius', u'Malta', u'Malawi', u'Maldives', u'Mauritania', u'Uganda', \
u'Malaysia', u'Mexico', u'Mozambique', u'United Arab Emirates', u'Venezuela', \
u'Antigua and Barbuda', u'Afghanistan', u'Iraq', u'Iceland', u'Iran', u'Armenia', \
u'Italy', u'Angola', u'El Salvador', u'Argentina', u'Australia', u'Vanuatu', \
u'India', u'Azerbaijan', u'Ireland', u'Indonesia', u'Nicaragua', u'Netherlands', \
u'Norway', u'Israel', u'Namibia', u'New Caledonia', u'Niger', u'Nigeria', u'New Zealand', \
u'Nepal', u'Kuwait', u'France', u'Kazakhstan', u'Finland', u'Fiji', \u'Falkland Islands', \
u'Swaziland', u'Syrian Arab Republic', u'Kyrgyz Republic', u'Kenya', u'Suriname', \
u'Cambodia', u'Saint Kitts and Nevis', u'Comoros', u'Sao Tome and Principe', u'Slovakia', \
u'South Korea', u'Slovenia', u'North Korea', u'Somalia', u'Senegal', u'Sierra Leone', \
u'Seychelles', u'Solomon Islands', u'Saudi Arabia', u'Austria', u'Sweden', u'Sudan']

currencies = ['XOF','CTQ','EUR','XAF','GYD','EUR','GEL','XCD','GBP','XAF','GNF','GMD','DKK', \
'GHS','TZS','XCD','LAK','TWD','TTD','TRY','LKR','TND','USD','TMT','LRD','LSL','THB','THB','XAF', \
'XOF','DOP','XCD','DJF','DKK','EUR','YER','DZD','UYU','QAR','ZMW','EUR','EGP','ZAR','USD','MKD', \
'ETB','ZWL','RUB','ERN','RUB','RWF','RSD','EUR','RON','BDT','EUR','XOF','BGN','BAM','BBD','BND', \
'BOB','JPY','BIF','XOF','BTN','JMD','BWP','BRL','BSD','LBP','BYR','BZR','OMR','UAH','JOD','XOF', \
'CHF','COP','CNY','XAF','CLP','ALL','CAD','CDF','XAF','CDF','CZK','EUR','CRC','CVE','CUP','LVL', \
'EUR','PYG','LTL','PAB','XPF','PGK','PEN','TJS','PHP','PLN','HRK','HTG','HUF','HNL','VND','PKR', \
'MDL','MGA','MAD','UZS','MMK','XOF','MNT','USD','MUR','EUR','MWK','MVR','MRO','UGX','MYR','MXN', \
'MZN','AED','VEF','XCD','AFN','UQD','ISK','IRR','AMD','EUR','AOA','SVC','ARS','AUD','VUV','INR', \
'AZN','EUR','IDR','NIO','EUR','EUR','ILS','NAD','XPF','XOF','NGN','NZD','NPR','KWD','EUR','KZT', \
'EUR','FJD','FKP','SZL','SYP','KGS','KES','SRD','KHR','XCD','KMF','STD','EUR','KRW','EUR','KPW','\
SOS','XOF','SLL','SCR','SBD','SAR','EUR','SEK','SDG']


filename = "jquery.vmap.sampledata.js"
f = open(filename,'w')
f.write("var sample_data = {")

cpcmd = "cp template.js oldfile.js"
subprocess.call(';'.join([cpcmd]), shell=True)
for i in xrange(0,len(countryISO)):
    countrydecode = countryISO[i].encode('UTF-8').upper()
    currencyname = currencies[i]
    
    countryencoded = pyc.countries.get(alpha2=countrydecode)
    decodedcountry = countryencoded.name.encode('utf-8')
    shortcountryname = decodedcountry.split(',')

    for country in countries:
        if shortcountryname[0] == country.encode('utf-8'):
            if currencyname != selectcurrency:
                c = converter.convert(from_curr=selectcurrency,to_curr=currencyname)
                newname = shortcountryname[0] + ": " + c + " " + currencyname
                print "--------------------"
                print str(countryISO[i]) + ": " + shortcountryname[0]
                print "Exchange rate: " + c
                if "Lao People's Democratic Republic" ==  shortcountryname[0]:
                    shortcountryname[0] = "Laos"

            elif currencyname == selectcurrency:
                newname = shortcountryname[0] + ": 1 " + selectcurrency
            
            cp_files = "sed -e 's/" + shortcountryname[0] + "/" + newname + \
                            "/g' oldfile.js > newfile.js"
            mv_files = "mv newfile.js oldfile.js"
            subprocess.call(';'.join([cp_files,mvfiles]), shell=True)
            if i != len(countryISO):
                datastr = '"' + countryISO[i] + '":"' + c + '",'
            elif i == len(countryISO)-1:
                datastr = '"' + countryISO[i] + '":"' + c + '"'
            f.write(datastr)

f.write("}")
f.close()
mving = 'cp oldfile.js jquery.vmap.world.js'
subprocess.call(';'.join([mving]), shell=True)
print "conversion complete!"
{% endhighlight %}

## Interactive Map

<object width="800" height="500" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://download.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,40,0"><param name="src" value="http://bgriffen.scripts.mit.edu/www/data/currencymap/world.html" /><embed width="800" height="500" type="application/x-shockwave-flash" src="http://bgriffen.scripts.mit.edu/www/data/currencymap/world.html" /></object>

This could further be expanded to make it able to take user input and scale the colors to be country GDP, population etc. Though I’m sure many maps of this kind already exist, I’ve found it a nice way to learn a new programming language.