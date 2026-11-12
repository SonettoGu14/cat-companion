# Branching narrative

"Branching" is not one structure. It is a family of four, and they differ by a factor of
ten in what they cost to build.

```
linear       A ── B ── C ── D                    everyone sees everything

foldback     A ── B ── C ── D                    branch, then rejoin
                   └── B' ──┘

tree         A ── B ── C                         paths never meet again
                   └── D ── E

web          scenes + state                      same scenes, different meaning
```

## What each one costs

**Linear** is free and gets a bad reputation it does not deserve. If the story is strong, no
choice is needed; *Portal* is linear and nobody calls it a bad story.

**Foldback** is the standard commercial answer. The player branches, feels the choice, and the
paths rejoin before the content budget explodes. *Uncharted* and most AAA games are this:
choices change the tone and the next ten minutes, then the main line resumes.

**Tree** is the honest version of choice, and it is expensive in a way that is easy to
underestimate. Two choices at each of three decision points is eight paths. Your writing
budget gets divided by the number of paths, and the player only ever sees one of them — so
each path is a tenth as good as the linear version would have been.

**Web** gives up on separate scenes and puts the variation in *state* instead. The scenes are
the same; what the player knows, can say, or notices is different. *Disco Elysium* is the
extreme case: your skills change the sentences available to you and even what the world shows
you, while the plot's skeleton stays stable.

## The technique that buys the most

**Delayed consequence.** Branch cheaply now, pay off later in one place. A line of dialogue in
chapter one changes one scene in chapter four. You wrote one extra scene, not a parallel act.

**Hub and loop.** *Hades* runs the same dungeon structure every time; the story advances in
the space between runs. The repetition is the system, the narrative is the wrapper, and the
wrapper is cheap.

## Pick the cheapest structure that carries the theme

Ask one question: **what does the player have to decide for this story to land?**

If the answer is "nothing, they need to witness it", write linear and spend the budget on the
scenes. If it is "whether to trust her", you need one real fork — not twelve.

Two rules that save projects:

1. **Write the state table before the scenes.** What can the player know at this point? What
   changes if they know it? If you cannot write the table, you do not have a branching story,
   you have a wish.
2. **Never promise consequences you cannot afford.** *Mass Effect 3*'s ending was not rejected
   because it was a bad structure; it was rejected because the trilogy had spent a hundred
   hours teaching players that choices mattered, and the last one did not.

## Check yourself

??? question "Three binary decisions. How many endings does a full tree produce, and what does that do to your budget?"
    Eight. Each ending gets roughly one eighth of the writing, art and testing time the linear
    version would have had — which is why most shipped games fold back instead.

??? question "Why is a hub-and-loop structure cheap for narrative?"
    Because the expensive part (the loop) is a system that is built once and reused, and the
    story only has to author what changes between runs.

??? question "Your design promises that choices matter but you can only afford a foldback. What is the honest fix?"
    Make the choices change something visible and local — tone, a relationship, one scene —
    and let the main line rejoin. A promise kept small beats a promise broken large.
