---
layout: post
title: "Creating Digests Of The Astronomy Arxiv"
description: "Does the genre of an artist determine their lifespan?"
tags: [python, ADS, arxiv, astronomy]
---

I took another look at [ADS-python](https://github.com/andycasey/ads) (a python tool for ADS) developed by [Andy Casey](http://astrowizici.st/). I modified his example script to email myself a digest of all of the papers published by my institute in the past month. I set it up as an automated cron job (`10 0 1 * * python script.py`) to be run on the 1st of each month so I don't have to run the script anymore to get the digest. You will need [FluentMail](https://github.com/alexandrevicenzi/fluentmail) for this to work. You will also need to allow Gmail to receive login requests from less familiar apps. If you want to use a different email client see [here](https://github.com/alexandrevicenzi/fluentmail#common-smtp-servers). I've setup a [GitHub repository](http://www.github.com/bgriffen/ads.git) for these codes and future codes relating to work done on ADS.

{% highlight Python %}
# Core libraries
from time import localtime
import sys,os
import subprocess as sub

# Module Libraries
import ads
from fluentmail import FluentMail

mail = FluentMail('smtp.gmail.com', 465, 'SSL')
email_address = 'brendan.f.griffen@gmail.com'
email_password = 'password'
output_directory = "./"
download_papers = False
my_affiliation = '"Kavli Institute For Astrophysics"'

if __name__ == "__main__":

    # Let's do it for *last* month
    current_time = localtime()

    # Last month
    year = current_time.tm_year - 1 if current_time.tm_mon == 1 else current_time.tm_year
    month = current_time.tm_mon - 1 if current_time.tm_mon > 1 else 12
    
    # Get all the articles
    articles1 = ads.search(
        affiliation=my_affiliation,
        filter="database:astronomy AND property:refereed",
        dates="{year}/{month}".format(year=year, month=month))

    articles = list(articles1)
    print("There were {0} articles found for {year}/{month} with MKI co-authors.".format(len(articles),year=year, month=month))

    # Sort articles
    sorted_articles = sorted(articles,
        key=lambda article: [(my_affiliation.strip('"').lower() in affiliation.lower()) for affiliation in article.aff].index(True))

    # Option to download these papers
    if download_papers:
        output_folder = output_directory + "{year}_{month}".format(year=year,month=month)
        cmd_make_this_months_folder = "mkdir -p " + output_folder
        sub.call([cmd_make_this_months_folder],shell=True)

        filename = output_folder + "/" + article.bibcode + ".pdf"
        if not os.path.isfile(filename):
            ads.retrieve_article(article, output_filename=output_folder + "/" + article.bibcode + ".pdf")

    # Construct email content
    email_content = "There were {num} articles published by astronomy researchers from the {my_affiliation} last month ({year}/{month})"\
                    .format(num=len(articles),my_affiliation=my_affiliation,year=year,month=month)

    for article in sorted_articles:
        email_content += article.author[0].split(",")[0] + " et al." +  article.title[0] + "" + article.abstract + "" + article.url + " "

    # Email yourself the digest
    mail.credentials(email_address, email_password)\
    .from_address(email_address).to(email_address)\
    .subject('Monthly Institute Arxiv Digest')\
    .body(email_content)\
    .send()

{% endhighlight %}

Here is an example of what an email might look like:

[![Digest](/assets/adspython/monthly_arxiv_digest.png)](/assets/adspython/monthly_arxiv_digest.png)

I then also wrote my own script to give me a list of papers which were the most read yesterday.

{% highlight Python %}
# Core libraries
from time import localtime
import numpy as np

# Module Libraries
import ads
from fluentmail import FluentMail


mail = FluentMail('smtp.gmail.com', 465, 'SSL')
email_address = 'brendan.f.griffen@gmail.com'
email_password = 'password'

if __name__ == "__main__":
    current_time = localtime()
    year = current_time.tm_year
    month = current_time.tm_mon
    day = current_time.tm_mday-1 # yesterday
    
    # Get papers
    papers = list(ads.query(dates="{year}/{month}/{day}"\
            .format(year=year, month=month, day=day),filter="database:astronomy", rows=20))

    # Get number of reads
    num_reads_list = []
    for paper in papers:
        num_reads = paper.metrics['all_reads']['Total_number_of_downloads']
        num_reads_list.append(num_reads)
    
    idx_num_reads = np.argsort(np.array(num_reads_list))
    
    # Construct email content
    email_content = "Top viewed papers for {day}/{month}/{year}"\
                    .format(year=year, month=month, day=day)

    for i in idx_num_reads[:10]:
        email_content += papers[i].author[0].split(",")[0] + " et al. > reads: " + \
        papers[i].metrics['all_reads']['Total_number_of_reads'] + " " + \
        papers[i].title[0] + "" + papers[i].abstract + "" + papers[i].url + " "

    # Email content
    mail.credentials(email_address, email_password)\
    .from_address(email_address)\
    .to(email_address)\
    .subject("Yesterday's most popular papers!")\
    .body(email_content)\
    .send()
{% endhighlight %}

This is really only the beginning of the sort of things you can do with nice Python-esk access to the ADS database.
