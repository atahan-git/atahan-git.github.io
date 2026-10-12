---
title: "7 - Made in Mars"
date: 2021-05-23
categories: 
  - "life-update"
  - "devlog"
coverImage: "all-the-smelters.gif"
---

### New Graphics, New Backend, Open Source, and More!

How the time flies! It's been already 8 months since my last blogpost, but actually not \*that\* much new happened with the game, because I was _a little busy_ with school. With that said I had a lot of ideas on how to make the game different from the other similar games, and also there has been some system/graphical changes. You might have seen them if you are following me [My Twitter](https://twitter.com/atahan_game_dev), but it would be too bad to not mention them here!

### New graphics

<figure>
  <img src="/images/all-the-smelters.gif" alt="So many options!" />
  <figcaption>So many options!</figcaption>
</figure>

Right around October, I started to seriously consider the graphical look of the game. So I picked a building that will be seen a lot (the furnace), and made a bunch of variations on what it could look like. In the end I decided on the style numbered 4, with sharp edges and a pixelated look, complete with smooth animations. I picked pixelart style because in most zoom levels they were indistinguishable from the high rez versions, but with the sprite-based animation, low rez made the overall file size significantly smaller.

With the graphic style generally set (at least until I decide to hire an artist), I made some more buildings:

<div>
  <figure>
    <img src="/images/Cutter-and-Coiler-245x300.gif" alt="Cutter and Coiler" />
    <figcaption>Cutter and Coiler</figcaption>
  </figure>

  <figure>
    <img src="/images/better-gfx-comp-1.gif" alt="Drillers really have a nice feel" />
    <figcaption>Drillers really have a nice feel</figcaption>
  </figure>

  <figure>
    <img src="/images/press-goes-press.gif" alt="Press" />
    <figcaption>Press</figcaption>
  </figure>

  <figure>
    <img src="/images/landing-rocket-better-3.gif" alt="Rockets" />
    <figcaption>Rockets</figcaption>
  </figure>
</div>

Just for fun I also made a menu:

<figure>
  <img src="/images/Menu-temporary.gif" alt="A fun rudimentary menu" />
  <figcaption>A fun rudimentary menu</figcaption>
</figure>

Also around this time I decided to make the previous node based item crafting recipe system from scratch.

### The Recipe Node System

One of the things I wanted a lot for my game was in-game modding capability. This is due to the fact that it is a mobile game, so it is pretty much impossible to 'mod' externally. Also it seems that neither of the big mobile platforms (Apple and Android) likes scripting capable mods. So anything to the level of the likes of Skyrim or even Factorio modding capabilities is impossible, just because the markets doesn't let it. You can't even put a simple sandboxed compiler, or precompiled Lua bytecode in your game!

With these problems, I decided that I would let players add buildings and change recipes in game. This meant building a mobile friendly recipe node editor. I looked at the asset store for anything similar, but none of the available solutions work in runtime! Everything only works in the Unity editor, which makes sense. It's not common for games to ship a node system for the users after all.

So with that I made my own:

<figure>
  <img src="/images/New-ingame-crafting-tree.gif" alt="So Complicated." />
  <figcaption>So Complicated.</figcaption>
</figure>

This was surprisingly difficult though. Mainly because it was my first time making something like this, so I approach it in a completely wrong way. I made the visuals code very entangled with the underlying node storage method. Imagine every time you click to make a new connection, the visuals would add a new visual port, which is heavly entangled to the underlying port storage object, and etc. etc.

I also hardcoded a lot of the things, like there are only 2 nodes, that 'extend' from a common Node base class, but the Node class is very barren with no functionality to stand on its own. Also all the ports were hardcoded to only connect to one other possible port.

This resulted in weird bugs where sometimes nodes would connect normally, but sometimes one port magically connects to two different nodes, or when you are trying to delete a connection suddenly you get 25x null reference exceptions...

