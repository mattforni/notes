# Eliminating Toil
## Overview
Toil is the kind of work that is related to operating a production service that is often times **manual**, **repetitive**, **automatable**, **tactical**, **devoid of enduring value**, and **scales linearly with growth**. 

### Manual
This is all work that requires time from a human to get done. Even if the task has been automated to some degree via a script, if a human is needed to initiate running that script, the work is manual.

### Repetitive
This is all work that is being performed for the third, fourth, or *n-th* time. This **does not** include work that is part of solving a novel problem.

### Automatable
This is all work that a machine can solve this problem as well as or better than a human, or if the need for the work can be designed away.

### Tactical
This is all work that is interrupt-driven and reactive versus work that is strategic and proactive. Often this time of work cannot be fully removed, but it can and should be minimized.

### No Enduring Value
This is all work that does not permanently improve the state of the system or operating the system. If the state of the world is the same at the beginning and end of the work, it is likely toil.

### Scales Linearly with Growth
This is all work that grows as the size of the service, traffic, or user count increases. Services should be able to scale by an order of magnitude without needing additional work.

## Why Less Toil
Less toil means more time spent on engineering problems, which in turn often results in improvements to reliability, latency, and utilization. The second order effect of improving these areas is often times less toil.

Engineering work most often requires human judgement and results in permanent improvement to the operation of a service. This work is also usually guided by strategy, and involves a fair amount of design-oriented thinking. In short, it is work that is engaging and produces tangible results. This kind of work often falls into either software or system engineering.

### Software Engineering
Software Engineering involves writing code that lessens the operational burden of running an application. This can manifest as automation scripts, tools or frameworks, service features that enhance scalability or reliability, etc.

### Systems Engineering
Systems Engineering involves configuring, modifying, or documenting systems to produce enduring improvements to the operational burden or running a service. This can manifest as setting up or tuning monitoring, load balancing configuration, determining autoscaling boundaries, etc.

## Is Toil Always Bad?
The short answer is no, and completely eliminating toil is not possible in most cases. Toil truly becomes toxic when it is experienced in large quantities. Too much toil can result in adverse effects to an organization, including:

- Career Stagnation
- Low Morale
- Confusion
- Slow Progress
- Increased Operational Burden
- Attrition
- 