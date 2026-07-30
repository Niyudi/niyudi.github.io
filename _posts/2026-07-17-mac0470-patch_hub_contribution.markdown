---
layout: post
title:  "MAC0470 - patch-hub contribution"
date:   2026-07-17
categories: mac0470
---

For the second phase project, the project to which I will contribute is patch-hub. It is a subproject of kworkflow to aid in the Linux kernel development. It is a TUI program that allows you to see mailing lists, review patches and apply them to your local branches.

There is a particular issue with path-hub that annoyed me. To apply a branch, you have to configure the target kernel tree and branch target. For now, this has to be done in the config file manually. As such, when first attempting to apply a patch you get an error that simply says the target is unset and no instructions on how to fix it.

As such, the idea of the patch is to implement auto-targeting of a tree if no tree is set. Specifically, if the target tree is unset and the current working directory is a valid kernel tree, it sets the current tree and branch as the target automatically. This way, when first using the program, users are less confused as to why it fails.

Essentially, when validating the tree in the apply patch action, if it detects no kernel tree set in the configurations, it attemps to detect the current tree and branch. If it is successfull, it changes the config to the detected target, and runs the rest of the function.

![patch-hub patch](/assets/images/patch_hub_1.png)

There were some other small adaptations, such as using KernelTree by value instead of reference, which added unecessary lifetimes, and changing the config to be mutable so that it can be changed automatically. Overall, not much needed to be refactored.

Improvements can be made, for example with some kind of pop-up that says that the program is targeting the current branch automatically. This will be refined and sent as a patch later.