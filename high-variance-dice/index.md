+++
title = "High Variance Dice"
description = "How changing your dice can change your game experience."
template = "blog_post.html"
date = 2023-04-03
draft = true
[taxonomies]
tags = ["ttrpg", "gaming"]
+++
In April of last year the gaming-supplies company Wyrmwood Gaming launched a [Kickstarter campaign](https://www.kickstarter.com/projects/wyrmwood/dice-by-wyrmwood/description) for their new dice lineup. Alongside beautiful new sets of gemstone, resin, glass, and carved wooden dice, they announced a new numbering system that can now be requested with your dice set called “High-Variance”.
<!-- more -->

In a [video](https://www.youtube.com/watch?v=z1oMnZW7Ry8) released on their YouTube channel, Wyrmwood CEO Doug Costello explains their new system and the idea of maintaining the average roll value on a dice while increasing the probability of rolling values on the lower and higher ends of the number array.

### What Are High-Variance Dice?
I’m only going to cover the functionality and math behind these types of dice briefly in this post; I’m definitely not a mathematician or even well versed in these concepts. On the surface however, they are easy to understand and Wyrmwood does a great job explaining them in the video linked above and this short description from the Kickstarter page (also referenced above):
> “High Variance dice are dice that have been shifted to exaggerate extreme results, without sacrificing the overall average value of the rolls. For example, on a typical d6, you have the numbers 1, 2, 3, 4, 5, 6. The average roll is a 3.5. On a High Variance d6? 1, 1, 3, 4, 6, 6. The average roll is STILL a 3.5, however your chances of great success - or massive failure - have just doubled.”

So let’s talk about these dice. Today, I want to focus on their impacts on the game, how DMs and players can utilize them, how they may change the game, and how you can determine if these dice are right for you and your play-group.

Wyrmwood covers a few examples of how or when these dice might be used in the game, such as a specific narrative context where the outcome is either a success or a failure (which might as well be a coin flip). Another being the dice mirror the nature of a player character or non-player character’s personality; maybe their chaotic nature tends to favor or strive towards the extreme with their actions. Or maybe there’s a mechanical context that overlaps the narrative, such as an environment that impacts the players and their abilities, which I find the most interesting.

### How Do They Work?
So let’s start by briefly explaining how these dice work looking at the d20. I’ve written a short program in Python3 that illustrates the probability difference between a standard d20 that has the number array of 1-20, representing each value once, and the high variance d20. You can compare the two as well as each other dice type in the image below from Wyrmwood’s Kickstarter page.

![Displays the number spread for both standard, and high variance dice. The spread for high variance is as follows: D4 1,1,4,4. D6 1,1,2,5,6,6. D8 1,1,2,3,6,7,8,8. D10 1,1,2,2,3,8,9,9,0,0 (this is the same for percentile). D12 1,1,2,2,3,4,9,10,11,11,12,12. and D20 1,1,1,2,2,3,3,4,5,6,15,16,17,18,18,19,19,20,20,20](dice-reference.png "Dice Reference")

The program I’ve written will roll both a standard d20 and a high-variance (weighted) d20 10,000 times each and plot the results on a bar graph using Matplotlib. The standard d20 has an evenly distributed probability of 5% to roll any of the values, while the weighted d20 has an increased probability to roll the values of 1 and 20 at 15%, 2, 3, 18, and 19 at 10%, while maintaining the 5% chance to roll the values of 4, 5, 6, 15, 16, and 17, and eliminating the probability of rolling any values from 7 to 14.

![Plotted results of rolling both a standard d20 and a weighted “high-variance” d20 10,000 times each.](dice-plot.png "Standard vs. High-Variance D20 Results")

As shown in the graph above, this removes the probability of rolling in the middle of the array and pushes higher odds to the ends, allowing for both great success, or great failure. This can present some interesting dynamics in d&d 5th edition when you consider the d20, which is at the core of the game. Outside of damage rolls, miscellaneous mechanics, or random dungeon-master shenanigans, the d20 is right smack in the middle of everything that players and DMs mechanically do in the game. From attacking in combat or casting an attack spell to determining if a player can climb or use an object, to deciding if an unconscious player lives or dies, and even the success or failure of some role-playing scenarios. The d20 is how it’s all determined.

### How and Where Could These Dice Be Used?
You can think of d&d 5e (at its core) to be broken into three distinct elements. Now, bare in mind a lot of these ideas can overlap. I do not want to try and form some kind of absolute structure for the game, and neither should you, unless that’s what you and your playgroup want. However, for the purposes of this discussion, let’s consider the following.

First, there’s the narrative, which consists heavily of the role-playing done by both players and the DM, but may also include the setting and lore of a campaign. This is the most personal element of the game. It is your imagination realized.

Second is what I call the “design and strategy”. This consists of how the players build and use their characters, how they interact with the scenarios and obstacles presented to them by the DM, as well as how the DM chooses to build and present those obstacles. This is the overlap between both the first element, the narrative, and the last element, the mechanics.

The mechanics of the game (which you can interpret as-written in the SRD, or whatever house rules you and your playgroup agree on) are centered almost entirely around the d20. This is where you will find the majority of dice rolls as well as other things, like spell functionality, armor-class, and how ability score modifiers affect what you as players do. This is largely where our conversation regarding high-variance dice will be had in this post, with a touch of the other two thrown in. 

So what should you consider about these dice before using them? Well, you don’t really need to consider anything, if you don’t want to. Maybe the best approach to determining if these dice would be good for your and/or your play-group would be to just get some, or make your own (see the program) and play some sessions with them; play a few one-shots and see how they feel. Maybe utilize them in different scenarios to see where they might fit best for you and your friends. Maybe swap them out in different d20 situations and see how they might impact different roll types (saving throws, checks, attack rolls, ect). Experimentation in d&d is the best way to determine what’s right for you and your group. But you didn’t come here just to read “see for yourself”, so I’ll lay out some ideas on how to use these dice, just as some thought starters, and then we’ll discuss how they may impact the game, for better or worse.

As I mentioned earlier, the mechanics of d&d 5e rely heavily on the d20. Whenever a player interacts with most of the game's rules, it’s through this die. This is the meat-and-potatoes of 5e. So let’s dive into these rolls, and see how the high-variance dice affects them, starting with combat.

### Combat
Combat in 5e can vary widely among play-groups. In my opinion, combat is where most of the “house-rules” are determined. For example, my play-group decided that the way critical hit damage worked in the SRD wasn’t rewarding or impactfull enough. The odds of landing a crit are pretty low with a standard d20, and we noticed a lot of underwhelming critical damage when they did happen. So, instead of the SRD critical damage rules (double the number of damage dice rolled) we decided to add the maximum value of the damage dice to the normal dice roll. For example: if you were to crit with a dagger, instead of rolling 2d4 and adding any modifiers, you would roll 1d4 and add 4 then add any additional modifiers. This change made critical hits much more impactful for the players, as well as the DM. An NPC that crits a player will also deal more devastating damage balancing the overall impact of critical hits. 

**Attack Rolls**

How can high-variance dice impact attack rolls? Using these dice for attack rolls would create a more dramatic climate during combat; each attempted attack would boil down to almost certain failure or success, ultimately resulting in a coin flip, especially in the earlier character levels where ability score and proficiency are too low to alter the highest value on the low-end of the high-variance d20, a roll of 6. The combat system in the 5e SRD relies on a much more varied roll from the d20. It’s a delicate balance of chance with the dice roll, the attacking character’s skill (ability modifiers) and the defender’s AC, and all of these elements play a role in determining success or failure. Altering this balance of attack rolls with high-variance dice, in my opinion, would dilute the combat experience in d&d to mere coin flips, with one exception: critical hits.

The crit changes a few things regarding how high-variance dice affect attack rolls and combat. Because you now have a 15% chance to hit a natural twenty instead of a 5% chance, your average damage-per-attack effectively increases. Because the high-variance dice maintain the average roll-value of the standard dice, using them in place of standard damage rolls would not increase your damage output (I’ll touch more on this in a bit); however changing your attack dice will, because landing a natural 20 on an attack adds more dice rolls to the damage, increasing the average damage of the damage rolls. 

Considering the attack roll, high-variance dice can drastically alter how combat is done and how it plays out. I don’t believe that this would break the game necessarily (it would be great to make some simulations that could test this), but it will alter it significantly. If your group is looking for more excitement in combat with higher crit chance, then you may consider this. With that being said, make sure you talk it over with the rest of your play-group and understand that while your overall crit chances will increase significantly, this will dilute other elements of any PC or NPC by making their ability modifiers or any other elements that affect roll values less effective, and in some cases, completely useless. 

**Saving Throws**

Of course, the attack roll isn’t the only instance where the d20 is used in combat. What about saving throws? I realize that saving throws are found outside of combat as well, however they do appear more often during combat encounters. Like the attack roll, the saving throw is affected by both ability score and proficiency, and similar to AC, success or failure of a saving throw is determined by the DC (difficulty class) of the action being met by the character that is saving against it (what a mouthful). This is effectively the same system as attack vs. AC, with a slight change; the roll is not being made by the character taking the action, and instead being made by the target of the action. 

So how would high-variance dice affect this? The same way that it affects attack rolls. Even with the roll burden shifting to the target instead of the action-taker, functionally, saving-throws work pretty much the same as the attack roll. There’s a huge determining factor with the target’s ability modifiers, proficiency, and the action-taker’s ability DC in the success or failure of the roll. If a high-variance dice were to be played here, it would be almost as effective as flipping a coin. This takes away significant elements of the game, watering these mechanics down to almost nothing.

![Graph showing DC success on a standard d20, accounting for modifiers of -3 to +3.](dc-successes-standard.png "DC Success Spread for Standard Dice")

In the figure above, you can see the success rate of a standard d20 vs. DC (or you can also treat the x axis as AC, they function the same). Each element in this DC array presents a different probability of success, even when considering attribute bonuses or deductions. This distribution is at the core of the d20 functionality in 5e’s success-or-failure mechanics. Now consider the next figure:

![Graph showing DC success on a high-variance d20, accounting for modifiers of -3 to +3.](dc-successes-hv.png "DC Success Spread for High-Variance Dice")

With high-variance dice, when rolling against DC (or AC) and again factoring for bonuses or deductions, success and failure rate begin to tend towards 50% from the middle of the array. DC or AC of around 8 to 14 are almost coin flips. This takes away so much of what makes each PC or NPC unique in their design by both the players and the DM.

**Damage Rolls**

So far, all the examples and explanations I’ve provided have been about the d20, but what if we took the idea of high-variance and applied it to damage dice instead? Functionally, the dice would work the same as the altered d20. There would be a higher emphasis on the lower and higher values on the dice, making the rolls swing in either direction more often. In the context of damage rolls, this would lead to successful attacks hitting either very low or very high more often. Let’s look at what the different dice involved in an attack are representing, and what this means for high-variance in this context. 

When you, as a player or a DM, decide to make an attack with a character, what’s the process? Well, first you have to be in range of whatever you’re planning to attack, either you’re already within range, or you move into range (this is also assuming there are no negative status effects that would prevent you from attacking, like blindness or paralysis). Okay, so now that you’re in range, you initiate the attack action against the target, probably declaring what you’re attacking and what you’re attacking with, then rolling a d20 to see if you hit. So, this d20 roll is effectively your character taking the action, and the result will determine success or failure if it matches up against the defender’s AC. 

You can almost imagine these actions as a series of events in the game-world: 
1. Your character swings at the defending target (this is the declared action).
2. You then either hit or miss the target (this is the result vs. the target’s AC). 
3. Assuming you hit, then the damage roll that follows would determine the damage that’s been caused as a result of the attack. This roll can be treated as the effectiveness of the hit; how well you hit the target. 

If you consider the damage rolls in this way, and look at the different pieces of this process through a narrative lens, then the value of high-variance dice in damage rolls becomes more distinct than that of any d20 rolls. Your character’s effectiveness regarding their attacks become more consistent, on both ends, rather than determining absolute success or failure. This better highlights your character’s strengths and weaknesses.

High variance dice in these scenarios may lead to some more exciting hits and also not-so exciting ones, however the high-variance dice maintain the average in the number array, and with damage dice, unlike the d20 rolls, the other important elements of your character (attribute bonuses, proficiency bonus, traits, ect.) can carry the lower ends of the dice rolls much more effectively than the d20; rolling a 1-6 on a high-variance d20 with an overall modifier of 4 or 5 doesn’t make as big of a difference in the final result as rolling a 1 or 2 on a d6 with the same modifier. This allows the character’s “build” to have a higher impact on how damage is dealt compared to the d20 interactions, while also bringing out the flavor intended with the high-variance dice. Also notice that rolls involving the d20 often determine the absolute success or failure of an action, whereas the damage roll is always a success but determines the extent of your success.

![Graph showing the results of both Standard and High-Variance D6.](d6-results.png "D6 Standard vs. High-Variance Results")

If you compare the above graph, which plots the standard and weighted results of a d6, to the graph shown earlier with the d20, you’ll notice that the only increase in probability are the 1 and 6 ends of the array, and the only removed values are 3 and 4. The average value output remains 3.5 (the overall average damage output remains the same) however, as your character develops and gains more “skill” with increases in attribute bonuses, traits, feats, better weapons, ect., the lower rolls become less of an “oh-crap” moment than on a d20. This makes the damage rolls a tad more exciting without changing the average damage output of your character, making each individual roll feel more significant. 

Combat is a very interesting medium in which we can discuss these dice. In the future, I’d like to dive deeper into these ideas and simulate some combat scenarios with actual characters and NPCs to better compare and understand the results of damage and attack dice both weighted and standard. The numbers so far appear to lead to the conclusion that an overall increase in average damage will only happen with a high-variance d20 for attack rolls, since your chances of landing a critical hit are increased from 5% to 15% and a critical hit is the only way to increase the average damage-roll value. High-variance dice, when rolled for damage, do not increase your average damage output, but can make your hits more exciting and may better simulate the intended “effectiveness” of a hit; however, it does appear that the d20 rolls, which majority work in a success-or-fail system, may not be as exciting (at least in this context), and can easily dilute the dice system into mere coin flips, rendering your character’s unique skills and attributes somewhat useless. 

### Narrative - Checks
Now that combat is out of the way, let’s talk about what impacts a high-variance dice system may have on narrative, and how RP (role-playing) encounters (checks) might be affected by them. Like the attack-roll and saving-throw in 5e, the check is rolled on a d20 and the result of the roll is added to the character’s ability score associated with the roll, any proficiency bonus, and any additional interactions from the character’s traits or feats, and the result makes the determination of a success or failure. 

However, a slight difference here is that the extent to which the failure or success is had can be determined by the DM. For example: A player enters a room looking for evidence of a crime. The DM then asks the player to roll an Investigation check to determine the success of the investigation. The DM may have an arbitrary DC for the player to meet or exceed, or they may determine that after the roll, or they may already determine the outcome, regardless of the player’s roll (this kind of dives into DM technique and philosophy, which I’ll talk about at some point in the future, just not here). 

The DM may also have several DC requirements laid out which would determine the extent to which the player has investigated. So if the player rolls a 10 on their check, the DM may tell the player they found clues, but also withhold other clues that the player would have found had they rolled higher. In most checks, the success and failure of the attempted action can vary, unlike attack-rolls and saving-throws, which can allow for more interesting uses of high-variance dice.

The check itself is always rolled by the character initiating the action, however most checks have a DC determined arbitrarily by the DM, and usually occur after the player in question tells the DM they want to complete a complex role-playing action (e.g. climb a wall, or search a room, or examine a statue to determine its historical significance). Or, they may ask the DM “Can I tell what this symbol means?”, and the DM would then in turn request a check from the player to make that determination.

**The DM**

Context is king when it comes to checks, and the DM has the last word on this. Giving the DM the option of changing the probability of a roll in certain RP scenarios allows for more substantial narrative “tools” for the DM; a high-stakes RP interaction between the characters and an NPC, or as Wyrmwood laid out, an all-or-nothing, last-ditch effort by a PC. These contexts can be exciting, and the DM having the ability to raise the stakes even further by increasing the probability of both success and failure for the PCs is a good narrative tool. Personally, I would use this sparingly. As a DM, I don’t want to dilute these exciting moments. I’d like to keep them few and far between, and when they do happen, they’ll be even more exciting. This seems to be one of the best areas of the game to use the high-variance dice. In my humble opinion, of course. 

This needs to be considered with the previous figures of the high-variance dice in DC scenarios. As a DM, considering that these dice will tend towards a 50% success rate should be in the back of your head, remember this when considering their use in a narrative encounter.

**The Player**

As a DM, one of my highest priorities is ensuring that my player’s have the freedom to create and realize their characters as they see fit. I use caution when restricting character elements from a campaign as a part of the world-building process. It’s a challenge sometimes to maintain a balance, however I want the players to create characters they enjoy and care about over long-form campaigns. 

This idea can be applied as well to the choice of a player using high-variance dice in both a long-term and short term context. A chaotic Sorcerer character who tends towards extremes, or a Warlock whose patron is chaotic, may be  examples of a PC using high-variance dice throughout a campaign. A short-term context may be a character in a RP situation where they may be acting erratically and has a higher propensity to fail or succeed, or a character overwhelmed with emotion, attempting to complete a check. 

These situations are all examples in which a PC may want to use high-variance dice at their discretion. It’s all context, and just having this option available may make these situations more exciting for your play-group.

### Design
So, lastly, I’d like to talk about using these dice as a part of campaign design. This would be an aspect of the game that DMs would be involved with, rather than players, and what excites me the most about high-variance dice. In Wyrmwood’s Kickstarter, one of the examples given for use of the dice was:
> “Venturing into the wrong forest, you’ve caught the attention of a devious fae looking for amusement. An enchantment on one or more of your party means all of your ventures are doomed to result in either wonderful fortune or horrifying failure, but hardly anything in between. Will you get back to normal when you leave the forest? Will you want to?”

For me, this was the example that stood out the most. My brain flooded with ideas in which I, as a DM, could utilize these dice as a part of my game-design. Or, perhaps these dice may be staples for certain NPCs as an element of their own personality or behavior, or maybe when designing new items or enemies these dice can be used to exaggerate the flavor of their design. This is where I feel these dice shine the most. Allowing the DM to fine-tune certain game functions through the dice is exciting. This adds another layer of customization to any campaign for the DM. 

### Conclusion
This was an interesting experiment. Should you use these dice? That’s your choice. Always discuss these things with your play-group, and find what makes your sessions more fun. At the end of the day, the only thing that matters in d&d, is that you and your friends have a good time. With that being said, I think in writing this piece, I’ve come to my own conclusion that these dice definitely have a home in the game, and seem to function at their best in narrative heavy situations, rather than mechanically heavy ones, and should be used sparingly at the DM’s and player’s discretion. 

I’ll probably revisit these ideas again in the future and drill down on some more in-depth simulations of these dice in combat encounters, as there are still some questions left here. How much does the high-variance impact combat rolls? Do these effects change with character level? If so, then how much? I’ll keep these in my back pocket for now. 
