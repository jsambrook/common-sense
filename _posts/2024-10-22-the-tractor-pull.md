---
title: "The Tractor Pull"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/tractor-pull-2.jpg
---

![Tractor Pull](/assets/img/tractor-pull-2.jpg)

<!-- [Audio Discussion](https://common-sense.com/assets/files/tractor-pull.mp3) -->

Sarah stops by Marco's office to talk about a recent topic they
discussed in a team meeting. Sarah wants to know more about what
Marco has planned for the team.

Marco is happy to oblige and discusses one of the purposes of their
team meetings. They have a good discussion in which Marco uses the
metaphor of a county fair tractor pull.

The metaphor makes sense to Sarah and she leaves the meeting with
Marco with a task that interests her and better prepared to help her
team.

Links to additional resources are at the end of this article.

Please note that all people and companies mentioned in this article
are fictional.

 ***

The Tractor Pull

Sarah knocked on Marco's office door, finding him hunched over his
standing desk, scope probe in hand and probing a development board of
some kind. He looked up with a welcoming smile. "Hey Sarah, come on
in. Just making some timing measurements on the new temporal assert
macro we discussed after the meeting yesterday. With it, we will be
able to use assertion checking to guarantee that we meet various
timing constraints in our code."

"Your timing is perfect, actually," Sarah said with a grin, settling
into one of the chairs across from his desk. "Yesterday's discussion
about assertion checking got me thinking. The technical approach makes
sense, especially for a Class III device, but I'm curious about the
bigger picture. What made you focus on this now?"

Marco lowered his desk and took a seat, clearly pleased by the
question. "You know, that's exactly what I was hoping someone would
ask. You've probably noticed I've been pushing for more structured
approaches lately - not just with assertions, but with our whole
development process."

Sarah nodded. "Yeah, like the new code review checklist and the
automated validation scripts you added last month."

"Exactly," Marco replied, absently adjusting his monitor. "What I'm
really trying to do is improve our flow - the rate at which we deliver
quality work. And by quality, I mean something that will stand up to
both FDA scrutiny and real-world use."

"Flow of quality work?" Sarah leaned forward, interested. "Is this
related to what you mentioned about process capability in the stand-up
last week?"

"You caught that, huh?" Marco grinned. "Yes, it's all connected. I'm
trying to understand and control the sources of variation in our daily
work. It's not just about meeting deadlines - it's about making our
work more predictable and consistently high-quality. Both throughput
and quality matter, especially with the kind of software we're
developing."

Sarah tapped her pen thoughtfully against her notebook. "And
controlling variation helps with that? I mean, I can see how it would
help with testing, but..."

"Think about it this way," Marco said, then paused. "Actually - random
question: have you ever been to a tractor pull?"

Sarah's face lit up with genuine amusement. "Okay, wasn't expecting
that pivot. But yeah, actually. Used to go to them when I was
younger. The noise, the excitement..." She gestured
expansively. "Watching those machines strain against increasingly
heavy loads. It was quite a spectacle."

"Perfect example!" Marco said enthusiastically. "Think about what
happens during a pull. The tractor starts out strong, moving easily
down the track. But as it moves forward, the weight shifts forward on
the sled, making it harder and harder to pull. Eventually, most of the
time, the load becomes too much. The engine's roaring, smoke
everywhere, but the tractor's just spinning its wheels."

"Making a lot of noise but not making progress," Sarah added, starting
to see where this was going. "Like when we hit that wall with the
sensor integration last quarter..."

"Exactly," Marco nodded. "And that's what I want to avoid on the
Denali project. We don't want our work to become progressively harder
as we go along, until we're working flat out but barely moving
forward."

He moved to the whiteboard. "Mind if I sketch something? I want your
take on this."

Marco quickly drew a graph showing team workload over project
progress, with a clear upward curve in the latter half:

![Usual Case](/assets/img/workload-single.png)

"This is what I've seen happen too often in our industry. We start at
a manageable workload - call it 1x - and it stays fairly steady
through the first half. But then..."

Sarah studied the graph, her expression thoughtful. "The technical
debt starts coming due?"

"Among other things," Marco agreed. "What else do you think
contributes to that curve?"

"Well..." Sarah considered, "There's the obvious stuff - all those
TODOs we leave in the code have to be addressed before we can
ship. Can't exactly explain those away during an audit." She smiled
wryly. "But it's more than that. We start getting hardware prototypes
that have their own issues. We need to build out testing
infrastructure. And honestly? People get burnt out. Especially if
they're constantly fighting fires."

"Good observations," Marco said, picking up a green marker. He added a
second, flatter line to the graph. It now looked like this:

![Desired Case](/assets/img/workload-comparison.png)

"This is what I'm aiming for on Denali. See the difference?"

Sarah's eyes narrowed as she studied the new line. "A controlled,
consistent workload... Is that why you've been so insistent about
documentation and test coverage from day one?"

"Part of it," Marco confirmed. "The assertion checking strategy, clear
documentation requirements, automated testing - they're all meant to
help us catch issues early and prevent technical debt from
accumulating. It's like having a lighter sled to pull."

"But how do we know if it's working?" Sarah asked. "I mean, beyond
just feeling less overwhelmed?"

Marco set down the marker. "That's where metrics come in. We track
things like our defect discovery rate, implementation time for new
features, stability of our test cycles. If these start trending up
over time..."

"We're pulling too heavy a load," Sarah finished. "And I suppose our
assertion failures during testing could be another indicator -
especially if we start seeing them in areas we thought were stable."

"You're getting it," Marco said, clearly pleased. "You know, Sarah,
you seem to have a really good grasp on this. Would you be interested
in leading a discussion about this with the team? Maybe at next week's
tech sync? I think you could help everyone understand how these
technical practices connect to our broader goals."

Sarah straightened, surprised but intrigued. "Really? I mean, yes, I'd
love to. It would be a great opportunity to dig deeper into these
concepts. Maybe we could use some real examples from our current
work?"

"Absolutely," Marco agreed. "Take some time to think about how you
want to present it. The tractor pull analogy seems to resonate - maybe
start there and then bring in specific examples from our project?"

"I'll start putting some thoughts together," Sarah said, standing
up. "Thanks, Marco. This really helps explain the method behind what
sometimes feels like madness," she added with a small laugh.

As Sarah left his office, Marco felt a sense of satisfaction. He knew
Sarah would do an excellent job sharing this perspective with the
team. More importantly, she'd understood that good software
engineering wasn't just about writing code - it was about building
sustainable processes that could stand up to the rigorous demands of
medical device development.

 ***

Key Takeaways:

Monitor Your Pull Weight: Like a tractor pull, software projects often
start easily but can become progressively harder to move
forward. Watch for signs that your team is "pulling a heavier load" -
such as increasing defect rates or longer implementation times for
similar features.

Control Process Variation: Predictable processes are manageable
processes. Implementing consistent practices (like assertion checking
and automated validation) from the start helps maintain steady
progress throughout the project lifecycle.

Prevent Load Accumulation: Technical debt, deferred decisions, and
incomplete documentation create a cumulative drag on
productivity. Address issues early rather than letting them accumulate
for the latter half of the project.

Measure What Matters: Track meaningful indicators of project health:

Rate of new issue discovery
Implementation time for features
Stability of testing cycles
Assertion failures in previously stable areas

Build Sustainable Practices: Good software engineering isn't just
about writing code - it's about creating processes that can
consistently deliver quality while standing up to regulatory scrutiny.

***

Additional Resources

## Additional Resources

1. **"The Goal for Healthcare"** by Boaz Ronen, Joseph S. Pliskin, and Shimeon Pass (2006)
   - While focused on healthcare delivery, this book applies Theory of
     Constraints principles to regulated environments and provides
     valuable insights for medical device development teams.

2. **"Accelerate: The Science of Lean Software and DevOps"** by Nicole Forsgren PhD, Jez Humble, and Gene Kim (2018)
   - Particularly relevant are chapters 4 and 11, which discuss
     technical practices that help maintain consistent delivery pace
     and quality in software development.

3. **"Good Practices for Computerized Systems in Regulated GxP Environments"** - PIC/S Guide PI 011-3
   - This regulatory guidance document provides context for how modern
     software development practices can be implemented while
     maintaining compliance in regulated environments.

4. **"The Phoenix Project: A Novel about IT, DevOps, and Helping Your Business Win"** by Gene Kim, Kevin Behr, and George Spafford (2013)
   - While not specific to medical devices, this business novel
     effectively illustrates how unmanaged work-in-progress leads to
     increasing organizational drag - similar to our tractor pull
     metaphor.

5. **"Technical Debt in Practice: How to Find It and Fix It"** by Neil Ernst, Ipek Ozkaya, and Robert Nord (2020)
   - Provides practical approaches to identifying and managing
     technical debt, with specific attention to regulated environments
     where quality requirements are strict.

