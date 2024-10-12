---
title: "Ship With Asserts Enabled"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/code-on-fire.jpg
---

![Code in flames](/assets/img/code-on-fire.jpg "Code going up in flames")

In the realm of medical device software engineering, the topic of best
practices frequently arises in professional discussions. Assertion
checking, a powerful technique for improving code quality and
reliability, is often underutilized or misunderstood.

Many engineers are either unfamiliar with assertion checking or fail
to leverage it to its full potential in their code. This gap in
knowledge or application is particularly concerning in the medical
device industry, where software reliability can have life-critical
implications.

Moreover, it's a common industry practice to disable assertion
checking in production builds. While this approach may offer marginal
performance benefits, it potentially removes a valuable safeguard
against unexpected runtime conditions in deployed systems.

This situation underscores the need for greater awareness and
education about assertion checking, its benefits, and its appropriate
use in both development and production environments, especially in
high-stakes fields like medical device software engineering.

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

To compile and run this program:

1. Save the code in a file named assert_example.c

2. Compile it with: gcc -o assert_example assert_example.c

3. Run the program: ./assert_example

You'll see the first division result, followed by an assertion failure
when trying to divide by zero.

Assertions are typically enabled during development and testing but
can be disabled in release builds for performance reasons. To disable
assertions, compile with the NDEBUG macro defined: gcc -DNDEBUG -o
assert_example assert_example.c

Remember, while assertions are valuable tools, they should not be used
for error handling in production code. Instead, use them to catch
programmer errors and invalid assumptions during development and
testing. I recommend you leave them enabled in your production code
as well, as I explain below.

## Assertion Checking: A Critical Safeguard in Medical Device Software

In medical device software, a single bug can mean the difference
between life and death. Let's explore how assertion checking, a simple
yet powerful programming technique, can prevent catastrophic failures
in devices like surgical robots or infusion pumps.

### The Scenario: An Infusion Pump Gone Rogue

Imagine an infusion pump designed to deliver pain medication to a
patient. The pump is programmed with a maximum safe dosage rate to
prevent overdose. Here's a simplified version of what the code might
look like:

```c
float get_rate_from_user_input() {
    // This function is expected to perform input validation
    // and return only valid rates within the safe range.
    // Implementation details omitted for brevity.
}

float get_duration_from_user_input() {
    // This function is expected to perform input validation
    // and return only valid durations within the safe range.
    // Implementation details omitted for brevity.
}

void deliver_medication(float rate, float duration) {
    float total_dose = rate * duration;
    activate_pump(total_dose);
}

// In main control loop
float rate = get_rate_from_user_input();
float duration = get_duration_from_user_input();
deliver_medication(rate, duration);
```

At first glance, this might seem fine. The input functions are
expected to perform their own range checking and return only valid
values. But what if there's a bug in one of these functions? Or what
if a hardware glitch causes it to return an abnormally high value
despite the checks?

### The Potential Disaster

Even with input validation, if a bug or hardware issue causes these
functions to return invalid values, the pump might accept them,
potentially leading to a massive overdose that could harm or even kill
the patient.

### The Assertion Solution

Now, let's see how assertion checking can add an extra layer of
safety:

```c
#include <assert.h>

#define MAX_SAFE_RATE 10.0  // ml per hour
#define MAX_SAFE_DURATION 24.0  // hours

float get_rate_from_user_input() {
    // Input validation logic here
    // ...
}

float get_duration_from_user_input() {
    // Input validation logic here
    // ...
}

void deliver_medication(float rate, float duration) {
    // Assertions as a safety net, not for input validation
    assert(rate > 0 && rate <= MAX_SAFE_RATE && "Rate must be positive and within safe limits");
    assert(duration > 0 && duration <= MAX_SAFE_DURATION && "Duration must be positive and within safe limits");

    float total_dose = rate * duration;
    assert(total_dose <= MAX_SAFE_RATE * MAX_SAFE_DURATION && "Total dose exceeds safe limits");

    activate_pump(total_dose);
}

// In main control loop
float rate = get_rate_from_user_input();
float duration = get_duration_from_user_input();
deliver_medication(rate, duration);
```

With these assertions in place:

1. If, despite input validation, the rate somehow exceeds the maximum
   safe level, the program will immediately halt before activating the
   pump.

2. If the duration is too long, again, the program stops.

3. Even if both rate and duration are within their individual limits,
   we check that their combination doesn't exceed the maximum safe
   total dose.

### The Life-Saving Difference

These assertions serve as an additional line of defense against
unexpected conditions. They're not replacing proper input validation,
but rather complementing it. In the event of a software bug, hardware
glitch, or any other unforeseen issue that bypasses the normal input
checks, these assertions would trigger, causing the program to
terminate immediately. The pump would not deliver the excessive dose,
and the patient would remain safe.

Moreover, these assertions serve as clear documentation of the
system's safety requirements, making the code self-explanatory and
easier to maintain and audit.

### Beyond Development: Assertions in Production

It's common practice to disable assertions in production builds.
People seem to have different reasons for doing this.

Turning them off for performance reasons is not sensible. First, you
almost never need to include asserts in very tight, performance
sensitive code. Second, if your processor(s) are not sized to allow
you enough performance margin so that assertion checking is too
burdensome, the solution is to get more compute capacity. You'll need
it down the road, anyhow, if the product is a valuable one.

The most common reason that I have encountered for turning them off is
that no one wants the software to crash in the field. I understand
that, yet turning off the error detectors is not the solution. The
solution is to pay the price to build the software as well as possible
and then to verify it thoroughly before shipping it.

### Assertion Checking Also Catches Hardware Errors

Assertion checking also provides a line of defense against hardware
errors. Even when the code is completely correct, hardware errors will
often result in assertion failures. This is a good thing, as we never
want to be in the position of allowing broken hardware to be used to
diagnose or treat patients.

## A More Concrete Example

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

In the case of an assertion failure, the system displays debug
information and then takes some appropriate action.

Depending on the use of the system, it may be sufficient to crash the
system. This often means doing a processor reinitialization and then
dumping the debug information to some kind of output device, such as
the system console (if it has one) and then spinning in a tight loop.

On other systems, it may be necessary to reinitialize the processor,
log the assertion failure information in some way, and then restart.
There are systems where this kind of reliability is necessary.

When an assertion failure is detected, there is debug information that
should be displayed and/or saved somewhere. This is often the contents
of the general registers as they were at the point of the assertion
failure, the conditional expression that triggered the assert (because
it evaluated false) and some kind of source code identifier that
indicates the location of the assert statement in the code. This might
be source file and line number, or an ever-increasing integer code
that never is reused in the code base.

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
