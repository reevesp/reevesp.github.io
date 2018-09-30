---
layout: post
title:  "Experiences of Environment Specific Config in APIC v5"
---

I was mulling over how best to do resolve my issue today.

I had a scenario with two APIs. (These are APIs in the APIC sense, so swagger-based integrations. I am viewing everything from the perspective of an API-provider.)

(If you were an API consumer, I wouldn't want you to be confused about how my security-gateway/backend is implemented. From an API-consumer-perspective, there is just one nice and simple API for you to consume.)

The first APIC API was a proxy API that would run some security-processing, security-enforcement, or routing before calling the second API. (_This is a very common pattern in DataPower-land as it allows an organisation to only have to expose a single port; I'm not sure how well it translates to APIC-land. I was just interested in playing with chained APIs_)

The second APIC API would be doing the api-specific application-processing of my gateway, and in my example would just echo back "Hello World" (like all the best APIs).

_How should I configure my first API to call the second one, given that IPs/ports/paths/resources are likely to change?_

An invoke policy would be necessary. But how was best to handle the environment-specific config?

There are always lots of different options here, and these are not product-specific. Do I bake the config into the deployable unit? Do I have a lookup in my processing?

I asked around some colleagues about what might be a good way to go. The suggestion that I received was to either amend to the swagger (APIC's deployable unit) at build- or deploy-time.

This reduced the problem to a string-manipulation build-script type problem, which is typically simpler and easier to handle and works for me.
