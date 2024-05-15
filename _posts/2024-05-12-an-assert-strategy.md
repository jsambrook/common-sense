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
inspect it.

In the case of production, your software will typically crash. When it
does, you want it to leave some clues as to why it crashed, Ofen, this
will be the source file name and line number of the failed assert, and
perhaps a textual representation of the conditional expression that
triggered the assert.



















This article will help you improve how you write software. It will
help you to write software that contains fewer errors and, when you
have errors in your software, you'll find and fix them more quickly.

If you understand this article and use assertion checking in your
code, it's likely that you'll become hooked and not want to write
software without assertions in the future.

Ultimately, this is all about helping you and other developers to
write the best software in the world.

Aim high!

## Assertions


<!--
In 1997 my client was Advanced Technology Labs in Bothell, WA. I was
asked to help with some work with ClearCase, a version control system
that was popular at the time. After working with them for a couple of
months they asked me to remain and help with the development of the
firmware for a new ultrasound system. Together we made it a win-win
deal and they continue as my client for many years.

One of the things I did along with the firmware lead (Bob Alexander)
was to develop a strategy or policy for using assertion checking on
the project. We were fortunate that the project was a green field (no
existing code) project, so we could work out whatever policies we
thought would be best.

We started with a decision to build the software in C++. The state of
C++ at that time was mostly that you could use it as a "better C." We
did that and a little more, in that we did do an object-oriented
software design while trying to avoid being on the bleeding edge of
C++ development.

We were using the MetaWare C++ compiler for the ARM7TDMI core we were
using. At that time you had to roll your own board support package.
That was my job, along with writing parts of the language runtime that
weren't supported in the MetaWare compiler at the time. The MetaWare
compiler was a good compiler, and their support was good too. It's
just that it was "early days" for C++ on embedded platforms.

In this article, I want to focus on the strategy I developed for using
assertion checking in our code. At the time, it seemed like a
controversial approach. Over time, it's proven to be a very good
approach.

# Assertion Checking Basics

Assertions ("asserts") are executable statements that can be inserted
in software. An assert is typically a macro that takes a conditional
expression. It's often the case that an assert statement will take
some additional arguments that provide information for use when the
assert fails. In desktop software, the assert statement takes only a
conditional expression. In the strategy I describe here, the assert
statement took a second argument, an unsigned 32 bit integer. More
about that later.

When an assert statement executes it evaluates the conditional
expression passed to it. If the expression evaluates to true
(non-zero, in C) the assert simply returns. If the expression
evaluates to false, then an "assertion failure" has occurred and the
assert routine handles the failed assert.

If assert statements are being used correctly on a project, an
assertion failure ("an assert") means that something is drastically
wrong. The executing program cannot diagnose exactly what has gone
wrong. In some sense, all it really knows is that the expression that
was supposed to evaluate to true actually evaluated to false.

# When to Use Asserts

Let's discuss how to use asserts in your software.

You use an assert statement when you are coding and you reach a point
where something must be true, or, if it's not, there is somethign
wrong with the logic of your program or the hardware on wich your
program is running.

For example, let's assume a simple routine written in C that is going
to fetch a value from a particular location in memory. Maybe it's the
address of a control register for a given bus controller, or something
like that.

You might have a routine like this:

```

//
// Initialize the system I2C controller. The controller's base address
// is given by base_addr. Return the status of the initialization operation.
//
status_code_t I2C_Initialize(uint32_t *base_addr) {
    assert(NULL != base_addr);

    ...

    return STATUS_SUCCESS;
}
```

In this example, the routine I2C_Initialize() is expecting to be
passed a pointer to the base of the controller's control
registers. That pointer has to be a valid one that can be
dereferenced. While the routine cannot completely validate the pointer
it is given, it can at least check to see to that is not obviously
invalid. In C, on most platforms, dereferencing a NULL pointer will
cause a bus error or other error condition. In embedded code, you
might have a need to write to location zero in memory, but even that
is pretty unlikely.

What is much more likely is that I2C_Initialize() will be called with
`base_addr` being NULL. Could be a simple coding error, or maybe some
kind of memory corruption has occurred and the caller's idea of where
the I2C controller is located in memory has been corrupted.

If I2C_Initialize() is called with a NULL pointer, the assert
statement will catch it and begin executing the code for dealing with
an assertion failure.

This is what we want. In a sense, the function I2C_Initialize() comes
with a usage contract. "If you do this, I'll do that."

In this case, if we pass the function a valid pointer, it will go on
to properly initialize the I2C controller. But if we pass it a NULL
pointer, it will assert. Perfect.

So, to summarize, you use assert statements when you want to test
something about the state the executing program at a particular place
in the code and that state is not something you would otherwise expect
during the normal operation of the program.

# When Not to Use Asserts

There are times when you might think you should use an assert, but in
fact, you should not.

For example, if the software is trying to open a file and cannot do so
for some reason, that's typically not a situation where you would code
an assert. Programs should not crash, for example, just because the
user mistypes a file name or tries to select an option that is not
available.

In these cases, you would use what most software engineers would call
normal error handling.

Another case would be errors that don't happen often, but could happen
rarely. For example, let's say you have an A-to-D converter on a SPI
bus in your device. You don't expect SPI bus errors in normal use, but
they could happen, and if they did, you might just retry the operation
if you can prove that doing so will be safe from other, unintended
side-effects.

-->
