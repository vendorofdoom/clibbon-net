---
title: "Optimal Kitchen"
date: 2023-08-12T19:08:52+01:00
draft: false
summary: Using genetic algorithms to play god with my kitchen, and learning about ML alignment and setting good reward functions along the way.
---

On moving into a new house early in 2020, we naturally unpacked all of our things into sensible places, and then got on with our lives. Two years later I moved job, and found myself with a spare week in between roles - the perfect chance to tackle a problem that had been annoying me for a long time.

![my_kitchen](/my_kitchen.jpg)

What is the best place to put the knife block? I was considering mounting it on the wall and drilling into these lovely tiles so this wasn't a decision to take lightly. And while I was thinking about the best place for the knife block, why not optimise the whole kitchen? The goal was to make every journey I took inside the kitchen as efficient as it could. This is a global optimisation problem. To start optimising this, I need a way to identify places in the kitchen. Going left to right, I can label the positions numerically (including the fridge as position 0, and the side table as 10). 

![labelled kitchen](/labelled_kitchen.jpg)

So what are we optimising? I have range of locations in the kitchen where things could live, for example the top shelf of cupboard number 3. I also have a range of journeys I make, for example the steps to make a cup of tea (properly) are:

1. Go to kettle (and boil water)
2. Go to the sink (and fill kettle with water)
3. Go back to kettle stand (replace kettle and set it boiling)
2. Go to where the mugs are (and select a mug to suit your current mood)
3. and so on...

If I create a graph of how far away these locations are from eachother, I could then start mapping these journeys onto this graph to see how long they take.

![my kitchen as a graph](/kitchen_unit_graph.jpg)

We want to optimise for shortest journey, but that's not all I need to consider:
* Some things need sockets.
* Some things are fiddly, like spices, or want to be seen (like nice mugs).
* Multiple things can occupy the same space if they are small enough.

I settle on a solution where the kitchen is represented by two data objects, a graph of location like the one shown above, and a dictionary mapping locations to the things they contain.

```
{
  place_1: [
    thing_1,
    thing_2
  ],
  place_2: [
    thing_3
  ]
}
```

After [some coding](https://github.com/clibbon/OptimalKitchen/tree/trunk) I had a working evolutionary algorithm which could be used!

![fitness over time](/algorithm_v1_fitness.jpg)

This graph shows the evolution over time of two aspects of the total fitness score of the best individual in the population, over 1000 populations. This looks like a classic evolutionary algorithm output, a rapid increase in performance, followed by periods of stability before step changes in performance as new configurations are discovered. Looking at the outputs, some pretty sensible configurations are being suggested; and they are pretty creative too. This individual innovates on the current arrangement by putting all the tea and coffee equipment together in unit 5, close to the sink.

```
'5_Cupboard_bot': {Thing(name='mugs', needs_surface=False, needs_socket=False, size=1, visibility_need=0.3)},
 '5_Cupboard_mid': {Thing(name='coffee_grinder', needs_surface=False, needs_socket=True, size=0.3, visibility_need=0),
  Thing(name='glasses', needs_surface=False, needs_socket=False, size=0.5, visibility_need=0.3)},
 '5_Cupboard_top': {Thing(name='bags', needs_surface=False, needs_socket=False, size=1, visibility_need=None)},
 '5_Side': {Thing(name='common_spices', needs_surface=True, needs_socket=False, size=0.3, visibility_need=1),
  Thing(name='kettle', needs_surface=True, needs_socket=True, size=0.3, visibility_need=None),
  Thing(name='toaster', needs_surface=True, needs_socket=True, size=0.3, visibility_need=None)},
 '5_Side_drawer_bot': {Thing(name='ferments', needs_surface=False, needs_socket=False, size=0.5, visibility_need=0.7),
  Thing(name='flavours', needs_surface=False, needs_socket=False, size=0.5, visibility_need=0.8)},
 '5_Side_drawer_mid': {Thing(name='coffee_stuff', needs_surface=False, needs_socket=False, size=0.3, visibility_need=0.7),
  Thing(name='veg_box', needs_surface=False, needs_socket=False, size=0.3, visibility_need=1)},
 '5_Side_drawer_top': set(),
```