Around this time I was also having a lot of problems with the belt system, and everything else in the game in general. Everything was 5 layers deep, with graphics entangled to the logic, and there were weird bugs everywhere that would only happen sometimes.

To my rescue came my discord friends.

### The Refactor

Around June-March, due to the code becoming impossible, I pretty much did not touch it at all. Between school and the inherent difficulty, I took a break. I wasn't just slacking however. During this time I made a different game, [VR Walking Simulator](https://store.steampowered.com/app/1539640/VR_Walking_Simulator/), and you can find it on Steam!

But came summer, I knew I had to do something, so I asked my pro friends on our ['factory games'/Recall Singularity discord.](https://discord.gg/yJrexRZgcd) I had been on this channel for a while now, and really enjoyed my time there. The channel is originally for the game called Recall Singularity, a game where you build factories on flying spaceships, but so far there are a bunch of developers there making a bunch of different factory games or other projects where we all share our progress and help each other out. Most of them use Godot/Rust, but still most concepts do carry over to my game.

Back to the topic: When I was talking about these problems I had, mainly how it becomes impossible to unit test anything if you have graphics and logic combined, Tom (Thanks Tom!) told me how I should really separate those two. After some discussion, it finally dawned on me. I was always confused on how to separate logic because I was so dependent on the whole Monobehaviour thing, but the idea is to have a twin pure Csharp class for every real world object monobehaviour you have.

So I set out to do that. Around this time I was also thinking about streamlining the belt system and make a 'building-connector-belt' system instead. So I made a new project, made a simulation completely independent of any graphics, and made a simple ASCII rendered:

<figure>
  <img src="/images/ASCII-Factory.gif" alt="Cute ASCII Factory is Cute." />
  <figcaption>Cute ASCII Factory is Cute.</figcaption>
</figure>

This was so nice, and so fresh, and without all the other stuff the factory had, it was so easy to do. This entire sim, with complete unit tests, took me 3 days. In comparison the previous factory system took me a few good months to manage (although that also includes all the graphics, so not a fair comparison).

This new system was so good, I then integrated it to the main game, re-integrated useful bits, and got the game running (with the whole player building stuff) in just a week!

<figure>
  <img src="/images/Connector-in-action-1.gif" alt="Drilling, Splitting and Smelting." />
  <figcaption>Drilling, Splitting and Smelting.</figcaption>
</figure>

To help newbies (like me!) out in case they also want to build such a complicated factory game, I made this ASCII factory code [open source on Github](https://github.com/atahan-git/UnityFactorySimulator).

I was considering for a while now, but after putting that little toy factory up and also after watching [this amazing youtube video that argues we should get rid of any and all copyright](https://youtu.be/jIM6dN3ogbk), I put [the entire game on Github](https://github.com/atahan-git/MadeinMars) for any and all to see!

More info on the Open Source-ness later in the blogpost.

### The New System

When doing my own playtesting I realized something with my system. With the whole weird belts splitting and remerging and everything, most of the time I was doing three simple things: Merging building outputs, Transporting items, and Splitting items to multiple buildings.

So I made a streamlined system. You have a connector, which is an all purpose splitter/inserter, that can take items from/to buildings/belts, and perfectly balances all its inputs and outputs in the process. The belts only can transport items from point A to point B, and they can't do any merging on their own.

<figure>
  <img src="/images/Connector-Splitter-1.gif" alt="Can you see the load balancing?" />
  <figcaption>Can you see the load balancing?</figcaption>
</figure>

### More Streamlining

Another big change I did was to get rid of the 'player inventory'. In Factorio, you are the player, and you always have your inventory as the main way of transporting/building/crafting/and everything. The player is very central to the game, and it is very powerful.

However in mobile it is fairly clunky to have a player moving around, so many games get rid of the player. However they do NOT get rid of the player, they just make the 'player' an all powerful main base structure. This is the case in pretty much all the base building games on mobile I've seen so far, including the likes of Mindustry and recently Drill Down.

I find this idea of a base really restricting, and also in my game I found it very clunky and annoying. Here is a list of reasons why it is bad in my opinion:

- Everything needs to come back to base and base has limited in/out space
- Very annoying if you need this just one item, and now has to route it back to the base
- Makes it really hard to expand, making multiple based become confusing and doesn't really solve anything.
- Crafting things is clunky. So is keeping track of what you need to craft.
- It feels weird to have a central building of everything in a Mars Colony.

So I decided to get rid of the central base. In its place I added drones. Smart Drones.

<figure>
  <img src="/images/Drones-build.gif" alt="Drones Build." />
  <figcaption>Drones Build.</figcaption>
</figure>

Now to build things, you simply put down 'constructions' which are like the blueprint outlines in Factorio. Then your drones venture around and collect all the necessary materials from storage units. They also work in the same way to de-construct buildings, they destroy them and put the resources in a storage unit.

This also puts emphasis to production. Because there is no player, everything needs to be crafted. There is no more being able to wait 5 minutes for waiting the crafting of the oil pump to finish.

Of course, that would make the early game too complicated, but there is a nice solution, the earth! In the early game, you can order complicated building materials from Earth, for a price. So player inventory crafting is replaced with ordering from the earth for a hefty price, instead of your own 'hand crafting time' and sanity.

### Old Habits Die Hard

It was however fairly difficult to 'follow good coding standards' all the way. When making the construction system, I did it again. Construction was controlled by the 'world buildings', and drones were their own units.

To fix this once and for all, I made all the world object 'transient'. They could at no point hold any important info, and everything about the game must be able to be contained in the 'simulation layer'. I made it so that the simulation at no point interacts with the world, other than some performance reasoned 'callbacks' that they run when the inventory contents are updated for example.

With this I also changed the save system to save the underlying factory entities instead of saving the world objects.

With my newfound experience, contruction/drones system was fairly easy to refactor. However it was reaaaallly tedious either way. It took me 3 days to botch up drone code and write myself to a corner. It took me another 3 days to untangle everything and 'do it the proper way'. So I still have some way to go.

It seems I'm still somewhat unable to 'write the code in the good way' first time around, but now I can at least see my mistakes and refactor when needed. With the experience, I imagine it will become easier.

Of course, I also saw a lot of the other systems also suffered from this same issue. I also refactored the Recipe Node System and extended it a bit to be able to add research nodes (that currently do nothing). I will also refactor the 'order from orbit' code as well, because it is also buggy and became so messy I can't extend it in the ways I want.

But for now, lets talk about the new 'strategic shift'.

### Meaningful Decisions in a Factory game?

In Factorio the biggest decisions you make are whether you are going to make a Main Bus factory or something more directly fed. But in the end, every player makes the same buildings, crafts the same items, and launches the same rocket.

So I thought with my game, I can give players some choice. This was also I wanted to include a significant story, and it would have been sad to have the story just as side flavor text. So I came up with the idea that you should be able to 'pick paths' by making different research.

Also to make things more interesting, during this entire process you will have to make difficult decisions because Earth will be bust soon. Global warming has become terrible, and most people want in your Mars Colony, especially some billionaires. How will you handle the masses and the influence of money?

Currently I planned 3 different stages for the game.

**Early-Game:** Pretty standard, and mostly everyone will be doing the same thing factory-wise. However the story will start to diverge based on your choices here.

**Mid-Game:** In this stage, there will be significant play changes to how you build your factories. I imagined 3 main 'ways of play' based on which buildings/upgrades you unlock. I imagine some of these will also depend somewhat in story, eg if you pick options that make your people sad, then you will probably advance 'automation' techs so you are not so dependent on them.

- **Worker Heavy:** As the name suggests, if you go down this path it will be about making the most of your workers by making them happy and more effective. So your main problems will be **planning** so that your workers are nearby buildings that make them happy.
- **Automation Heavy:** In this path, you slowly replace your workers with robots. So you will need to deal with people less, but need more raw materials to build expensive robots. So your main problem will be **throughput** as you make the automation a reality.
- **Complexity Heavy:** I'm not perfectly sure how this one will go, but the main idea is that by making highly complicated crafting items, you do not need as many workers or raw materials. So your main problem will be **complicated recipes**, and making all the belts work.

**End-Game:** With that, you will have multiple 'final projects' to finish the game. These of course follow from what you did in the previous stage, but you are not necessarily tied to any one of them. Or at least thats the plan.

- **Terraform Mars:** The classic ending. Employ your people to plant seeds, and make Mars liveable for everyone. A truly giant effort for everyone.
- **Cyborg Revolution:** Flesh is weak, why change Mars when we can change ourselves? In this ending you will modify your people to be able to live on mars without suits. Lets hope everyone likes surgery.
- **Robot Revolution:** Flesh is unnecessary. Why try to change it, while you can make it anew and better. With this you completely replace your workforce with robots that will listen to your every whim. Best part? There are no puny governments on Mars to stop you!
- **Help Earth:** During any of the above endings, you will have the chance to welcome incoming refugees from Rarth. Will you be able to house them all in the midst of your ambitions? Will your factory be able to scale fast enough, or will it crumble under its own weight?
- **Reject Earth:** Another option is to reject helping your poor Earthen people. Will they try to attack you? How about the colonists who left family behind? Will you be able to be self sufficient when Earth stops trading with you?

So those are the general ideas. Following the game you will get story bits from time to time and also at certain milestones, where you will have to make decisions. Also what you research will be important, as simply trying to unlock everything is too difficult you have to pick priorities.

### Why Open Source?

I truly believe the world will be a better place if we all magically dropped all copyright and made every IP free. I think these two youtube videos make a lot of sense, and explains pretty much everything I would like to explain much better than me, so go watch them:

https://youtu.be/jIM6dN3ogbk

https://www.youtube.com/watch?v=mnnYCJNhw7w

Although they are a bit long, totaling to 50 minutes, so here's my short summary:

Copyright and Patents only really help big corporations, and small creators never really benefit from it. It also stifles competition, and the supposed benefits are quite little.

Instead of that, the video proposes we make everything free for the public, and switch the payment time from before consumption to before production. So it basically says everything should be funded through patreon or kickstarter.

This has a lot of benefits for the consumer. Instead of having to do with whatever gets produced, you can fund the projects you really like, and as everything is free you can watch/play more stuff. Imagine instead of paying for $60 each for 5 games, you can play them all and pay like $300 to the project that you really like.

This also benefits (assuming people gets used to this system) the creators. If it becomes the norm, then it will be much easier to budget as you get paid along the way, and also if a project is wildly popular then you can up scope.

Also imagine your favorite has this one bug or you wish it had this cool weapon. Then you can just go ahead and fix/add it. Or even pay someone to do it.

It also talks about 'trusting consumers to do good', in the way that if someone blatantly steals something you have then they will boycott it. Which does happen from time to time, especially when big corporations steal from small developers too much.

With that I decided to make my game open source. I know there needs to be a huge culture shift for this to be viable, but I'm a small developer still studying so I have zero risk. It may even help me by being unique!

### My Strategy

So with that, my main strategy is going to be keeping the game open source all the time, while putting the game on the playstore for free. To make anything out of the game, when I release I will also make a Patreon page, and a Kickstarter.

Then I will wait. If the the Kickstarter or the Patreon is successful, then it's amazing, I can spend more time on the game as I won't need to make money to survive anymore! I can also hire an actual artist to improve the art, and also maybe even expand the team by adding another programmer.

If it is not successful, then I will decide there was no market, or it wouldn't have been sold anyways, so I will put it away as a portfolio project (and maybe return to it later as a hobby project again).

That all also depends on me simply being able to finish the game. But I feel like this summer is the charm. Especially after seeing the positive reception to the [VR Walking Simulator](https://store.steampowered.com/app/1539640/VR_Walking_Simulator/), I feel myself _filled with determination._
