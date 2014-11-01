---
layout: post
title: "Creating a GUI in Python"
description: "A tutorial and template to create a very basic GUI in Python."
tags: [Python, GUI, Traits, Visualisation, Interactive]
---

So you want to create a Graphical User Interface (GUI)? This tutorial/code might help getting you started. The first feeling I had when I decided to follow this path was *intimidation*. With much patience and Googling I managed to create something I think will save me time in the future. I've used this tool countless times already, whether it be to improve my intuition of complex functions, speed up work flows or render dynamic data. The entire Pythonic GUI template I am about to introduce can be found at its own Github repository.

<center>
<div markdown="0"><a href="https://github.com/bgriffen/PythonGUITemplate" class="btn">Python GUI Github Repository</a></div>
</center>

## Where do I start?

Back in December of 2012, I wanted a tool which got me up and running relatively quickly without too much extra “fluff”. After some searching I found [Traits/TraitsUI](https://github.com/enthought/traits). It seemed like a good option since it allowed me to integrate a bunch of other features I already used at the time (e.g. matplotlib for plotting and [Mayavi](http://mayavi.sourceforge.net/) for rendering) within one distribution. Enthought state it best.

> The TraitsUI package is a set of user interface tools designed to complement Traits. In the simplest case, it can automatically generate a user interface for editing a Traits-based object, with no additional coding on the part of the programmer-user. 

Conveniently, all of the tools required to get you started (and then some) come with the [Enthought distribution](https://www.enthought.com/). For more background on TraitsUI there is (as always) [a Youtube tutorial](https://www.youtube.com/watch?v=ohHoU4qvsNs). First we need to install our required tools.

## Requirements

#### Installing EPD

Download the [Enthought distribution tools](https://www.enthought.com/) appropriate for your system. If you have an academic email then you can get it for free. Be sure to get the **32-bit** (rh5) version as the 64-bit version does not contain Mayavi. If you're on OSX then just get the .dmg image for OSX.

{% highlight Bash %}
bash epd-7.3-2-rh5-x86.sh 
{% endhighlight %}

#### Getting the GUI template

If you want to cut to the chase you can [clone the template from the respository]("https://github.com/bgriffen/PythonGUITemplate") and run in your terminal window:

{% highlight Python %}
python main.py
{% endhighlight %}

This should open up the template window with an empty canvas. Explore the code structure though there will be more on this later.

## Googling "make a GUI using Traits in Python"
Much to my surprise, there are few examples of how to get started using TraitsUI. Sure you can build very small tool with a slide bar which changes one variable and re-plots it – but there is nothing which brings it all together: multiple tabs, plotting and 3D rendering. The [examples provided in the documentation](http://docs.enthought.com/traitsui/tutorials/traits_ui_scientific_app.html#the-gui-elements) are either too basic or too complex for my liking so I spent a few months trying to create something in between -- basically what I wished the Internet had 12 months ago. Back in December 2012, I was transitioning my core research language to Python which took only a short while as I came from MATLAB/FORTRAN/SQL-land. Eventually built [a few useful tools](https://github.com/bgriffen/cme) (well at least for me and my research group) with what I’m about to discuss.  Here is the most sophisticated example available in the documentation:
<br/>
<br/>
![Traits Example](/assets/pythongui/TraitsExample.png)
<br/>
<br/>
Good start right? Whilst it does bring together the main ideas, it didn’t immediately give me a blank slate. It uses a bunch of Pythonic features which at the time I was not familiar with. The [documentation](http://docs.enthought.com/traitsui/tutorials/traits_ui_scientific_app.html#the-gui-elements) also didn’t incrementally build up to it (some basic tools then insta-migraine). I found myself back at the drawing board.

## Introducing: "The Blank Slate"
To create the blank slate, I essentially had to take apart what was already available in the above example whilst being sure I didn’t remove any critical components of the application. This soon became tedious and I felt like I wasn’t learning anything new. There were also a number of features in the basic layout which were not included (e.g. 3D rendering). This was shortly remedied by finding out that Traits does [play nicely with Mayavi](http://docs.enthought.com/mayavi/mayavi/auto/example_mayavi_traits_ui.html). Though my problem now became that of integration: these features existed independent from one another and I couldn’t find anywhere to put them all together (interactivity, plotting, 3D rendering). Eventually, after much head scratching I got it all working. I then stripped it down with the help of a colleague ([@astrowizicist](https://twitter.com/astrowizicist)) to create the following. I now use it as my default template to create new scientific applications.
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx1-1024x599.png)
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx2-1024x598.png)

## Code Layout
Initially this was all constructed in one giant Python script but eventually it became unmanageable with all of the objects involved in my application so I broke it down into the following set of modules which can be [found in the source](https://github.com/bgriffen/PythonGUITemplate) ([install git](http://git-scm.com/downloads), a version control system and clone the repository to get started). If you haven't already [downloaded the code](https://github.com/bgriffen/PythonGUITemplate) I highly suggest you do so now so you are familiar with what I'm about to discuss.

{% highlight Python %}
main.py
{% endhighlight %}

This contains the master script which controls the application and integrates everything. Once you’ve installed the [Enthought Python Distribution tools](https://support.enthought.com/entries/23407541-Getting-Started-with-EPD-on-OS-X), you can just run this from the command line `python main.py` and it should launch.

{% highlight Python %}
Common.py
{% endhighlight %}

This contains all of the modules which are used globally across the application. You can import function specific modules deeper in the code to improve launch time if you like as well. `Common.py` also constructs the matplotlib figure where you will be plotting (left panel). You’ll also find it imports a whole bunch of other things such as:

{% highlight Python %}
from enthought.traits.ui.api import View,UItem, Item, Group, Heading, \
Label, HSplit, Handler, CheckListEditor, EnumEditor, TableEditor, \
ListEditor, Tabbed, VGroup, HGroup, RangeEditor, Spring, spring
{% endhighlight %}

All of these are just apart of the Traits API which you call to put in your GUI. You’ll see how these are used later. I’ve also added a bunch of other commonly used modules for scientific processing but you can remove these if you don’t need them.

{% highlight Python %}
firstcalc.py
secondcalc.py
{% endhighlight %}

These are effectively the tabs on the right hand panel and the place where you will add your buttons, sliders, tables. They both contain a clone of (roughly) the same thing:

{% highlight Python %}
class FirstCalc(HasTraits):
    view = View()
    def __init__(self, main, **kwargs):
        HasTraits.__init__(self)
        self.main = main
{% endhighlight %}

## Adding Some Basic Functionality
Say for instance you want to add a few of the typical buttons, directories etc. Traits has to offer. Your firstcalc.py simply becomes:

{% highlight Python %}
from Common import *
class FirstCalc(HasTraits):
    # Add Traits objects
    option = Bool(True)
    rangex = Range(1,10,5)
    yval = Float()
    listoptions = Enum(['Option 1', 'Option 2','Option 2'])
    stringopt = Str("Default string which can also be empty.")
    stringoptread = Str("Default string which can also be empty [read-only version].")
    masterpath = Directory(os.getcwd())
    button1 = Button("Button 1 Name")
    floatval = Float()
    changedcount = Int()
 
    #Construct the view
    view = View(
                Item(name='rangex',label='X-Value'),
                Item(name='yval',style='readonly',label='1/x',format_str='%.2e'),
                Item(name='listoptions',label='list of options'),
                Item(name='stringopt'),
                Item(name='stringoptread',style='readonly'),
                Item(name='masterpath',label='Directory'),
                Group(Item(name='button1',show_label=False)),
                Group(Item(name='option',label='Boolean Option')
                     ,enabled_when='floatval < 0.5',label='Enabled Area Upon Random Value < 0.5',show_border=True),
                Item(name='changedcount',style='readonly',label='Attempts'),
                Item(name='floatval',label='random generated')
                )
 
    def _rangex_changed(self):
        self.yval = 1./self.rangex
 
    def _button1_fired(self):
        self.changedcount += 1
        self.floatval = random.random()
 
    def _floatval_changed(self):
        self.stringopt = "Hey, you changed the integer value!"
 
    def __init__(self, main, **kwargs):
        HasTraits.__init__(self
{% endhighlight %}
So let’s see what we’ve added when we relaunch.
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx3-1024x599.png)
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx4Zoom.png)
<br/>
<br/>
Slide the X-value ruler we see the objects change accordingly.
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx5.png)
<br/>
<br/>
Hopefully you can match up each of the objects with what is in the code. It is hard to convey the interactivity here but I can show a snapshot. Let’s change the slider and hit the button a few times.
<br/>
<br/>
![Traits Example](/assets/pythongui/PythonGUIEx6.png)
<br/>
<br/>
As we can see from the code the output is simply 1/x which is updated in real-time. Also when I hit the button, a random number is generated. If that number is less than 0.5 then the enabled area becomes illuminated and we can access the objects within (i.e. the Boolean option).  This should give you a basic idea of how to add certain button. Now we want to plot something, say y = mx + c where x and c are dynamic. I added three objects needed:

{% highlight Python %}
plotbutton = Button("Plot Me!")
yintcept = Range(0.0,5.,10.)
gradient = Range(0.0,5.0,10.)
{% endhighlight %}

which is inserted into the View() object just like the other objects.

{% highlight Python %}
Group(Item(name='gradient',label='gradient'),
      Item(name='yintcept',label='y-intercept'),
      Item(name='plotbutton',show_label=False),show_border=True,label='Plotting Area')
{% endhighlight %}

We also need to add an action for when the plot button is fired.

{% highlight Python %}
def _plotbutton_fired(self):
    y = self.gradient * np.array(range(10)) + self.yintcept
    figure = self.main.display
    figure.clear()
    ax = figure.add_subplot(111)
    ax = self.main.display.axes[0]
    ax.plot(np.array(range(10)),y,color=self.main.markercolor,
            marker=self.main.markerstyle,
            markersize=self.main.markersize,
            markeredgewidth=0.0,
            linestyle='-')
    wx.CallAfter(self.main.display.canvas.draw)
{% endhighlight %}
![Traits Example](/assets/pythongui/PythonGUIEx7-1024x599.png)
<br/>
<br/>
Also note how the marker style and marker size is “brought in” or inherited from main.py via self.main.xxx. It is hard to convey here ([try it out!](https://github.com/bgriffen/PythonGUITemplate)), but that now allows you to change the gradient and y-intercept dynamically and it updates on the plot in real-time. It is a much more intuitive (and faster) way to examine complex functions where it is unclear by looking at it how it will change using different inputs.

## Adding Mayavi Functionality
To implement Mayavi functionality you simply add the following (or something similar which uses your underlying data) to a button function which runs when fired (as above). This will produce something like the next image in the left panel.  There is also some limited documentation here.

{% highlight Python %}
def _mayavibutton_fired(self):
    self.main.scene.mlab.clf(figure=self.main.scene.mayavi_scene)
    x = np.array(data['posX'])
    y = np.array(data['posY'])
    z = np.array(data['posZ'])
    vx = np.array(data['pecVX'])
    vy = np.array(data['pecVY'])
    vz = np.array(data['pecVZ'])
    self.main.scene.mlab.quiver3d(x,y,z,vx,vy,vz)
    self.main.scene.mlab.xlabel('x-pos')
    self.main.scene.mlab.ylabel('y-pos')
    self.main.scene.mlab.zlabel('z-pos')
    self.main.scene.mlab.colorbar(title='velocity [km/s]')
    self.main.scene.mlab.show()
    self.main.scene.mlab.outline()
{% endhighlight %}

That’s the end of my “tutorial/demo” section. If you want to learn how to use the various other layout and object options I suggest taking a look at one of the tools I developed.

## Example Project
The [Catepillar Project](http://caterpillar.scripts.mit.edu/www/) is one of my main research tasks at MIT which involves looking at several hundreds of simulations of a similar kind. Rather than building endless panels in Matplotlib and tediously plotting different marker types and colors to inspect the data I decided to automate it using the tools described. Thus was born [Caterpillar Made Easy](https://github.com/bgriffen/cme) which is one such tool which which allows you setup and run cosmological simulations on a large cluster, inspect the data dynamically in two and three dimensions. It is based on exactly the same template I have shown above (plus a few hundred hours of coding!). Here are a few screen shots of the sort of thing you can do in this environment. Perhaps you have a study you’re doing which might also benefit from such interactivity and automation. I haven't *officially* released it to the public but I might do so soon. This is just to give you an idea of the sort of things you can achieve.
<center>
<div markdown="0"><a href="https://github.com/bgriffen/cme" class="btn">CME Repository</a></div>
</center>
<br/>
![Traits Example](/assets/pythongui/inspectparams-1024x620.png)
<br/>
<br/>
![Traits Example](/assets/pythongui/contaminationheatmap-1024x616.png)
<br/>
<br/>
![Traits Example](/assets/pythongui/velocityhaloinspection-1024x619.png)
<br/>
<br/>
Head to the repository to see more screenshots. If there is a button, or particular feature you want, you can check through the source code to see how it is generated. Just check the `View()` section which is usually near the top. The rest of the code is just calculations etc. which is specific to the problems I am trying to solve. If you have questions about how to get started, please leave a comment below or if it is more technical in nature, drop me a line via email. Good luck and remember if you are new to this kind of thing: *be patient* -- good things will come.

