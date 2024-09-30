---
title: "Interrupt or not?"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/no-fear.jpg
---

I'm energized! I just returned from the TOCICO 2023 Innovation Summit
in Fort Lauderdale, Florida. Wow! What a great conference. Super
presentations and lots of time to talk with colleagues in a great
venue.

Now that I'm back to work, I'm catching up on the group chat. What did
I miss while I was gone?

Scanning the group chat, I see that one of the discussions is whether
to read a certain status signal by polling the register that contains
it, or to have a change in the status trigger an interrupt that then
causes the processor to read the value of the register containing a
bit representing the status.

This is embedded software 101, right? The discussion in the chat was
such that people were thinking reading the status via an interruypt
was the way to go. As usual, polling felt like the low-tech way to go
and would result in the processor doing needless work much of the
time.

That's all true, but unless we need the cycles that would be used by
polling they are free. If they aren't used for polling, they will just
be wasted in a spin loop somewhere else, right?

Something about the discssion bugged me, so I thought about it a bit.
And on the plane ride home last night (seven hours, ack!) I had been
listening to "Beyond the Goal," which is an audiobook where Eli
Goldratt is sharing his insights.

One of those insights is the importance of translating local
measurements into global measurements, so you can make decisions that
actually move the needle for the company, as against just making your
life better.

An example of a global measure is throughput (T), which is the rate at
which the company makes money through sales. In a practical sense, to
help the company, we should always be looking at how to increase T.

So how does this relate to the design / coding discussion in the
chat?

Here's the point: From the local-optima point of view, reading the
status signal by polling versus by taking an interrupt seemed like a
push. One is not much harder than the other.

But from a global optima point of view, the two alternatives may
have different impacts.

Here's how this goes: "Which system is easier to debug? One that
is not very dynamic or one which is highly dynamic?"

A system that is "not very dynamic" might be a system that has a
superloop architecture and generally run the same set of routines
on every loop iteration. Of course there are small differences, but
in general, the code does more or less the same thing all the time.

In other words, it's not very dynamic.

Now, imagine a system that prefers interrupts over polling. It's going
to be much more dynamic. You'll have opportunities for all kinds of
problems that "can't happen" under a simple polling approach.



