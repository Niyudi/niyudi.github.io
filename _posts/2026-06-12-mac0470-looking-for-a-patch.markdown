---
layout: post
title:  "MAC0470 - Looking for a patch"
date:   2026-06-12
categories: mac0470
---

Now the task is to find a patch to apply to the Linux kernel. Since I have a personal interest in Rust, I thought to look into the Rust for Linux project.

First of all, studying the code and project as a whole, just like C kernel code, there's a lot of slightly unusual code, as such a low level system requires. The main efforts in the project seem to be in making a good interface between the Rust parts of the kernel with the C parts. The main strength of Rust laguage is its robust type system, and so many types and traits are defined to make abstractions over common patterns in C.

For example, one recent patch implements the type PhysAddr, which is a wrapper around the C binding to the phys_addr_t type:

![PhysAddr patch](/assets/images/phys_addr.png)

Wrapping the binding in a Rust struct allows the compiler to enforce stronger guarantees, and to have convenience functions implemented directly over the type. This makes mantainability easier and memory mistakes harder to make. This type of pattern is common accross the codebase.

Another interesting thing is that the Rust for Linux project also involves a lot of work outside the kernel. Mainly, people are encouraged to contribute to Rust itself, and some crates that the kernel has adopted. If a crate seems useful, it may be absorbed into the kernel just like C libraries.

A mantainer also keeps issue on Github specifically for new developers to jump in. Of course, you still send patches normally to solve them, but it is a nice place to quickly look for easy entry points into the development. It is updates a bit infrequently, however.