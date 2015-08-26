---
layout: post
title: "Generating An Institute's Publication Profile"
description: "Does the genre of an artist determine their lifespan?"
tags: [python, ADS, arxiv, astronomy]
---

Today I wanted to get an idea of my home institute's publication profile based on the [staff list from its own website](http://space.mit.edu/people/all). I'm sure if you're in academia you would have the same for your own. My list includes members which belong to various categories: faculty, affiliated faculty, postdoctoral scholar, student and technical staff.

To build the profile, we need to make use of ADS metrics. For example we can search for a paper of interest on ADS labs:

[![Paper](/assets/adspython/example_paper.png)](/assets/adspython/example_paper.png)

Then we can click on `Analyze` in the top right to bring up a new panel of information:

[![Metric](/assets/adspython/example_metric.png)](/assets/adspython/example_metrics.png)

Using [ADS-python](https://github.com/andycasey/ads), I can access these metrics for an author with the simple line:

{% highlight Python %}
import ads
metrics = ads.metrics(author_name)

print metrics[0]

{u'all_reads': {u'Average_number_of_downloads': 73.0,
  u'Average_number_of_reads': 218.8,
  u'Median_number_of_downloads': 53.0,
  u'Median_number_of_reads': 173.5,
  u'Normalized_number_of_downloads': 1.8,
  u'Normalized_number_of_reads': 5.2,
  u'Total_number_of_downloads': 1460,
  u'Total_number_of_reads': 4375},
 u'all_stats': {u'Average_citations': 9.6,
  u'Average_refereed_citations': 5.9,
  u'H-index': 7,
  u'Median_citations': 5.5,
  u'Median_refereed_citations': 2.0,
  u'Normalized_citations': 0.2,
  u'Normalized_paper_count': 0.1,
  u'Normalized_refereed_citations': 0.1,
  u'Number_of_citing_papers': 160,
  u'Number_of_papers': 20,
  u'Refereed_citations': 118,
  u'Total_citations': 192,
  u'e-index': 10.2,
  u'g-index': 13,
  u'i10-index': 5,
  u'i100-index': 0,
  u'm-index': 3.5,
  u'read10_index': 1,
  u'roq_index': 41.0,
  u'self-citations': 12,
  u'tori_index': 0.0},
 u'citation_histogram': {u'2013': [23.0,
   19.0,
   23.0,
   19.0,
   0.0309913153179,
   0.0251989779794,
   0.0309913153179,
   0.0251989779794],
  u'2014': [169.0,
   99.0,
   154.0,
   97.0,
   0.207952689976,
   0.123712598875,
   0.190817259927,
   0.121428547883],
  u'type': u'citation_histogram'},
 u'metrics_series': {u'2013': [2.0,
   4.0,
   1.0,
   0.00125995174574,
   2.0,
   35.0,
   0.0,
   0.0],
  u'2014': [7.0, 13.0, 5.0, 0.00685960767226, 3.5, 41.0, 0.0, 0.0],
  u'type': u'metrics_series'},
 u'paper_histogram': {u'2013': [4.0, 4.0, 0.00504776053564, 0.00504776053564],
  u'2014': [16.0, 11.0, 0.0659490983056, 0.0125452264852],
  u'type': u'publication_histogram'},
 u'reads_histogram': {u'1996': [0.0, 0.0, 0.0, 0.0],
  u'1997': [0.0, 0.0, 0.0, 0.0],
  u'1998': [0.0, 0.0, 0.0, 0.0],
  u'1999': [0.0, 0.0, 0.0, 0.0],
  u'2000': [0.0, 0.0, 0.0, 0.0],
  u'2001': [0.0, 0.0, 0.0, 0.0],
  u'2002': [0.0, 0.0, 0.0, 0.0],
  u'2003': [0.0, 0.0, 0.0, 0.0],
  u'2004': [0.0, 0.0, 0.0, 0.0],
  u'2005': [0.0, 0.0, 0.0, 0.0],
  u'2006': [0.0, 0.0, 0.0, 0.0],
  u'2007': [0.0, 0.0, 0.0, 0.0],
  u'2008': [0.0, 0.0, 0.0, 0.0],
  u'2009': [0.0, 0.0, 0.0, 0.0],
  u'2010': [0.0, 0.0, 0.0, 0.0],
  u'2011': [0.0, 0.0, 0.0, 0.0],
  u'2012': [0.0, 0.0, 0.0, 0.0],
  u'2013': [951.0, 951.0, 1.14041166693, 1.14041166693],
  u'2014': [3424.0, 3037.0, 4.10556990785, 3.50217212234],
  u'type': u'reads_histogram'},
 u'refereed_reads': {u'Average_number_of_downloads': 87.4,
  u'Average_number_of_reads': 265.9,
  u'Median_number_of_downloads': 55.0,
  u'Median_number_of_reads': 179.0,
  u'Normalized_number_of_downloads': 1.5,
  u'Normalized_number_of_reads': 4.6,
  u'Total_number_of_downloads': 1311,
  u'Total_number_of_reads': 3988},
 u'refereed_stats': {u'Average_citations': 11.8,
  u'Average_refereed_citations': 7.7,
  u'H-index': 7,
  u'Median_citations': 7.0,
  u'Median_refereed_citations': 4.0,
  u'Normalized_citations': 0.2,
  u'Normalized_paper_count': 0.0,
  u'Normalized_refereed_citations': 0.1,
  u'Number_of_citing_papers': 150,
  u'Number_of_papers': 15,
  u'Refereed_citations': 116,
  u'Total_citations': 177,
  u'e-index': 10.2,
  u'g-index': 13,
  u'i10-index': 5,
  u'i100-index': 0,
  u'm-index': 3.5,
  u'read10_index': 1,
  u'roq_index': 40.0,
  u'self-citations': 10,
  u'tori_index': 0.0}}

{% endhighlight %}

Using this data, we can take a look at the distribution of citations, papers and number of people for each of the respective positions.

[![MKI Profile](/assets/adspython/mki_profile.png)](/assets/adspython/mki_profile.png)

3 conclusions can be made about MKI:

* There are more students than any other position.  
* Students publish the most number of papers.  
* Postdocs, faculty and students roughly equally share overall citation count.
