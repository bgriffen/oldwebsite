---
layout: static
title: Globular Clusters & Reionization
---

![47Tucanae][47tuc]
<sup>*47 Tucanae; 17k light years away from Earth, 120 light years across and visible to the naked eye.* Source: wiki.org</sup>

It is now well understood that the universe emerged from the so-called *dark ages* (\\(30 \leq z \leq 1100\\)) when light from the first stars and quasars ignited and radiated large quantities of ionizing photons into the inter-galactic medium (IGM). This radiation reionized the universe in two epochs: one for hydrogen (\\(7 \leq z \leq 15\\)), and one for helium (z ~ 3.5). Though the most likely sources of helium reionization are quasars, the sources of hydrogen reionization are far less well known. 

Possible sources of high redshift reionization include Population III stars, Population II stars, quasars and more recently, particle decay/annihilation. Recent work suggests that initial stellar masses of today's ancient globular clusters (GCs) could have been as much as 8-25 times higher when they first formed, reinforcing earlier conclusions that metal-poor GCs could have significantly contributed to the reionization of the IGM at high redshift. 

<center>![Timeline of the Universe][universetimeline]  
<sup>History of the Universe from the Big Bang to the present day. The universe emerged from the so-called *dark ages* when light from the first stars and quasars ignited and radiated large quantities of ionizing photons into the inter-galactic medium. Source: NASA/WMAP Science Team</sup></center>

Ricotti et al. (2000) were among the first to formally suggest that GCs could have supplied a large quantity of the ionizing radiation. Several simulation studies have estimated the GC contribution. These studies were limited by uncertain parameters (e.g. star formation efficiencies, escape fractions, photo-ionization rates) and lacked the resolution to resolve globular cluster formation sites, but they all concluded the flux from the first generation of globular clusters could have significantly contributed to the reionization of the Milky Way. 

Herein lies one of the questions at the heart of my (now completed) PhD research...

> To what extent did metal-poor globular clusters contribute to the reionization of the intergalactic medium by z = 7?

The common approach used in many studies of GC formation is to *force* a truncation redshift (after which no more GCs form), giving satellite numbers and distributions comparable to observations. Whilst this approach can reproduce some observables, it does not account for the spatial inhomogeneity of the ionization field whereby sources are suppressed at different places and at different times. Ciardi et al. (2003), Furlanetto et al. (2004), Barkana et al. (2004), and Iliev et al. (2006) all clearly demonstrated that the reionization process of was in fact extended in time and spatially inhomogenous resulting in vastly different reionization times for different areas of the Universe. If studies of GC formation neglect the inhomogeneity of reionization field, their models will result in inaccurate formation numbers and distributions. 

Over the years, many have studied the connection between the reionization epoch and satellite formation. Alvárez et al. (2009) combined a *N*-body simulation with three-dimensional reionization calculations (1 \\(h^{-1}\\) Gpc width) to determine the relationship between reionization history and local environment. They found that on average, halos with mass less than \\(10^{13} M\\) were reionized internally, whilst almost all halos with mass greater than \\(10^{14} M\\) were reionized from without (consistent with Ocvirk et al. 2011). Busha et al. (2010) combined the subhalo catalogs from the *Via Lactea II* simulation with a Gpc-scale *N*-body simulation and found that by varying the reionization time over the range expected for Milky Way mass halos it could change the number of satellite galaxies by roughly 2-3 orders of magnitude. 

The work of Alvárez et al. (2009) and Busha et al. (2010) were semi-analytic studies of the reionization epoch. Iliev et al. (2011) however, combined a cosmological simulation with radiative transfer in a model of the Local Group and nearby clusters. They found that for photon-poor models, the Local Group could have been reionized by itself (photon-rich models found that nearby clusters reionized the Local Group externally). 

<center> ![Patch reionization in Iliev et al. (2012)][iliev]  
<sup>Position-redshift and position-frequency slides from a 163 Mpc box. These slices illustrate the large-scale geometry of reionization and the significant local variations in reionization history as seen at the redshifted 21-cm line. It also shows a high degree of inhomogeneity with different regions of the Universe ionizing at different times. Source: Iliev et al. (2012).</sup>
</center>

