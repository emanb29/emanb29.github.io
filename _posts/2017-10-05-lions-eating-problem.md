---
published: false
title: The Lions-Eating Problem
---
_This article continues my eclectic collection of recently-learned-topic reviews. Essentially, when I’m in class or work and come across a topic I find particularly difficult, counterintuitive, or just interesting, I’ll write one of these to hopefully ease the learning process for the next poor sucker who needs to learn the topic. Today, I’ll be discussing the Lions-Eating Problem, otherwise known as the lions and lambs game._


In this “game” (ie, scenario), there is a single defenseless lamb, and some hyper-rational but hungry lions. These lions always act in their best interest, with the following goals and priorities:

 1. Stay alive
 2. Eat
 
That is, their compulsion to stay alive will beat out their compulsion to eat. The catch is, once a lion eats, it falls asleep and becomes vulnerable to being eaten by any other lion. The question posed then is as follows: For what number\[s\] of lions will the lamb be eaten?

The first (flawed) solution that might come to mind (as did for me) is that the lamb will never be eaten when there are multiple lions. After all, if a lion eats the lamb, it then becomes prey for all other lions. However, this is not correct. Below I’ll outline the correct solution, and hopefully the reason the previous solution is flawed will become apparent.
Let’s build a solution somewhat inductively. Suppose there is only 1 lion. Naturally, the lion will eat the lamb, since it is hungry and there are no other lions to worry about.

Now, let’s look at the case of 2 lions. Suppose one lion chooses to eat the lamb. Then the now-fed lion falls asleep and the remaining lion eats it. Note that the situation after the first lion has eaten is equivalent to the situation with only 1 lion: one defenseless creature, and one active lion. Since these lions are hyper-rational and their primary goal is survival, neither lion would allow themselves to be eaten in this manner. Therefore, with 2 lions, the lamb survives.

Moving on, let’s evaluate 3 lions. If one lion (the fastest or closes to the lamb) eats the lamb and falls asleep, we’re left with a situation equivalent to that of 2 lions: neither of the awake lions wish to eat the sleeping one, for they will then be eaten themselves. Thus, the lion who eats first may sleep soundly knowing she is safe. Since there is no threat to her life, she realizes she is free to satisfy her lesser need of wishing to eat.

You may now see a pattern forming. Let’s go one further, and then review if this pattern holds. With 4 lions, if one eats, it becomes the lamb in the 3-lion situation, and will thus be eaten. Since the lions wish to live above all else, no lion will eat and thus become prey for one of the remaining 3, and so the lamb may graze in peace.

As we can see, any odd number of lions will eat, and any even number will not. This is because with an even number of lions, an odd number of lions will be left to eat the first lion. With an odd number, a lion may eat leaving an even number in a stalemate. This seems circular, until you remember the base case of a single lion, who may obviously eat without being eaten.

_This article was primarily based on the week 2 seminars for the ECN214 - Games and Strategies class offered by Queen Mary University of London’s School of Economics and Finance in the Fall of 2017._
