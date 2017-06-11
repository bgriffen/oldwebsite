---
layout: post
title: "End Hiatus"
share: true
tags:
- dataviz
---
I haven't posted much content over the past year as I've been quite preoccupied with other activities. It's time to chage that.

The number and quality of data visualization tools available on the web have increased markedly over the past few years. It is time to bring the presentation of my work into the new age. To this end I'm retroactively going to update my previous blog posts to be more easily read and used. 

Starting from today each of my blog posts will be a standalone Jupyter notebook. This hopefully will allow people to work through the same process I did in achieving a given result. Additionally, my posts which contained static figures output from matplotlib will now be rendered dynamically using tools such as [Plotly](https://plot.ly/python/). What does this mean in practice? Let's quickly take a look at a stock set of library imports.


```python
import plotly.plotly as py      # my new goto plotting library (https://plot.ly/)
import plotly.graph_objs as go
import pandas as pd             # for handling data
import math                     # for doing math
```

Time to digest some data.


```python
# import some data about countries

data = pd.read_csv("https://raw.githubusercontent.com/plotly/datasets/master/gapminderDataFiveYear.csv")
df_2007 = data[data['year']==2007]
df_2007 = df_2007.sort_values(['continent', 'country'])
slope = 2.666051223553066e-05
hover_text = []
bubble_size = []

# construct our hover text for the widget

for index, row in df_2007.iterrows():
    hover_text.append(('Country: {country}<br>'+
                      'Life Expectancy: {lifeExp}<br>'+
                      'GDP per capita: {gdp}<br>'+
                      'Population: {pop}<br>'+
                      'Year: {year}').format(country=row['country'],
                                            lifeExp=row['lifeExp'],
                                            gdp=row['gdpPercap'],
                                            pop=row['pop'],
                                            year=row['year']))
    bubble_size.append(math.sqrt(row['pop']*slope))

# set our text size and bubble size

df_2007['text'] = hover_text
df_2007['size'] = bubble_size

# create our first 'trace' which is just a container for data
# color by continent, Africa first

trace0 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Africa'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Africa'],
    mode='markers',
    name='Africa',
    text=df_2007['text'][df_2007['continent'] == 'Africa'],
    marker=dict(
        symbol='circle',
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Africa'],
        line=dict(
            width=2
        ),
    )
)

# Americas second

trace1 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Americas'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Americas'],
    mode='markers',
    name='Americas',
    text=df_2007['text'][df_2007['continent'] == 'Americas'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Americas'],
        line=dict(
            width=2
        ),
    )
)

# Asia third

trace2 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Asia'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Asia'],
    mode='markers',
    name='Asia',
    text=df_2007['text'][df_2007['continent'] == 'Asia'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Asia'],
        line=dict(
            width=2
        ),
    )
)

# Europe fourth

trace3 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Europe'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Europe'],
    mode='markers',
    name='Europe',
    text=df_2007['text'][df_2007['continent'] == 'Europe'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Europe'],
        line=dict(
            width=2
        ),
    )
)

# Oceania fifth

trace4 = go.Scatter(
    x=df_2007['gdpPercap'][df_2007['continent'] == 'Oceania'],
    y=df_2007['lifeExp'][df_2007['continent'] == 'Oceania'],
    mode='markers',
    name='Oceania',
    text=df_2007['text'][df_2007['continent'] == 'Oceania'],
    marker=dict(
        sizemode='diameter',
        sizeref=0.85,
        size=df_2007['size'][df_2007['continent'] == 'Oceania'],
        line=dict(
            width=2
        ),
    )
)

# put all of these together in a list

data = [trace0, trace1, trace2, trace3, trace4]

# construct the layout - ease of understanding being the priority

layout = go.Layout(
    title='Life Expectancy v. Per Capita GDP, 2007',
    xaxis=dict(
        title='GDP per capita (2000 dollars)',
        gridcolor='rgb(255, 255, 255)',
        range=[2.003297660701705, 5.191505530708712],
        type='log',
        zerolinewidth=1,
        ticklen=5,
        gridwidth=2,
    ),
    yaxis=dict(
        title='Life Expectancy (years)',
        gridcolor='rgb(255, 255, 255)',
        range=[36.12621671352166, 91.72921793264332],
        zerolinewidth=1,
        ticklen=5,
        gridwidth=2,
    ),
    paper_bgcolor='rgb(243, 243, 243)',
    plot_bgcolor='rgb(243, 243, 243)',
)

# render using plotly

fig = go.Figure(data=data, layout=layout)
py.iplot(fig, filename='life-expectancy-per-GDP-2007')
```




<iframe id="igraph" scrolling="no" style="border:none;" seamless="seamless" src="https://plot.ly/~bgriffen/371.embed" height="525px" width="100%"></iframe>



The embedded interactivity adds so much more value to a dataset. [Github rendering Jupyter notebooks](https://help.github.com/articles/working-with-jupyter-notebook-files-on-github/) means presenting data through my blog is a breeze. Speaking of data, I will also make the data available for all my musings so that you can play around with the exact notebook I used seamlessly.

This is all part of my goal to make projects more open source and reproducible. It also allows others to build on whatever I'm working on for their own projects which is how I got started all those years ago.

Anyway, I'm looking forward to posting some of my new musings soon. Stay tuned!


```python

```