Lunnan et al. (2012) combined three-dimensional maps of reionization (using the semi-analytic models of Furlanetto et al. 2004) with the initial density field of the *Aquarius* simulation (Springel et al. 2008). They found that the number of satellites depends sensitively on the reionization model, with a factor of 3-4 difference for a given host halo. 

Most recently and perhaps most intriguingly, Boylan-Kolchin et al. (2011) have found that dissipationless dark matter simulations predict that the majority of the most massive subhaloes of the Milky Way are too dense to host any bright satellites at all. The vast majority of these works primarily focus on the effect of patchy reionization on the satellite (dwarf) systems of the Milky Way. All of them however, exclude the contributions from primordial GCs. Whilst all of these studies show the consequences of a patchy reionization process on galaxy evolution, a model incorporating both this process and *all* of the relevant reionization sources is yet to be carried out.

In my first paper (Griffen et al. 2010) I used the cosmological *Aquarius* simulation to study the reionization of the Milky Way by metal-poor globular clusters. In light of the aforementioned caveats in the literature, I was motivated to investigate this more thoroughly and aimed to achieve the following:

> Through simple models of reionization could I recover the correct spatial and kinematic distributions of z = 0 globular clusters around the Milky Way? If so, what sort of contributions would metal-poor globular clusters make to the reionization budget of the IGM? Could I also generate a model to produce the bimodality observed in globular clusters in the Milky Way whereby merger events created the metal-rich population in a post-reionization era (i.e. z \\(\leq\\) 7).

I concluded that with a reasonable escape fraction and star formation rate, the primordial metal-poor globular clusters of the Milky Way could have ionized the Milky Way by as early as  z ~ 13. I found that the UV flux from high-z GCs had drastic consequences for not only the spatial and dynamical properties of present day (*z* = 0) GCs, but also for their overall ages as well. 

The main caveats of my work however, was i) I assumed that *all* photons emitted from a potential MPGC would contribute to the reionization the Milky Way (much of the UV flux would have escaped into the outer IGM), ii) I did not model the dynamical destruction of the GCs once they merged with the central halo and iii) I did not allow for delayed star formation to take place whereby a previously suppressed halo could reignite if the recombinations were high enough and/or it became neutral later. 

<center>![Radiative transfer used in Griffen et al. (2013)][nformed_griffen2013]  
<sup>The number of metal globular clusters which have formed over the history of the Milky Way as modelled by Griffen et al. (2013). Including a modest treatment of their ionizaing flux drastically reduces the total number by z = 8 (colored vs. hollow bars). Source: Griffen et al. (2013).</sup></center>

In the second follow up paper (Griffen et al. 2013) paper I extended the methods employed by Griffen et al. (2010), addressing all these major caveats. I did this by (i) modelling a spatially-dependent reionization process by calculating the propagation of ionization fronts *directly* with a ray tracing radiative transfer code (\\({C^{2}Ray}\\), Mellema06 et al.), (ii) measuring the effects of dynamical destruction on present day (z = 0) GC properties by combining the *Aquarius* data with dynamical models Baumgardt & Makino (2003) and (iii) allowing for delayed star formation by combining the spatial information of halos within the *Aquarius* merger trees with the state of the IGM as modelled by \\({C^{2}Ray}\\). 

> I aimed to quantitatively analyze the formation of primordial GCs in a Milky Way type environment and determine to what extent GCs contributed to the reionization of the IGM and how the their contributions affected present day properties of metal-poor GCs.

After much torment, I completed a suite of radiative runs which had sequences which looked like the following: 

<center>![Radiative transfer used in Griffen et al. (2013)][radiationfield_griffen2013]  
<sup>Time evolution of the ionization field. Each panel represents a spatial slice (x-y projection), 460 \\(h^{-1}\\) kpc thick, of the ionized and neutral gas density from simulations of Model 1 using photo-ionization efficiencies of \\(f_\gamma\\) = 500 at z = 14, 12, 9 and 7 respectively. The box in each panel has a comoving width of 6 \\(h^{-1}\\) Mpc. Source: Griffen et al. (2013).</sup></center>

