# No Man’s Sky and Procedural Generation

Procedural Generation is getting more and more used in the gaming industry to make huge and interesting worlds. 

Huge in this context means quintillions of planets and stars in a game like No man’s sky and more recently over 1000 planets in Bethesda’s latest game Starfield.

One new to this might wonder how is it even possible to create near infinite believable worlds by using an algorithm. This post hopes to shed some light backstage, and de-mystify procedurally generated worlds.

# Perlin Noise and How it works

Naturally we need randomness if we want to generate functionally limitless worlds.

But how do random numbers help us generate worlds? They sound completely unrelated. 

For simplicity we will focus on how do we generate the terrain map of an area using procedural generation.

A terrain map tells us the heights of all the points of the map.

If we could fill this map with numbers. Connect all of the points and give them a texture.

We would have something resembling a landmass or a terrain.

Now the questions remains, how do we fill this map with values?

Filling them with random numbers will not suffice. As this will lead to sudden increases and decreases in heights of relatively close points which is not something that is natural in the real world.

But the idea is not totally scrap, all we really need is some sort of smoothness between two extreme values of height.

If you are confused by my wording, which of these look more natural?

![Figure_1.png](https://github.com/dubeyKartikay/blog/blob/cbe087679d0477385ee918df9bc64eb1b0791e07/No%20Man%E2%80%99s%20Sky%20and%20Procedural%20Generation/Figure_1.png?raw=true)

![Figure_1.png](https://github.com/dubeyKartikay/blog/blob/cbe087679d0477385ee918df9bc64eb1b0791e07/No%20Man%E2%80%99s%20Sky%20and%20Procedural%20Generation/Figure_1%201.png?raw=true)

The latter graph looks to be more like a hilly area right? While the first one is just too chaotic to resemble anything natural.

But no place is that hilly you say? well look at this graph

![Figure_1.png](https://github.com/dubeyKartikay/blog/blob/cbe087679d0477385ee918df9bc64eb1b0791e07/No%20Man%E2%80%99s%20Sky%20and%20Procedural%20Generation/Figure_1%202.png?raw=true)

all i really did was scale the small y values to -1 and 1 and everything else to -100 and +100.

This is how perlin noise helps us in generating believable worlds.

Perlin noise on its own is not good enough to generate realistic looking areas but we can modify and tweak it to our wishes and generate something more typical.

Heres the same things but in 3 dimensions,   

![Figure_1.png](https://github.com/dubeyKartikay/blog/blob/cbe087679d0477385ee918df9bc64eb1b0791e07/No%20Man%E2%80%99s%20Sky%20and%20Procedural%20Generation/Figure_1%203.png?raw=true)

This forms the basis for procedural generation in all games whether it is Minecraft or No Man’s Sky. 

# Overview of what No man’s sky does

However useful perlin noise is, as you could see in the 3d graph, it produces terrain what many would call ‘tacky’. 

Sean Murray, the one of the creators of No Man’s Sky has an hour long talk discussing this and how to tweak and modify perlin noise and other noise generating functions to produce interesting yet natural looking worlds.

Now to give a brief summary,

They do a multitude of things in succession to produce interesting noise.

************Layering -************ This is a pretty common things in the games industry to layer noise on top of each other to even out extremities. 

************************************************************Filtering and Image processing************************************************************ - After we have a base of noise, we filter out the noise we deem unimportant and/or bland and enhance/highlight features that are interesting using Image processing.

********************************************************************************************DEM and using real world data to tweak noise -******************************************************************************************** This is using real world data to terrain to tweak our noise field to be more earthy looking. However this can produce terrain that is simply too boring. As walk though this world will feel like walking on earth, and walking on earth is pretty boring.

****************************Domain warping****************************  - This is the act of stretching and twisting noise to produce some interesting looking shapes.

No Man’s Sky uses all of these in tandem to work with each other to produce planets.