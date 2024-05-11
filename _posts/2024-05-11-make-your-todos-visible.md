---
title: "Make Your TODO:'s Visible"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/michael-scott-grimacing.jpg
---

In the world of software and firmware development, it's pretty common
to come across comments like this one:

```
// TODO: Check for possible race condition.
```

These comments are reminders of work to be done sometime in the
future, but not now.

Recently, I've started encouraging my colleagues to go a step further
by creating a corresponding work item in our workflow management tool,
which in our case is Microsoft Azure Dev Ops (ADO), and leaving a
pointer to the created work item in the TODO comment itself.

So, using the example above and assuming that the created work item
in ADO is Task 3422, the TODO: comment might look like this:

```
// TODO: (3422) Check for possible race condition.
```

Note that I suggest leaving the literal "TODO:" prefix alone; some
IDEs have tools for finding literals like this, and I don't want to
invalidate that capbility if someone is depending on it.

Your situation will likely differ in the specifics, but the underlying
principle remains the same: Don't let future tasks linger unseen in
the various dark corners of your projects.

There are a couple of reasons for this:

First, it’s almost a given that people higher up than your immediate
manager aren’t diving into the code. And those TODO comments? They can
end up concealing a substantial amount of work. If higher-ups aren’t
aware of these tasks, they're in for a surprise—and usually, surprises
in this context don’t lead to good outcomes.

Second, as projects progress the workload often increases, to the point
where the project runs a very real risk of bogging down and flaming out.

An analogy I like to use is a "tractor pull." A tractor pull is an event,
often held at county fairs, where people bring high-powered farm tractors
to a racetrack and compete to see who can pull a "sled" behind the tractor
for the whole length of the track (a "full pull.")

The thing is, the sled is designed to get harder and harder to pull the
further the tractor goes down the track.

Often, tractors fail to go the distance. They start spinning their
wheels, there is a lot of smoke and flames from the tractor's exhaust,
and eventually the driver of the tractor has no choice but to give up.

[![Tractor pull fails](http://img.youtube.com/vi/728EEeKgFjo/0.jpg)](http://www.youtube.com/watch?v=728EEeKgFjo "Tractor Pull Fails")

So bottom line, it's important to make sure that your workload over
the lifetime of the project doesn't overwhelm the ability of the
project team to complete it in a highly satisfactory manner.

To wrap up, I suggest making likely future tasks visible in your
workflow mangement system, whatever it is. Annotating your TODO's as I
have described is a positive step in that direction.

I’d love to hear your thoughts on this. How do you ensure that
upcoming tasks don’t get lost in the shuffle in your work environment?
Is there a better strategy you’ve found effective? Feel free to share
your experiences!



