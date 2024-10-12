---
title: "Ship With Asserts Enabled"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/code-on-fire.jpg
---

![Code in flames](/assets/img/code-on-fire.jpg "Code going up in flames")

As a software engineer writing software for medical devices, I often
find myself in discussions about best practices. One topic that comes
up from time to time is assertion checking. I have found that
engineers often do not know about the practice or if they do know
about it, they don't use asserts as often as they could. It's also
common practice to disable assertion checking in production builds.

Not using assertion checking is a mistake, as is disabling asserts in
production builds. This does assume that the development team is
taking some other actions, which I discuss later in the article.

First though, let's understand what it means to use assertion checking
in software.

## Introduction to Assertion Checking in Software

Assertion checking is a powerful technique in software development
that helps ensure program correctness and catch bugs early in the
development process. Assertions are statements that express expected
conditions or invariants in your code. When an assertion fails, it
indicates that the program has entered an unexpected state, helping
developers identify and fix issues quickly.

Key benefits of using assertions in code include:

1. Bug detection: Assertions catch logical errors and invalid
assumptions early.

2. Self-documenting code: They clarify the developer's intentions and
expected behavior.

3. Improved debugging: When assertions fail, they provide immediate
   feedback on where and why an error occurred.

4. Enhanced code quality: Regular use of assertions leads to more
robust and reliable software.

## Simple Example: Using Assertions in a C Application

Here's a basic example of how to use assertions in a simple CLI C
application:

```c
#include <stdio.h>
#include <assert.h>

int divide(int a, int b) {
    // Assert that the divisor is not zero
    assert(b != 0 && "Division by zero is not allowed");
    return a / b;
}

int main() {
    int result = divide(10, 2);
    printf("10 / 2 = %d\n", result);

    // This will trigger an assertion failure
    result = divide(5, 0);
    printf("This line will not be reached\n");

    return 0;
}
```





As a software engineer working on safety-critical systems, particularly
medical devices, I often find myself in discussions about best
practices for code safety and reliability.


One topic that frequently
comes up is the use of assertion checking ("asserts") in production
code. In this article I share my thinking on why you should do this.

## It's Controversial

I'm hard core when it comes to using assertion checking in my code. I
use them frequently in my code to validate my assumptions about the
program's state at the current point of execution.

But here's where I differ from many of my colleagues: I argue for
leaving assertions in place and enabled in production code to be
shipped.

This is often alarming to developers that have typically turned off
assertion checking in code that is to be shipped. In this article, I
make the case for leaving them enabled.

## A Practical Example

Let's look at a simple example of how an assertion might be used in a
medical device's SPI driver. Here's a snippet from a hypothetical SPI
bus driver initialization function:

```c
void spi_init(uint8_t device_id) {
    hal_assert(device_id < MAX_SPI_DEVICES);

    // SPI initialization code...
    spi_config_t config;
    config.clock_speed = get_device_clock_speed(device_id);
    config.mode = get_device_mode(device_id);

    hal_assert(config.clock_speed > 0);
    hal_assert(config.mode <= SPI_MODE_3);

    // More initialization...
}
```

In this example, hal_assert() is the assertion checking routine from
our Hardware Abstraction Layer (HAL). It takes a single boolean
argument and signals (triggers) an assertion failure and some kind of
system halt if the condition is false.

In the case of an assertion failure, the system displays debug
information and then halts. On systems I build, this means doing a
processor reinitialization and then dumping the debug information
to some kind of output device, such as the system console (if it has
one.)

The debug information is often the contents of the general registers
as they were at the time of the assertion failure, the conditional
expression that triggered the assert (because it evaluated false) and
some kind of source code identifier that indicates the location of the
assert statement in the code. This might be source file and line
number, or an integer code that never is reused in the code base.

In the example code above, the first assertion checks that the
device_id is valid before we attempt to use it. The subsequent
assertions verify that we've gotten valid configuration data for our
SPI device.

These checks catch issues like:

