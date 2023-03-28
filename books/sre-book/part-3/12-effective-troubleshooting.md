# Chapter 12: Effective Troubleshooting
## Overview
Troubleshooting is a skill necessary for anyone tasked with developing and operating a system, no matter how large. Confidence in troubleshooting systems comes from repeatedly having to do so, but is a skill that can be taught and learned over time.

## Theory
Ultimately the process of debugging a system involves generating a list of hypotheses as to why the system is failing and then test each hypothesis, systematically eliminating each as the cause of the failure until the correct hypothesis is validated. Sometimes this involves "treating" the system, which involves changing the system in some controlled manner and observing the outcomes in the operation of the system. 

## In Practice
Troubleshooting rarely adheres to theory though. In practice, the troubleshooting process is most often initiated by some kind of problem report, most commonly in the form of a page. An effective problem report specifies both the expected behavior and the actual behavior, and in an ideal world, how to reproduce the issue.

All issues vary in impact and should be handled according to their impact. Issues that only affect a single user do not warrant the same degree of attention as an issue that affects large swaths of users. In either case though the first course of action should be to triage and mitigate the impact by modifying the system to work as well as it can under the circumstances. First, stop the bleeding by any means necessary, and then focus on figuring out what went wrong.