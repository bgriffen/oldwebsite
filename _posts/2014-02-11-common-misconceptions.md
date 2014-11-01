---
layout: post
title: "Scraping Wikipedia: Common Misconceptions"
description: "A very basic example of scraping Wikipedia using the Pattern suite."
tags: [scraping, misconceptions, Pattern]
---

I recently stumbled upon this [list of common misconceptions on Wikipedia](https://en.wikipedia.org/wiki/List_of_common_misconceptions) which contains a wide range of interesting tidbits. For instance, did you know Napoleon was taller than average for a Frenchman and that Vikings did not wear helmets with horns? There are a great number of little factoids and I wondered if I could not just scrape the first sentence to get the core facts of the article. I ended up writing a ~20 line Python script which uses the very useful Pattern suite to strip down the Wikipedia article into its basic content and output the first sentence. This work can also be found on Github. There are a few minor parsing errors but it turned out OK. Extremely basic, but fun.

<center>
<div markdown="0"><a href="https://github.com/bgriffen/wikipedia/tree/master/wikimisconceptions" class="btn">Github Repository</a></div>
</center>

{% highlight Python %}
import os, sys; sys.path.insert(0, os.path.join("..", ".."))
from pattern.web import Wikipedia
engine = Wikipedia(language="en")
article = engine.search("list of common misconceptions", cached=True, timeout=30)
f = open("wikimisconceptions.txt",'w')
for s in article.sections:
    f.write("\n")<br title="s.title.upper()<br" />    f.write(title.encode("utf8")+"\n")
    print ""
    print title
    facts = s.content.split("*")
    num = -1
    for fact in facts:
         num += 1
         if "See also" not in fact \
         and "Further information" not in fact \
         and "Main articles" not in fact and fact != "":
             line = str(num) + "." + fact.split(".")"."
             print line
             f.write(line.encode("utf8")+"\n")
f.close()
{% endhighlight %}

This results in the following output:

#### ANCIENT TO MODERN HISTORY
1. Vomiting was not a regular part of Roman dining customs.
2. It is true that life expectancy in the Middle Ages and earlier was low; however, one should not infer that people usually died around the age of 30.
3. There is no evidence that Vikings wore horns on their helmets.
4. King Canute did not command the tide to reverse in a fit of delusional arrogance.
5. There is no evidence that iron maidens were invented in the Middle Ages or even used for torture.
6. The plate armor of European soldiers did not stop soldiers from moving around or necessitate a crane to get them into a saddle.
7. Modern historians dispute the popular misconception that the chastity belt, a device designed to prevent women from having sexual intercourse, was invented in medieval times.
8. Medieval Europeans did not believe Earth was flat; in fact, from the time of the ancient Greek philosophers Plato and Aristotle on, belief in a spherical Earth remained almost universal among European intellectuals.
9. Columbus never reached any land that now forms part of the mainland United States of America; most of the landings Columbus made on his four voyages, including the initial October 12, 1492 landing (the anniversary of which forms the basis of Columbus Day), were in the Caribbean Islands.
10. There is a legend that Marco Polo imported pasta from China which originated with the Macaroni Journal, published by an association of food industries with the goal of promoting the use of pasta in the United States.
11. Contrary to the popular image of the Pilgrim Fathers, the early settlers of the Plymouth Colony did not wear all black, and their capotains (hats) were shorter and rounder than the widely depicted tall hat with a buckle on it.
12. The accused at the Salem witch trials were not burned at the stake, they either died in prison or were hanged.
13. Marie Antoinette did not say “let them eat cake” when she heard that the French peasantry were starving due to a shortage of bread.
14. George Washington did not have wooden teeth.
15. The signing of the United States Declaration of Independence did not occur on July 4, 1776.
16. Benjamin Franklin did not propose that the wild turkey be used as the symbol for the United States instead of the bald eagle.
17. There was never a bill to make German the official language of the United States that was defeated by one vote in the House of Representatives, nor has one been proposed at the state level.

#### MODERN HISTORY
1. Napoleon Bonaparte was not short; rather he was slightly taller than the average Frenchman of his time.
2. Cinco de Mayo is not Mexico’s Independence Day, but the celebration of the Mexican Army’s victory over the French in the Battle of Puebla on May 5, 1862.
3. The Great Chicago Fire of 1871 was not caused by Mrs.
4. The claim that Frederick Remington, on assignment to Cuba, telegraphed William Randolph Hearst that “.
5. The popular image of Santa Claus was not created by The Coca-Cola Company as an advertising gimmick; by the time Coca-Cola began using Santa Claus’s image in the 1930s, Santa Claus had already taken his modern form in popular culture, having already seen extensive use in other companies’ advertisements and other mass media.
6. Italian dictator Benito Mussolini did not “make the trains run on time”.
7. There was no widespread outbreak of panic across the United States in response to Orson Welles’ 1938 radio adaptation of H.
8. There is no evidence of Polish cavalry mounting a brave but futile charge against German tanks using lances and sabres during the German invasion of Poland in 1939.
9. During the occupation of Denmark by the Nazis during World War II, King Christian X of Denmark did not thwart Nazi attempts to identify Jews by wearing a yellow star himself.
10. Albert Einstein did not fail mathematics in school.
11. Actor Ronald Reagan was never seriously considered for the role of Rick Blaine in the 1942 film classic Casablanca, eventually played by Humphrey Bogart.
12. Eva Perón never uttered the quote “I will return and I will be millions”.
13. The Rolling Stones were not performing “Sympathy for the Devil” at the 1969 Altamont Free Concert when Meredith Hunter was stabbed to death by a member of the local Hells Angels chapter that was serving as security.

#### LEGISLATION AND CRIME
1. It is rarely necessary to wait 24 hours before filing a missing person’s report; in instances where there is evidence of violence or of an unusual absence, law enforcement agencies in the United States often stress the importance of beginning an investigation promptly.
2. Entrapment law in the United States does not require police officers to identify themselves as police in the case of a sting or other undercover work.

#### FOOD AND COOKING
1. Searing meat does not “seal in” moisture, and in fact may actually cause meat to lose moisture.
2. Some people believe that food items cooked with wine or liquor will be totally non-alcoholic, because alcohol’s low boiling point causes it to evaporate quickly when heated.
3. Monosodium glutamate (MSG) has a widespread reputation for triggering migraine headache exacerbations, but there are no consistent data to support this relationship.
4. Sushi does not mean “raw fish”, and not all sushi includes raw fish.
5. Microwave ovens do not cook food from the inside out.
6. Placing metal inside a microwave oven does not damage the oven’s electronics.
7. The functional principle of a microwave oven is not related to the resonance frequencies of water, and microwave ovens can therefore operate at many different frequencies.  
8. The Twinkie does not have an infinite shelf life; its listed shelf life is approximately 45 days (25 in its original formulation) and generally remains on a store shelf for only 7 to 10 days.  
9. Fortune cookies, despite being associated with Chinese cuisine in the United States, were in fact invented and brought to the U.

#### WORDS AND PHRASES
1. Non-standard, slang or colloquial terms used by English speakers are sometimes alleged not to be real words.
2. The word “fuck” did not originate in Christianized Anglo-Saxon England (7th century CE) as an acronym for “Fornication Under Consent of King”; nor did it originate as an acronym for “For Unlawful Carnal Knowledge”, either as a sign posted above adulterers in the stocks, or as a criminal charge against members of the British Armed Forces; nor did it originate during the 15th-century Battle of Agincourt as a corruption of “pluck yew” (an idiom falsely attributed to the English for drawing a longbow).
3. The word “crap” did not originate as a back-formation of British plumber Thomas Crapper’s surname, nor does his name originate from the word “crap”, although the surname may have helped popularize the word.
4. The expression “rule of thumb” did not originate from a law allowing a man to beat his wife with a stick no thicker than his thumb, and there is no evidence that such a law ever existed.
5. “Golf” did not originate as an acronym of “Gentlemen Only, Ladies Forbidden”.
6. The word “gringo” did not originate during the Mexican-American War (1846–1848), the Venezuelan War of Independence (1811–1823), the Mexican Revolution (1910–1920), or in the American Old West (c.
7. “420″ did not originate as the Los Angeles police or penal code for marijuana use.
8. The word “the” was never pronounced or spelled “ye” in Old or Middle English.
9. “Xmas” did not originate as a secular plan to “take the Christ out of Christmas”.
10. Although the expression “the enemy of my enemy is my friend” is often described as an Arabic proverb, there is no evidence of such an origin.

#### ASTRONOMY
1. It is commonly claimed that the Great Wall of China is the only human-made object visible from the Moon.
2. Black holes, contrary to their common image, have the same gravitational effects as any other equal mass in their place.
4. Meteorites are not necessarily hot when they reach the Earth.
5. When a meteor or spacecraft enters the atmosphere, the heat of entry is not (primarily) caused by friction, but by adiabatic compression of air in front of the object.
6. Egg balancing is possible on every day of the year, not just the vernal equinox, and there is no evidence of a relationship between astronomical phenomena and the ability to balance an egg.

#### VERTEBRATES
1. Older elephants that are near death do not leave their herd and instinctively direct themselves toward a specific location known as an elephants’ graveyard to die.
2. Bulls are not enraged by the color red, used in capes by professional matadors.
4. Lemmings do not engage in mass suicidal dives off cliffs when migrating.
5. Bats are not blind.
6. Ostriches do not hide their heads in the sand to hide from enemies.
7. It is not harmful to baby birds to pick them up and return them to their nests, despite the common belief that doing so will cause the mother to reject them.
8. A duck’s quack actually does echo, although the echo may be difficult to hear for humans under some circumstances.
9. The notion that goldfish have a memory span of just a few seconds is false.
10. Sharks can actually suffer from cancer.

#### INVERTEBRATES
1. It is a common misconception that an earthworm becomes two worms when cut in half.
2. Houseflies do not have an average lifespan of 24 hours.
3. According to urban legend, the daddy longlegs spider (Pholcus phalangioides) is the most venomous spider in the world, but the shape of their mandibles leaves them unable to bite humans, rendering them harmless to our species.
4. The flight mechanism and aerodynamics of the bumblebee (as well as other insects) are actually quite well understood, in spite of the urban legend that calculations show that they should not be able to fly.

#### PLANTS
1. Poinsettias are not highly toxic to humans or cats.
2. Flowering sunflowers do not track the Sun across the sky.

#### EVOLUTION
1. The word theory in the theory of evolution does not imply mainstream scientific doubt regarding its validity; the concepts of theory and hypothesis have specific meanings in a scientific context.
2. Evolution does not attempt to explain the origin of life or the origin and development of the universe.
3. Humans did not evolve from either of the living species of chimpanzees.
4. Evolution is not a progression from inferior to superior organisms, and it also does not necessarily result in an increase in complexity.
6. Evolution does not “plan” to improve an organism’s fitness to survive.
7. Humans and (non-avian) dinosaurs did not coexist.
8. Dinosaurs did not become extinct due to being generally maladapted or unable to cope with normal climatic change, a view found in many older textbooks.
9. Mammals did not evolve from any modern group of reptiles.

#### HUMAN BODY AND HEALTH
1. Waking sleepwalkers does not harm them.
2. In South Korea, it is commonly and incorrectly believed that sleeping in a closed room with an electric fan running can be fatal.
3. Eating less than an hour before swimming does not increase the risk of experiencing muscle cramps or drowning.
4. Drowning is often thought to be a violent struggle, where the victim waves and calls for help.
5. It is a common misconception that hydrogen peroxide is a disinfectant or antiseptic for treating wounds.
6. Human blood in veins is not actually blue.
7. Exposure to a vacuum, or experiencing uncontrolled decompression, does not cause the body to explode, or internal fluids to boil.
8. Antibiotics do not cure the common cold, because it is caused by a virus infection against which antibiotics are useless.
9. A person doesn’t become resistant to certain antibiotics.

#### SENSES
1. All different tastes can be detected on all parts of the tongue by taste buds, with slightly increased sensitivities in different locations depending on the person, contrary to the popular belief that specific tastes only correspond to specific mapped sites on the tongue.
2. Humans have more than the commonly cited five senses.

#### SKIN AND HAIR
1. Water-induced wrinkles are not caused by the skin absorbing water and swelling.
2. Shaving does not cause terminal hair to grow back thicker or coarser or darker.
3. Hair and fingernails do not continue to grow after a person dies.
4. Hair care products cannot actually “repair” split ends and damaged hair.

#### NUTRITION, FOOD, AND DRINK
1. Eight glasses or two to three liters of water a day are not needed to maintain health.
2. Sugar does not cause hyperactivity in children.
3. Alcoholic beverages do not make one warmer.
4. Alcohol does not necessarily kill brain cells.
5. A vegetarian or vegan diet can provide enough protein for adequate nutrition.
6. Swallowed chewing gum does not take seven years to digest.

#### HUMAN SEXUALITY
1. There is no physiological basis for the belief that having sex in the days leading up to a sporting event or contest is detrimental to performance.

#### BRAIN
1. Mental abilities are not absolutely separated into the left and right cerebral hemispheres of the brain.
2. Until recently, medical experts believed that humans were born with all of the brain cells they would ever have.
3. Vaccines do not cause autism or autism spectrum disorders.
4. People do not use only ten percent of their brains.

#### DISEASE
1. Drinking milk or consuming other dairy products does not increase mucus production.
2. Humans cannot catch warts from toads or other animals; the bumps on a toad are not warts.
3. Neither cracking one’s knuckles nor exercising while in good health causes osteoarthritis.
4. Eating nuts, popcorn, or seeds does not increase the risk of diverticulitis.
5. The Trendelenburg position (lying on the back with the feet elevated) for treating hypotension or shock is not supported by evidence and may in fact be harmful.
6. Stress plays a relatively minor role in hypertension – contrary to common belief.
7. In those with the common cold the color of the sputum or nasal secretion may vary from clear to yellow to green and does not indicate the class of agent causing the infection.
8. In general, Vitamin C does not prevent the common cold, although it may have a protective effect during intense cold-weather exercise and may slightly reduce the duration of colds.
9. In people with eczema, bathing does not dry the skin and may in fact be beneficial.

#### MATERIALS SCIENCE
1. Glass does not flow at room temperature as a high-viscosity liquid.
2. Most diamonds are not formed from highly compressed coal.

#### MATHEMATICS
1. When an event with equally probable outcomes comes out the same way several times in succession, the other outcome is not more likely next time.
2. There is no evidence that the ancient Greeks designed the Parthenon to deliberately match the golden ratio.

#### PHYSICS
1. It is not true that air takes the same time to travel above and below an aircraft’s wing.
2. Blowing over a curved piece of paper does not demonstrate Bernoulli’s principle.
3. The Coriolis effect does not determine the direction that water rotates in a bathtub drain or a flushing toilet, so this direction is not influenced by location.
4. Gyroscopic forces or geometric trail are not required for a rider to balance a bicycle or for it to demonstrate self-stability.
5. The idea that lightning never strikes the same place twice is one of the oldest and most well-known superstitions about lightning.
6. A penny dropped from the Empire State Building will not kill a person or crack the sidewalk.
7. When the ambient temperature is low, temporarily decreasing the temperature setting on a building’s programmable thermostat.

#### PSYCHOLOGY
1. There is no scientific evidence for the existence of “photographic” or eidetic memory (the ability to remember images with so high a precision as to mimic a camera).
2. Schizophrenia is not the same thing as dissociative identity disorder, namely split or multiple personalities.

#### SPORTS
1. Abner Doubleday did not invent baseball.
2. The black belt in martial arts does not necessarily indicate expert level or mastery.

#### HEBREW BIBLE
1. The forbidden fruit mentioned in the Book of Genesis is commonly assumed to be an apple, and is widely depicted as such in Western art.

#### BUDDHISM
1. The historical Buddha was not obese.
2. The Buddha is not a god.

#### CHRISTIANITY
1. There is no evidence that Jesus was born on December 25.
2. Nowhere in the Bible does it say exactly three magi came to visit the baby Jesus, nor that they were kings, rode on camels, or that their names were Casper, Melchior and Balthazar.
3. The Immaculate Conception is not synonymous with the virgin birth of Jesus, nor is it a supposed belief in the virgin birth of Mary, his mother.
4. The idea that Mary Magdalene was a prostitute before meeting Jesus is not found anywhere in the Bible.
5. Roman Catholic dogma does not say that the pope is either sinless or always infallible.

#### ISLAM
1. A fatwā is a non-binding legal opinion issued by an Islamic scholar under Islamic law.
2. The word “jihad” does not always mean “holy war”; literally, the word in Arabic means “struggle”.
3. The Quran does not promise martyrs 72 virgins in heaven.

#### LITERATURE
1. Frankenstein was not the name of the monster in the novel Frankenstein; or, The Modern Prometheus by Mary Shelley; rather it was the surname of the monster’s creator, Victor Frankenstein.

#### MUSIC
1. “Edelweiss” is not the national anthem of Austria, but is in fact an original composition created for the musical The Sound of Music.
2. “Twinkle Twinkle Little Star” was not composed by Wolfgang Amadeus Mozart when he was 5 years old; he only composed variations on the tune, which originated from a French folk song, and only at the age of 25 or 26.

#### INVENTIONS
1. George Washington Carver did not invent peanut butter, though he reputedly discovered three hundred uses for peanuts and hundreds more for soybeans, pecans, and sweet potatoes.
2. Thomas Crapper did not invent the flush toilet; flushing toilets were first used in the Indus Valley Civilization, around the 26th century BCE.
3. Thomas Edison did not invent the light bulb.
4. Henry Ford did not invent either the automobile or the assembly line.
5. Guglielmo Marconi did not invent the radio, but only modernized it for public broadcasting and communication.
6. Al Gore never said that he “invented” the Internet, although Gore did say, “During my service in the United States Congress, I took the initiative in creating the Internet.
7. James Watt did not invent the steam engine, nor were his ideas on steam engine power inspired by a kettle lid pressured open by steam.

#### COMPUTING
1. Computers running Mac OS X are not immune to malware such as trojan horses, although malware for this operating system is significantly less common.

#### TRANSPORTATION
1. Toilet waste is never intentionally jettisoned from an aircraft.
2. Automotive batteries stored on a concrete floor do not discharge any faster than they would on other surfaces, in spite of worry among Americans that concrete harms batteries.