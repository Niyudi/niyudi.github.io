---
layout: post
title:  "MAC0470 - Final patch-hub contribution"
date:   2026-07-28
categories: mac0470
---

The last part of this contribution was adding the message after applying the patch mentioning it was auto detected. The intention was originally to use a pop-up, but when further analysing the structure of the actions, the pop-up wasn't really feasible without big refactorings. The App logic was very strict on only giving one pop-up after the action was done.

![patch-hub patch](/assets/images/patch_hub_2.png)

So, as an alternative, I changed the output of the pop-up to indicate if the branch was auto-detected or not. It gives a little bit of clarity already, altough not much better then before this feature. Next steps could implementing a way to alter those configs inside path-hub, and also better error messages and pop-ups.