If we now look at how this looks in terms of environment, one can find widely varying impacts depending the nature of the radiation eminating from these ancient systems:

<center>![Radiative transfer used in Griffen et al. (2013)][reioniz_contribution_griffen2013]  
<sup>The fraction of the volume (xv) and mass (xm) ionized at three different redshifts (z = 7, 10 and 13) plotted simultaneously against the six different photo-ionization efficiencies (\\(f_\gamma\\)) and the width of the box surrounding the host halo in comoving coordinates. For each of the models, at a given redshift, a box was drawn around the central host galaxy (to within the nearest cell) and the volume and mass fraction ionized was calculated. Clearly, if metal-poor globular clusters do form via the dark halo formation channel then their contributions to the ionization of their formation environments is substantial. Source: Griffen et al. (2013).</sup></center>

These works comprised the bulk of my PhD thesis and can be summarized in the following way:

* A proper spatial treatment of the ionization field leads to drastically different numbers and spatial distributions when compared to models where globular cluster formation is simply truncated at a given redshift. This is one of the first studies of its kind in terms of using a 3-D radiative transfer code with the goal of measuring GC contributions to reionization.  
* By varying the photo-ionization efficiency from \\(f\gamma\\) = 150 photons/baryon to \\(f\gamma\\) = 5000 photons/baryon it is clear that small differences in GC photo-ionization efficiencies can lead to quite large differences in both their formation number and present day spatial distributions.  
* Using the photon efficiency as a free parameter, it is evident that it is unlikely that globular clusters formed solely via the dark matter halo formation channel since even under the most conservative estimates of the photon efficiencies the radial distributions are too shallow.
4. Despite this deficiency in number, GC contributions to the reionization of the local (i.e. 23 \\(h^{-3}\\) Mpc\\(^3\\) centred on a Milky Way type galaxy) volume and mass by redshift 10 could have been as high as 98% and 90%, respectively. In the photon poorest model, their contribution dropped to 60% and 50% by volume and mass respectively. This means globular clusters were important contributors to the reionization process on local scales at high-redshift until more photon-rich sources dominate the photon budget at later times.
* The non-suppressed clusters in all models have a narrow average age range (mean = 13.34 Gyr, \\(\sigma\\) = 0.04 Gyr) consistent with current ages estimates of the Milky Way metal-poor globular clusters (Salaris & Weiss 2002).
* My simple dynamical destruction model conservatively estimates that at least ∼60% of all metal-poor globular clusters which formed at high redshift have since been destroyed via tidal interactions with the host galaxy.
* In an extension to the model, I utilised the Aquarius merger trees whereby sup- pressed globular cluster descendants can potentially become active sources provided they resided within a neutral region of the simulation volume and had no active an- cestors. Though this doubles the potential number of GC candidates, the resulting radial distributions are not consistent with those of Milky Way metal- poor globular clusters, unless an additional ionization source is added at later times, around z = 10.

Whilst I currently do not directly work on globular cluster formation at the moment as my time is primarily occupied with the [*Caterpillar Project*](http://www.caterpillarproject.org/), I am still reading the literature and am encouraged to see more complete models of globular cluster formation coming out complimenting the ever increasing quality of data we have of globular cluster populations in the nearby universe.

[47tuc]: /assets/globular/47tuc.jpg "47 Tucanae"
[reioniz_contribution_griffen2013]: /assets/globular/reionizationcontribution_griffen2013.png "Griffen et al. (2013)"  
[radiationfield_griffen2013]: /assets/globular/radiationfield_griffen2013.png "Griffen et al. (2013)"  
[nformed_griffen2013]: /assets/globular/nformed_griffen2013.png "Griffen et al. (2013)"  
[iliev]: /assets/globular/iliev_patchyreionization.png "Iliev et al. (2012)"  
[universetimeline]: /assets/globular/timeline.jpg "Timeline of the universe"

[gh]: https://github.com/bgriffen