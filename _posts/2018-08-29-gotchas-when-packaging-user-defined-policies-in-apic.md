---
layout: post
title:  "Gotchas when packaging User-Defined-Policies in APIC"
---
Something I have been playing around with recently are user-defined-policies (UDPs) in APIC.
As a quick recap, APIs can have custom assemblies attached to them, so that custom processing will be run on the message in the gateway.
Assemblies are made up of policies, and a user-defined-policy is not unlike a callable unit/function/subroutine.

I started off from these two KC articles:
https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.policy.doc/tapim_import_custpolicies.html
https://www.ibm.com/support/knowledgecenter/SSMNED_5.0.0/com.ibm.apic.policy.doc/tapim_creating_policy.html

I built myself a single DataPower action (with INPUT and OUTPUT of NULL), which was being consumed by a single DataPower processing rule. I packaged it up, and naively loaded into APIC. The API Designer returned the following error to me.
```
Validation failed for policy type CountToTen. The implementation zip file does not contain an export.xml file
```
I figured that I had copied the DP export incorrectly. The first KC link outlines the correct directory structure:
```
policy.yaml
implementation/
              mypolicy_dp.zip
```

I rebuilt my zip and tried again.
```
Validation failed for policy type CountToTen. The implementation zip file has the following files that are not valid: local:///foo/bar.js
```
And some further rebuilding...
```
Validation failed for policy type CountToTen. The implementation zip file has the following files that are not valid: local:///policy/bar.js
```
And finally, some googling. I eventually came upon another page full of gotchas on KC, that really should be the first page!
https://www.ibm.com/support/knowledgecenter/en/SSMNED_5.0.0/com.ibm.apic.policy.doc/tapim_pol_implement.html
_Seriously, this is a good link to read start-to-end, with loads of good details._
The gotchs which I was falling for was that the name of the policy has to be repeated in many places. E.g. For a UDP called `my-cool-policy`:
  - the `policy.yaml` should be `my-cool-policy.yaml`.
  - Inside the yaml, the value of-
  ```
  info:
          name:
  ```
    -must be set to `my-cool-policy`.
  - The DP processing rule in your export should be named `my-cool-policy-main`.
  - The actions in your processing rule should have names starting with the prefix `my-cool-policy` .
  - Any files (.xsl's and .js's) that your processing actions need to reference should be stored in the `local:///policy/my-cool-policy/` directory.
