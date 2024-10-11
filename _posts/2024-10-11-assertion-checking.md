---
title: "An Opinionated Assertion Checking Strategy"
author: John Sambrook
tags: [journal]
thumbnail-img: /assets/img/code-on-fire.jpg
---

# Keeping Assertions Enabled in Medical Device Code

![Code in flames](/assets/img/code-on-fire.jpg "Code going up in flames")

As a developer working on safety-critical systems, particularly
medical devices, I often find myself in discussions about best
practices for code safety and reliability. One topic that frequently
comes up is the use of assertions in production code. Today, I want to
share my perspective on why I believe keeping assertions enabled in
shipped medical device code is not just acceptable, but often
necessary.

## The "Hard Core" Approach

I am "hard core" when it comes to assertions. I use them frequently in
my code to validate assumptions about the program's state at various
points. But here's where I differ from many of my colleagues: I leave
these assertions enabled in production code, shipping them active in
the final product.

## A Practical Example

Let's look at a simple example of how an assertion might be used in a
medical device's SPI driver. Here's a snippet from a hypothetical SPI
initialization function:

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

In this example, hal_assert() is our Hardware Abstraction Layer (HAL)
assertion function. It takes a single boolean argument and triggers a
system halt if the condition is false.

When the system halts, it will display the contents of the processor
general registers and the source file and line number of the failed
assert, along with the expression that evaluated false.

In this code, the first assertion checks that the device_id is valid
before we attempt to use it. The subsequent assertions verify that
we've gotten valid configuration data for our SPI device.

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
2. If the code is well-tested, assertions shouldn't be necessary in production.
3. An assertion failure could cause the program to crash, potentially at a critical moment.

## Why I Disagree

While these points have merit in some contexts, they don't hold up for
medical device software. Here's why:

1. Safety First

In a medical device, safety is paramount. An assertion failure, by
crashing the code, tells us immediately when we've made an incorrect
assumption or when something unexpected has occurred. This is
invaluable information.

2. Undefined Behavior is Worse Than Crashing

If an assertion would have failed but was disabled, the code continues
to run in an undefined state. In a medical context, this could lead to
incorrect measurements, improper device operation, or even harmful
actions. It's always better to crash than to "soldier on" with
compromised integrity.

3. High-Priority Fixes

A product crash in the field is a high-priority issue for any medical
device manufacturer. It may trigger a Corrective and Preventive Action
(CAPA) process or even a recall. While disruptive, this ensures that
critical issues are addressed promptly.

4. Thorough Testing Isn't Perfect

While we strive for comprehensive testing, no testing regime is
perfect. Assertions act as a last line of defense against unforeseen
issues or edge cases that slipped through testing.

## The Reputational Concern

One common reason for turning off assertions in production is a fear
that assertion failures in the field will damage the company's
reputation.

This fear is misplaced:

If the code is well-tested and robust, assertion failures in
production should be extremely rare. If the code is not well
tested and robust, it's not ready to ship.

Note that assertion failures can also be triggered by hardware
issues. In such cases, the assertion is doing its job by preventing
the software from operating with faulty hardware.

Software that continues to run past a point where an assertion would
have failed (because assertions were disabled) is likely to exhibit
strange, buggy behavior. This can lead to even worse reputational
damage than a clean crash and halt.

Remember, a medical device that's operating incorrectly is far more
damaging to a company's reputation than one that safely shuts down
when it detects a problem. Patients and healthcare providers rely on
these devices to work correctly, and that trust is paramount.

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

In the world of medical device software, the cost of a crash is almost
always less than the potential cost of continuing to operate with
compromised integrity. By keeping assertions enabled, we provide an
additional layer of safety that can prevent catastrophic failures and
protect patient lives.

I'm happy to discuss this topic further. If you'd like to dive
deeper into assertion strategies, error handling in medical device
code, or any related topics, feel free to schedule a consultation with
me through my website.

Remember, in safety-critical systems, it's not just about writing
code—it's about saving lives.

