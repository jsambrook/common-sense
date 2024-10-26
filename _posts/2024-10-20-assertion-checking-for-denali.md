---
title: "Assertion Checking for Denali"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/marcos-team-whiteboarding.jpg
---

![Marco's Team Whiteboarding](/assets/img/marcos-team-whiteboarding.jpg "Marco's team whiteboarding")

[Audio Discussion](https://common-sense.com/assets/files/assertion-checking-for-denali.mp3)

Marco convenes his software team to discuss the assertion checking
strategy they will use on Denali, the new high-end infusion pump that
MedTech is designing.

The team dives into the details and improves the design as they
discuss it. It's a good example of the kind of shared learning that is
possible when a team has good leadership and a friendly but not weak
vibe.

Links to additional resources are at the end of this article.

Please note that all people and companies mentioned in this article
are fictional.

 ***
Assertion Checking For Denali

The Jupiter conference room at MedTech was abuzz with activity as
Marco's software team gathered for their weekly meeting. Today's
agenda: discussing the assertion checking strategy for their new
project, Denali. The team was eager to build upon the success of their
previous project, Rainier, and ensure that Denali would be just as
robust and reliable.

Marco opened the meeting with a smile. "Alright, team. Today we're
going to dive deep into our assertion checking strategy for Denali. I
know we've all worked with assertions before, but I want to make sure
we're all on the same page. Who wants to kick us off with a quick
refresher on what assertion checking is and why we use it?"

Sarah, a senior developer known and respected for her careful work
habits, raised her hand. "I can start us off, Marco. Assertion
checking is a programming technique we use to catch bugs early in the
development process. We insert assertions into our code to express
conditions that we expect to be true at certain points in the
program. If an assertion fails, it means our program has entered an
unexpected state, which helps us identify and fix issues quickly."

Marco nodded. "Excellent summary, Sarah. Can anyone add to that? Maybe
give us some specific benefits of using assertions?"

Tom, a junior developer who had recently joined the team, hesitantly
spoke up. "Well, from what I've learned, assertions can help make our
code self-documenting. They clarify our intentions and expected
behavior, right?"

"Absolutely, Tom," Marco agreed. "That's a great point. Assertions do
indeed serve as a form of executable documentation. Anyone else?"

Rachel, the team's QA specialist, chimed in. "They're also incredibly
helpful for debugging. When an assertion fails, it provides immediate
feedback on where and why an error occurred. It's like having a
built-in watchdog for our code."

Marco beamed at his team's enthusiasm. "You're all spot on. Now, let's
talk about how we're going to implement our assertion strategy in
Denali. We'll be following a similar approach to what we used in
Rainier, but I want to make sure everyone understands the nuances. Who
wants to walk us through the basic structure of an assertion in our
codebase?"

Lisa, a mid-level developer with a knack for clear explanations,
volunteered. "Sure, I can do that. In our codebase, we typically use a
macro called `hal_assert`. It takes a boolean condition as an
argument, and if that condition is false, it triggers an assertion
failure. Here's a simple example:

```c
void spi_init(uint8_t device_id) {
    hal_assert(device_id < MAX_SPI_DEVICES);
    // Rest of the initialization code...
}
```

In this case, we're asserting that the `device_id` is valid before we
try to use it."

Marco nodded. "Great example, Lisa. Now, let's discuss when and where
we should use assertions in our code. Any thoughts?"

Tom raised his hand again, looking a bit uncertain. "I've always been
told that assertions are for catching programmer errors, not for error
handling in production code. But I'm a bit confused about the
difference. Can someone clarify?"

Sarah jumped in to answer. "That's a great question, Tom. You're right
that assertions aren't for handling errors that can occur during
normal use of our software. Instead, they're for confirming that our
design assumptions aren't violated while the software is running. For
example, we might use an assertion to check that a function argument
is within an expected range, but we'd use proper error handling for
issues that could occur due to user input or external factors."

Rachel added, "And it's important to note that while many developers
turn off assertions in production builds, we keep them enabled. Can
you explain why we do that, Marco?"

Marco nodded, grateful for the segue. "Absolutely, Rachel. This is a
crucial point that I want everyone to understand. We keep assertions
enabled in our production builds for several reasons:

1. Safety: In medical devices like ours, a single bug can have serious
   consequences. Assertions provide an additional layer of safety.

2. Hardware error detection: Assertions can catch not just software
   bugs, but also hardware errors. Even if our code is perfect,
   hardware issues can cause unexpected behavior.

3. Immediate feedback: If an assertion fails in the field, it gives us
   valuable information about what went wrong and where."

Lisa looked concerned. "But Marco, what about the performance impact?
Won't running all these checks slow down our device?"

Marco smiled, appreciating the valid concern. "Good question,
Lisa. The performance impact is usually minimal, especially if we're
thoughtful about where we place our assertions. We generally avoid
putting them in tight, performance-sensitive loops. Plus, if we find
that assertion checking is too burdensome on our current hardware,
it's a sign that we need more computing power anyway – which is
valuable information for future planning."

Tom raised another question. "What happens if an assertion does fail
in a deployed device? Won't that cause the device to crash in front of
a patient?"

Marco nodded, his expression serious. "That's a critical question,
Tom. Given that Denali is an infusion pump, we need to handle
assertion failures very carefully. Let me walk you through our
strategy."

He stood up and moved to the whiteboard. "When an assertion fails, we
can't assume much about the system state. The failure could be due to
anything from a simple logic error to memory corruption. So, our
approach is this:

1. Immediate action: We immediately stop any ongoing infusion to
   ensure patient safety.
2. Minimal logging: We save essential information about the assertion
   failure in a predefined section of DRAM that won't be lost on
   reboot. This includes the location of the failed assertion and the
   processor register values at the time of failure.
3. Reboot: We then initiate a system reboot to return to a known, safe
   state.
4. Post-reboot processing: After rebooting, we complete the logging
   process, writing the saved information to flash memory.
5. Alarm activation: Most importantly, we activate alarms to alert
   healthcare professionals immediately."

Sarah interjected, "But Marco, wouldn't stopping the infusion be
dangerous for some patients who need continuous medication?"

Marco appreciated the concern. "Excellent point, Sarah. You're right,
and that's why we must work closely with the folks in system
engineering.  In some cases, the pump might enter a 'fail-safe' mode
where it continues a minimal, conservatively safe infusion rate while
alerting staff. The specific behavior depends on the medication and
the potential risks involved."

Rachel added, "This is why our alarm system is so critical. We can't
just have the pump stop and wait for someone to notice that it's
offline. We need to ensure that the situation gets immediate
attention."

"Exactly," Marco agreed. "Now, let's talk about how we identify where
an assertion failed. Lisa, you mentioned informative assertion
messages earlier. Can you expand on how we pinpoint the location of a
failed assertion?"

Lisa nodded. "Sure, Marco. In our current system, we use file name and
line number to identify assertion locations. For example:

```c
#define hal_assert(condition, message) \
    ((condition) ? (void)0 : _hal_assert_fail(#condition, __FILE__, __LINE__, message))
```

This approach is straightforward and directly ties to our source
code."

Tom looked thoughtful. "But doesn't that change every time we modify
the code? Couldn't that make it hard to track issues across different
versions?"

Marco smiled, pleased with Tom's insight. "Great observation,
Tom. That's one of the challenges with the file and line number
approach. Does anyone know of an alternative method?"

Sarah spoke up. "I've heard of systems using unique integer codes for
each assertion. Each assert gets its own 32-bit unsigned integer, and
these codes are never reused, even as the code evolves."

"Exactly," Marco confirmed. "This approach has some advantages. Who
can think of some?"

Rachel raised her hand. "Well, for one, it would make it easier to
track issues across different versions of the software. The codes
wouldn't change even if we refactor the code."

Lisa added, "And it could potentially use less memory than storing
full file names and line numbers, right?"

Marco nodded. "Both excellent points. Here's an example of how we
might implement this:

```c
#define hal_assert(condition, code, message) \
    ((condition) ? (void)0 : _hal_assert_fail(#condition, code, message))

// Usage
hal_assert(rate > 0 && rate <= MAX_SAFE_RATE, 0x0000A001, "Rate must be positive and within safe limits");
```

This approach does require us to manage these codes carefully,
ensuring they're unique and well-documented."

Tom looked concerned. "But isn't it harder for developers to quickly
identify where an assert failed without the file and line number?"

Sarah jumped in. "It's easy to search for these codes in the IDE,
or we could have a simple script that used `grep` or `ripgrep` on
the code to find them. I don't think this would be a big problem."

Marco nodded approvingly. "This has been a good discussion. For
Denali, we're going to use a hybrid approach. We'll include both the
unique code and the file and line information:

```c
#define hal_assert(condition, code, message) \
    ((condition) ? (void)0 : _hal_assert_fail(#condition, code, __FILE__, __LINE__, message))
```

This gives us the best of both worlds – stable codes for tracking
across versions, and easy reference for developers."

The team nodded in agreement, seeing the value in this comprehensive
approach.

As the team seemed satisfied with the technical aspects of the
assertion strategy, Marco raised his hand to draw attention to one
final, crucial point.

"Before we wrap up, there's something important I want to emphasize,"
Marco said, his tone serious. "For this assertion strategy to be
effective, it's absolutely critical that we maintain our commitment to
thorough testing."

Rachel, the QA specialist, nodded vigorously. "Absolutely. Our goal
should be to catch every possible assertion failure during our testing
phase, right?"

"Exactly, Rachel," Marco confirmed. "At MedTech, we already have a
robust testing process, but I want everyone to understand why this is
so crucial, especially with our assertion strategy."

Tom looked a bit confused. "But if we're handling assertion failures
gracefully in production, why is it so important to catch them all in
testing?"

Sarah jumped in to explain. "Think about it this way, Tom. Every
assertion in our code represents an assumption about how our system
should behave. If an assertion fails, it means one of our fundamental
assumptions was wrong."

Marco nodded in aggreement. "Well put, Sarah. In an ideal world,
assertion failures should almost never happen in the field. If they
do, it means something significant and unexpected has occurred."

Lisa's eyes widened with understanding. "So if we see an assertion
failure in a deployed device..."

"Exactly," Marco finished for her. "It should be treated as a major
event. In fact, it would likely trigger a new CAPA action for the
company."

Seeing some puzzled looks, Marco explained further. "CAPA stands for
Corrective And Preventive Action. It's a system we use to investigate
and correct quality issues, and to prevent them from recurring. An
assertion failure in the field would definitely qualify as something
requiring this level of attention."

Rachel added, "This is why our testing needs to be so
comprehensive. We're not just testing for expected behaviors, but also
trying to uncover any scenario that could trigger an assertion
failure."

"Precisely," Marco agreed. "Our assertion strategy is like an
additional safety net, but our goal is to never need that net. We want
to catch everything during our testing phase."

Tom nodded slowly, processing this information. "So when we're writing
our tests, we should be thinking not just about normal use cases, but
also about edge cases that might trigger assertions?"

"Absolutely, Tom," Marco said encouragingly. "We need to think
creatively about what could go wrong. What if a sensor fails? What if
there's a power spike? What if two usually independent systems
interact in an unexpected way?"

Sarah chimed in, "And this is where our unique assertion codes will be
really helpful. If we do encounter an assertion failure during
testing, we can quickly identify exactly which assumption was
violated."

"That is exactly right, Sarah," Marco agreed. "This approach allows us
to continuously improve our software. Every assertion failure we catch
in testing is a potential critical issue we've prevented from reaching
a patient."

Lisa looked thoughtful. "This really emphasizes how important our work
is. Each assertion we write, each test we design, could literally be
life-saving."

Marco nodded solemnly. "That's exactly right, Lisa. We're not just
writing code; we're building a system that patients and healthcare
providers will rely on. Our assertion strategy, combined with rigorous
testing, helps ensure we're worthy of that trust."

The room fell silent for a moment as the team reflected on the weight
of their responsibility.

Rachel broke the silence, a look of curiosity on her face. "Marco,
I've been wondering. With all our structured testing, are there any
other approaches we're using to really push our system to its limits?"

Marco smiled.  "I'm glad you asked, Rachel. In fact, there's another
crucial component to our testing strategy that I wanted to discuss. We
call it our 'random' or 'thousand monkeys' test."

Tom couldn't suppress a chuckle. "'Thousand monkeys'? What does that
mean?"

Marco smiled. "It's a bit of a playful name, Tom, but it's a serious
and incredibly useful tool. Both our Rainier device and the new Denali
support a network connection that allows for remote, scripted button
pushing. The random test uses this to essentially push buttons at
random."

Lisa looked intrigued. "So it's like having a user who doesn't know
how to use the device just pressing buttons randomly?"

"Exactly," Marco confirmed. "It might look crazy if you watch it in
action, but this random testing has a knack for finding edge cases
that our structured tests might miss. It walks down various pathways
in the code and pushes buttons in contexts we might never have thought
to test."

Sarah nodded in understanding. "Because no one would expect a certain
button to be pushed in that particular context, or the system isn't
prepared for it."

"Precisely," Marco said. "These tests often trigger assertion failures
because they uncover scenarios we didn't anticipate. And that's
exactly what we want to find during testing, not in the field."

Rachel looked impressed. "How long do we run these random tests?"

Marco's expression turned serious. "Before we release any firmware for
production use, the Denali systems will have to run for hundreds of
hours of random testing without a single crash or assertion failure."

Tom's eyes widened. "Hundreds of hours? That's a lot of testing!"

"It is," Marco agreed. "But remember, we're dealing with a medical
device that people's lives will depend on. We can't afford to cut
corners. And of course, we might have ten such systems running the
test in parallel, so the actual time to complete the testing won't
be too bad."

Lisa nodded thoughtfully. "So between our comprehensive structured
tests and these extensive random tests, we're really covering all our
bases."

"That's the goal," Marco confirmed. "The structured tests ensure we're
meeting all our known requirements and edge cases. The random tests
help us uncover the unknown unknowns – those scenarios we couldn't
have anticipated."

Sarah added, "And all of this testing helps ensure that our assertions
are doing their job correctly, without being too restrictive or too
permissive."

"Yep, that's right, Sarah," Marco said with a nod. "It's all part of
our commitment to creating the safest, most reliable infusion pump
possible. Our assertion strategy, combined with both structured and
random testing, forms a robust framework for ensuring the quality and
safety of Denali."

As the meeting wrapped up, there was a palpable sense of purpose among
the team members. They left the Jupiter conference room not just with
a clear understanding of their assertion checking strategy, but with a
renewed commitment to the quality and safety of their work. The Denali
project was more than just another assignment; it was a chance to make
a real difference in patient care, and they were determined to get it
right.

The team dispersed, each member mentally preparing for the challenges
ahead. They knew that implementing this comprehensive assertion and
testing strategy would be demanding, but they also understood the
critical importance of their work. As they returned to their desks,
there was a shared sense of pride and responsibility. They were not
just software engineers; they were guardians of patient safety, and
they were ready to rise to the challenge.

 ***

Additional Resources

1. [Using Asserts in Embedded Systems](https://interrupt.memfault.com/blog/asserts-in-embedded-systems)

2. [A Nail for a Fuse](https://www.state-machine.com/a-nail-for-a-fuse)

3. [7 Tips for Using Assertions in C](https://www.beningo.com/7-tips-for-using-assertions-in-c)