1. Incorrect device IDs passed from higher-level code
2. Configuration errors in the device setup
3. Potential memory corruption affecting our config structures

By keeping these assertions in our production code, we ensure that the
SPI initialization fails safely if any unexpected conditions occur,
rather than trying to initialize an SPI device with invalid
parameters.

## The Argument Against Assertions in Production Code

The common argument I hear is that assertions should be turned off
when shipping a product. The reasoning usually goes something like
this:

1. Assertions add overhead and can impact performance.
2. If the code is well-tested, assertions shouldn't be necessary in
   production.
3. An assertion failure could cause the program to crash, potentially
   at a critical moment.

## How I See It

While these points may have merit in some contexts, they don't hold up
for medical device software. Here's why:

1. Safety First

In a medical device, safety is paramount. My experience is with
devices that are of a moderate or high level of concern. If the
device has the ability to help you it also has the ability to hurt
you if it does not operate properly.

An assertion failure, by halting the system in a highly visible way,
tells us immediately that something is horribly wrong. We've made an
incorrect assumption in the code or a hardware failure has occurred.

2. Undefined Behavior is Worse Than Crashing

If an assertion would have failed but was disabled, the code continues
to run in what amounts to an undefined state. In a medical device, this
is unacceptable. It could easily result in patient or operator harm.
If a device finds itself in this situation, it's always better to signal
a serious failure than to try to soldier on pretending all is well.

3. High-Priority Fixes

An assertion failure in the field is (or should be) a high-priority
issue for any medical device manufacturer. It may trigger a Corrective
and Preventive Action (CAPA) process or even a recall. While no one
wants this to happen, assertion failures in the field are indisputable
evidence that something unepected has happened and needs to be
investigated.

4. Thorough Testing Isn't Perfect

While we strive for comprehensive testing, no testing regime is
perfect. Assertions act as a last line of defense against unforeseen
issues or edge cases that slipped through testing.

## The Reputational Concern

A common reason for turning off assertions in production is a concern
that assertion failures in the field will somehow damage the company's
reputation.

I see it differently.

If the code is well-tested and robust, assertion failures in
production code will be extremely rare. If the code is not well tested
and robust, clearly, it's not ready to ship.

Of course assertion failures can also be triggered by code running on
broken hardware. In such cases, the assertion is doing what we want by
preventing the software from operating with faulty hardware.

Software that continues to run past a point where an assertion would
have failed (because assertions were disabled) is likely to exhibit
strange, buggy behavior. This can lead to even worse reputational
damage to a company than a clean crash and halt.

A medical device that's operating incorrectly is far more damaging to
a company's reputation than one that safely shuts down when it detects
a problem. Patients and healthcare providers rely on these devices to
work correctly. It's important to protect that trust.

## A Real-World Example

Consider a radiation therapy machine. If there's the slightest hint
that something's wrong with the code controlling dose delivery, it's
infinitely better for the machine to crash in a controlled way than to
continue running and deliver an incorrect dose of radiation. The
consequences of continuing to try to run when it's known that the
software is somehow compromised could easily be catastrophic.

## Best Practices

While I advocate for keeping assertions enabled, it's important to use
them judiciously and in conjunction with other safety practices:

1. Implement robust error handling and logging alongside assertions.

2. Consider graceful degradation for non-critical functions.

3. Assess and minimize performance impacts.

4. Ensure compliance with regulatory requirements.

5. Clearly document your assertion strategy and train people on it.

## Conclusion

In medical device software, the cost of a crash is almost always less
than the potential cost of continuing to operate with compromised
integrity. By keeping assertions enabled, we provide an additional
layer of safety that can prevent catastrophic failures and protect
patient lives.

I'm happy to discuss this topic further. If you'd like to dive
deeper into assertion strategies, error handling in medical device
code, or any related topics, feel free to schedule a consultation with
me through my website.

It's a privilege to be able to write software that improves people's
health. Let us do the best work we can do to protect the trust we are
shown.
