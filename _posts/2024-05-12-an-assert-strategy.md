---
title: "An Effective Strategy For Using Asserts in Code"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/pc-bite.jpg
published: false
---

# Assertion Checking

## For What Purpose?

I'm assuming you want to write valuable software quickly and well. If
that's your purpose, this article is for you.

I will explain how adding simple statements to your C or C++ code will
make it easier to debug and more reliable.

Let's start with an example from a real project.

## Build a Good Foundation

It was 1998 and I was working as a contract software engineer for a
medical device company near Seattle, Washington. We were setting out
on a new project, to build a new radical new ultrasound system. It was
to be easily hand-carried, all digitial, and battery powered.

The processor we selected was the ARM7TDMI, a very capable 32 bit core
designed by ARM and implemented in a custom ASIC we were designing.

My initial role with this company was helping them to build a good
foundation for their project. After doing that for a few months, they
asked me to help write the embedded software for this new project. I
was happy to do so.

As I look back on that project, I recognize how important that early
work was to the success of the project.

When you start a new project, if you build a good foundation, it will
pay benefits over the course of the whole project. If you don't build
a good foundation, and instead rush to start writing code, you will
lock yourself and everyone else into a poor experience over the whole
rest of the project.

Am I saying that it's impossible for a project that starts off poorly
to fully recover?

Well, it might happen, but in 40 years of writing software and working
on projects, I've never seen it.

The problem is that once you start writing code, it quickly becomes a
practical impossibility to stop and start over. Yes, you might be able
to refactor components here and there, but you're not going to have a
totally green field.

So, recognize a green field as a once-in-a-project opportunity. Don't
waste it.

The practice I am describing here, assertion checking, is something
you should include in your project foundation from the start. You
should also ensure that every member of your team understands the
practice very well. The strategy I am about to describe goes farther
than usual in our industry. You'll probably get some push-back from
people until they really understand it.

What can I say? No risk, no reward.

Our amps go to 11.

## Assertions In C and C++

An assertion is a claim. If I say to you "The sky is blue" that's a
claim you can easily verify, by simply looking at the sky.

In software - and I am only talking about C and C++ in this article -
we can make claims and have them tested at run-time by making calls to
a function (strictly speaking, it's usually a preprocessor macro)
called `assert`:

```
void init_i2c_ctlr(i2c_ctlr_t *ctrl_base_addr) {
    assert(NULL != ctlr_base_addr);

    ...
}
```

In the above, the C function `init_i2c_ctlr` is a function to
initialize an I2C controller whose base address is passed in
`ctlr_base_addr`. The first thing the function does when called is to
check that the address it was given is not NULL.

If the given pointer is valid, the function continues on to do its
work. If it's not - if it's NULL - an assertion failure occurs.

Typically, assertion failures cause the running program (or thread
of execution) to exit after recording some information about the
location of the failed assert in the software.

Sounds drastic, I know, yet it's actually a really good thing. More
about that in a moment.

## Asserts Facilitate Design by Contract

Design by Contract (DbC) is a methodology where software designers
define precise and verifiable interface specifications for software
components. These specifications, or "contracts," detail the mutual
obligations and benefits of software components, such as functions.

Function contracts often have three key components:

### Preconditions:

Conditions that must be true before a function is executed.

These are the responsibilities of the caller of the function.

Example: A function that calculates the square root of a number may
have a precondition that the input must be non-negative.

### Postconditions:

Conditions that must be true after the function has executed.

These are the responsibilities of the function itself.

Example: A function that sorts an array may have a postcondition that
the array is in ascending order.

### Invariants:

Conditions that must remain true during the execution of a program.

These are typically used in the context of object-oriented programming
to ensure that an object's state remains consistent.

Example: An invariant for a class representing a bank account might be
that the balance cannot be negative.

### Example:

Here is a C function that illustrates pre-conditions and
post-conditions:

```
#include <assert.h>
#include <stdio.h>

// Function to divide two integers and return a float
float divide(int a, int b) {
    // Precondition: b must not be zero
    assert(b != 0 && "Precondition failed: b must not be zero");

    float result = (float)a / (float)b;

    // Postcondition: The result must be a valid float
    assert(result != 0.0f || a == 0 && "Postcondition failed: result must be a valid float");

    return result;
}

int main() {
    int a = 10;
    int b = 2;
    float result = divide(a, b);
    printf("Result of %d / %d = %.2f\n", a, b, result);

    // Uncommenting the following line will trigger a precondition failure
    // float error = divide(a, 0);

    return 0;
}
```

## When Assertions Fail

Assertions are claims that you make when you are writing your
software. What should happen at run-time when one of these claims
turns out to be wrong?

When you're running your software under the control of a debugger, you
can set a breakpoint on the function that the `assert` macro calls
when an assertion failure occurs. That is often great, because you
have access to the state of the whole program and the debugger to
inspect it when it asserts.

In production builds an assertion failure usually causes a program
crash. You'll want to make sure that when your production code does
assert, it reports the location of the failed assert, often in the
form of a source file name and source file line number. It is often
possible to arrange for a textual representation of the conditional
that caused the assert to be printed. Bottom line, you need to be able
to go from the information reported in the crash to the location of
the assert in your code.

## A Key Assumption

It's often claimed that you should turn off asserts in your production
builds. I have never found a good reason for doing this. Here's why.

If there is a concern that you software might assert while a customer
is using it, you're not doing enough testing of the software before
releasing it.

You might not be doing enough testing because you have not made an
investment in building systems that will help you to test it. This is
an area of interest for me: Building practical systems for testing
embedded systems thoroughly before releasing them to customers.

I have built embedded test frameworks that supported a large number
of high-end medical ultrasound systems. By large number, I mean 30
or so, but it could have been scaled even higher if necessary.

Each ultrasound system under test was connected to a JTAG emulator and
also had a serial line connection to a host controller. Through the
JTAG connection we could completely control the Unit Under Tests
(UUT). We could flash new code, reboot the system, save a core image
when the system crashed, etc. Through the serial line, we could inject
commands for the system to run. With the test scripts we wrote to
drive the UUT through its paces, we could explore any system states we
wished. We could also save screen images from the UUT for image
analysis, including OCR.

The above might seem like overkill, but it was a key component of the
engineering department for this company that went from startup to a
billion dollar company in about twenty years.

So the key point is this: It's fine to ship production code with
assertion checking enabled as long as you test it well enough that
having a customer experience an assertion failure is a truly rare
event.

## Bottom Line

Your success as a software engineer hinges, in part, on improving
your reputation as someone who is effective and reliable. You can
greatly improve your software by ensuring that as it runs, it is
constantly checking to see if the assumptions you had in mind when
you wrote it are still valid.

If the program is going off the rails, it will serve you and your
colleagues to have it fail in a highly obvious way as early as
possible. These programs as the easiest and fastest to debug.

Programs that rumble on with undiagnosed internal inconsistencies
before they finally either fall over or display nonsense answers
to customers are not only difficult and painful to debug, they
also do damage in many other ways, seen and unseen.

Assertion checking is a technique that, used properly, will
greatly improve your effectiveness as a software engineer.

Asserts help you do this. I hope you'll consider experimenting with
them if you're not already using them. Let me know if you need help
creating a highly effective solution for you and your colleagues.
This work a












