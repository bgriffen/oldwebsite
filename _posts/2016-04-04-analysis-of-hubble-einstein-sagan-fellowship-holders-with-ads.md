---
layout: post
title: "Hubble, Einstein and Sagan Fellows With ADS"
tags:
  - python
  - astronomy
  - ads
share: true
---


Now that the job hunt is largely over, I thought it might be interesting to briefly examine the publication profiles of previous and current holders of fellowships which are the most in demand. The core fellowships I focus on are the NASA-funded Einstein, Hubble and Sagan fellowships which often get hundreds of applicants each year. I also include the last round (2016) fellowship holders in this analysis.

# Introduction 

For this study I use only the [ADS python](https://github.com/andycasey/ads) explorer and some other mainstream Python tools (primarily [Pandas](http://pandas.pydata.org/)). The only part of this analysis which took time was compiling the names and years for all fellowship holders. I eventually found the previous receipients on various NASA related webpages and got the remainder on this year's [Rumour Mill page](http://www.astrobetter.com/wiki/Rumor+Mill). 

The primary data set is a list of the following quantities: fellowship type, name, gender, year received, host institution and current institution. In some cases there was no data (only for institutions). Feel free to download the [csv file](/assets/fellowships/data/processed.csv) or the [dictionary file](/assets/fellowships/data/processed_dict). You can also get the full [Google Doc with the postprocessed analysis](https://docs.google.com/spreadsheets/d/1ByyiRw91dAFzmwZql2kfKbFuv_glNgk1VP76J92mSnM/edit?usp=sharing) but please read on first!  Though, please let me know if your publications are incorrect in the Google Doc -- I don't want to misrepresent people. Also keep in mind I am only using published, refereed, first author papers.

This is an example of the type of data I started with:

type     | year | sex | person |  phd_institute | host_institute
 --      | --   | -- | -- | -- | --
Hubble   | 2015 | f |  Katherine Alatalo |   University of California Berkeley    | Carnegie Observatories/Pasadena
Hubble   | 2015 | m |  Peter Behroozi |  Stanford University   | University of California Berkeley
Hubble   | 2015 | m |  Rongmon Bordoloi |    ETH Zurich  | Massachusetts Institute of Technology
Hubble   | 2015 | f |  Elodie Choquet |  University of Paris Diderot  | Jet Propulsion Laboratory
Hubble   | 2015 | f |  Lauren Ilsedore | Cleeves University of Michigan  |  Smithsonian Astrophysical Observatory
Einstein | 2015 | f |  Anna Barnacka  | - |     Harvard University 
Einstein | 2015 | m | Simeon Bird    | - | Johns Hopkins University 
Sagan    | 2015 | f |  Courtney Dressing  | California Institute of Technology |  -
Sagan    | 2015 | m |  Daniel Foreman-Mackey  | University of Washington |  -

# Goals & Caveats

A few natural questions one might ask with a list such as this are:

*  How many fellowship holders have there been over time?
*  What is the ratio of male-to-female fellowship holders over time?
*  How many papers on average do each type of fellowship holder typically have before being awarded?
*  Is there any difference between the number of citations women and men have at the time of being awarded?
*  How many citations do they have before, during and after the award?
*  Do men or women have more co-authors?
*  How many co-authors do each fellow type have?
*  How many citations do they have when awarded compared to later in their careers?

I must say up front that this analysis is not without its drawbacks. 

* I had to manually create the sex category based on first names. I actually wrote a gender classifier but I found that I could just do the laborious activity whilst watching a film and achieve a much higher accuracy than my classifier could, even if it did do it in seconds.
* When querying the ADS database, I used the last name and the first initial. Thankfully, most of the names a quite unique but if there are duplicate names in the ADS database, then their paper count and citation count will be inflated. In most plots, it is clear who is affected by this and in most cases, I'm simply interested in broad trends.
* I combined the Einstein, Chandra and Fermi fellows into just "Einstein fellows" (as per [this page](http://cxc.harvard.edu/fellows/fellowslist.html)).

# Method

First I queried the ADS database for the publication profiles for each person. This involved constructing a simple dictionary with the relevant quantities I wanted to calculate. With this pruned version of the code:


```python
for paper in papers:
    # find all papers which cite this paper
    cites_to_paper = paper.citation
    citation_count = paper.citation_count
    paper_year = paper.year
    paper_references = paper.reference
    paper_authors = paper.author
    if citation_count != 0:
        for cite_to_paper in cites_to_paper:
            # collect the year
            year = int(cite_to_paper[:4])
            # number of papers which cite them before award   
            if year < year_of_fellowship:
                num_cites_before_award += 1
            # number of papers which cite them during award (within 3 year time period)
            if year < year_of_fellowship+3 and year > year_of_fellowship:
                num_cites_during_award += 1
            # number of papers which cite them after award  
            if year > year_of_fellowship+3:
                num_cites_after_award += 1    
    # get number of coauthors
    if paper_authors: 
        num_coauthors += len(paper_authors)-1
    # number of papers which cite them before award   
    if int(paper_year) < year_of_fellowship:
        num_papers_before_award += 1
    # number of papers which cite them during award (within 3 year time period)
    if int(paper_year) < year_of_fellowship+3 and int(paper_year) > year_of_fellowship:
        num_papers_during_award += 1
    # number of papers which cite them after award  
    if int(paper_year) > year_of_fellowship+3:
        num_papers_after_award += 1    
````

I obtained the following new set of variables (the names of which are fairly self-explanitory:

| variable |
| :--: |
| `num_papers` |
| `num_papers_before_award` |
| `num_papers_during_award` |
| `num_papers_after_award` |
| `num_cites` |
| `num_cites_before_award` |
| `num_cites_during_award` |
| `num_cites_after_award` |
| `num_coauthors` |

I have added these quantites to a Google Document which you can explore.

[<img src="/assets/fellowships/table.png">](https://docs.google.com/spreadsheets/d/1ByyiRw91dAFzmwZql2kfKbFuv_glNgk1VP76J92mSnM/edit?usp=sharing)

# Results

With this curated dataset, I could now turn to my original list of questions

## How many fellowship holders have there been over time?

![total number](/assets/fellowships/total_numbers.png "total number of fellowship holders")

As I expected, there are a great many more Hubble fellows, followed by Einstein and Sagan fellows.

##  What is the ratio of male-to-female fellowship holders over time?

![total number based on sex](/assets/fellowships/sex_number.png "total number based on sex")

The number of women compared to men shows an under 50-50 breakup across time. The late 2000s saw the highest female intake for Hubble fellows.

##  How many papers on average do each type of fellowship holder typically have before being awarded?

![Number of papers per type of fellow](/assets/fellowships/npapers_box.png "Number of papers per type of fellow")

I opted to show this in a boxplot format. The redline is the median and the edges of the box represent the 25th and 75th percentile. The ends represent the maximum and minimum. From this, we see that people awarded Hubble and Einstein fellowships typically publish (median difference ~ 2) more papers than Sagan fellows at the time of their award. My feeling is that this is perhaps more indicative of the exoplanet field? I'd be interested to hear your feedback on why there is such a difference. One thing to keep in mind is that the maximums and minimums of these box plots are likely just wrong as ADS didn't find a paper matching the name in the case of the minimums and that there were more than one match for a given name in the case of the maximums.

##  Is there any difference between the number of papers women and men have at the time of being awarded?

![number of papers by gender](/assets/fellowships/gender_box.png "number of papers by sex")

Again, the tail of this distribution is likely wrong due to the aforementioned reasons. Here I group all the data by sex and show the number of first author papers they had the year they were awarded the fellowship. It seems that women have slightly fewer publications than men at the time of their award (median difference ~ 1).

##  How many citations do they have before, during and after the award?

![before during and after](/assets/fellowships/cite_box.png "before during and after citations")

Typically, researchers have approximately ~73 citations at the time of their award, ~102 citations during their time as a fellow and ~10 citations each year thereafter.

##  Do men or women have more co-authors?

![number of coauthors by sex](/assets/fellowships/malevsfemale_numcoauthors.png "number of coauthors by sex")

It seems, women have more co-authors per paper (median difference ~ 2).

##  How many co-authors do each fellow type have?

![number of coauthors by fellowship](/assets/fellowships/fellowship_numcoauthors.png "number of coauthors by fellowship")

It seems Sagan fellows have by far the most number of co-authors followed by Hubble and Einstein fellows.

## How many citations do they have when awarded compared to later in their careers?

![future publications](/assets/fellowships/malevsfemale_cites.png "future publications")

There is a weak trend indicating that if you start out with many citations you tend to get many more per year in subsequent years. This makes sense as certain papers do gain momentum once sufficient circulation has been reached.

# Conclusions

It isn't the most in-depth analysis but there are only so many hours in the day. I've got a long list of other things one could investigate but not the time right now. Feel free to take my original dataset and see if there are any other metrics you might want to investigate.

