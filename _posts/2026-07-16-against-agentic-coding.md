---
title:  "Against Agentic Coding, For Collaborative Coding"
layout: post
categories: media
---

## The Problem

Agentic coding is bad for 3 reasons:
1. You lose focus
2. You lose understanding of the code
3. The AI subtly introduces bugs and conceptual errors

### 1: Focus

Letting the AI write the code for you (as opposed to typing it yourself) causes you to context switch out.
So you say “implement this feature for me…” bla bla bla, you go into plan mode and whatever.

Then you switch to youtube and start watching shorts.
- So you’re context switched out. and you’re not thinking about the problem
- Whereas typing the code forces you to go slow and gives you time to think about the problem subconsciously
- The act of typing is also stimulating and fun (especially in vim), so you don’t get bored

Constantly thrashing your context like this makes you really tired, and it gets unengaging fast.

### 2: Understanding
- You're not paying attention, but the code is changing
- So by construction you're losing understanding
- You also don't learn as much because you aren't really thinking about the implementation as it happens

### 3: Errors
All of this combines to make it a lot more difficult to verify the code that the AI generates.
You might go into it with a P=NP idea where you say "it’s easier to verify code than to come up with it"
- but that doesn’t take into consideration that the AI is persuading you that the solution is correct
- and the hard part about verification is the conceptual and architectural considerations
- if you haven’t been thinking about that (while you’re typing), its very hard to falsify the AI code

## So what's the solution?
The solution is to use the chat.com or claude.ai website, and manually type the code that the AI produces.
- This keeps you stimulated and in the loop

Furthermore, this is more of a collaborative relationship. You and the ai work together.
It provides the domain knowledge, you provide the critical thinking.

Fundamentally the chatbot interface is collaborative.
The interaction is literally modelled like a text conversation in discord or slack or whatever.

Conversely, in agentic coding there isn’t much interaction.
There isn't a clear point where you can step in and help the AI.
You have to wait till it’s done the whole thing to read it.
This makes it harder to verify correctness, and you also just wasted a lot of compute if it spent time working on a wrong spec.

The strength of AI are it's crazy wide domain knowledge. The weakness is reasoning.
The strength of human intelligence is reasoning.
By working together, you can produce better code and a more complete model of the problem.

## PS: What about "vibe coding"?

There's this idea in silicon valley that it's okay to not understand the code.
And that it's okay to write bugs, as long as the AI is fixes the bugs fast enough.

I think the anthropic compiler experiment showed that this isn't true.
Eventually the agents will get to a point where they can't make any progress because everytime they "fix" something, they break something else.
The problem here is that they fundamentally modelled the problem wrong.

## In my opinion, programming is not about producing code, but rather understanding problems and creating correct models.
