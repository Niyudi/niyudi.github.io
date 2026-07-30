---
layout: post
title:  "MAC0470 - A short KW contribution"
date:   2026-07-27
categories: mac0470
---

Earlier in the course, I noticed a bug in KW. In short, a function that used to be in scope when running "kw deploy --modules" was moved to another file with a refactoring, but no one noticed this particular command broke. The solution was a one-liner: just import the correct file right before running the function.

![KW patch](/assets/images/kw.png)

I sent this a while ago, and recently the maintainer responded saying that my commit message was not in style, so I should just adjust that. I adjusted the commit and shortly, it should be approved.