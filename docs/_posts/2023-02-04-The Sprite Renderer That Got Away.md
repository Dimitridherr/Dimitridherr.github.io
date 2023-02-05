---
layout: post
title: "The Sprite Renderer That Got Away"
date: 2023-02-04 21:35:00 -0000
categories: 
---

I have decided to do some bug testing and run our game as a standalone build instead of in the unity engine. When I did, I was surprised to see that some of the character UI was not showing at all. Everything else seemed to work just fine, so why not the UI? It was throwing a NullReference, a sign that some of my old code could do with more testing using the new practices of I have learned. It was easy enough to find the location of the problem, but I was struggling to figure out exactly why it worked in the editor while it did not work in a standalone build. I then realized that an issue that works in one build and not in another could only come down to two things: editor only features or timing.

When the level starts, it first loads and assigns the appropriate drawing/sorting order for all the appropriate sprites to ensure proper overlap. To do this, it needs to get the SpriteRenderer object of the sprites. To ensure this is done before the first frame, this is called immediately in their Start() method. After looking at how the SpriteRenderer in question was assigned, I realized that it was assigned in a Start() statement as well. This made it clear that based on the runtime of each individual Start() statement, there was a chance that the SpriteRenderer would be called before it was assigned too. This was eye opening, as this reminded me the importance of knowing the timing of when things load in the game. Luckily, the solution was simple. Instead of assigning the SpriteRenderer, it can be made a property and be obtained directly when called. This will work fine for getting a single object attached to a gameobject, however, this should not be done if the object could potentially not be attached or its location within a gameobject is dynamic. I wanted to outline this particular bug today due it being a good reminder that timing can drastically change the outcome of operations. 

I am continuing to bug test our game, a process I find quite fun actually. All the bugs thus far have been easy fixes that push the game that much closer to completion. 

<img src="/Images/astralprojection.jpg" alt="Remoting">