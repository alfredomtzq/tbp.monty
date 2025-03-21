---
title: Implement Efficient Saccades Driven by Model-Free and Model-Based Signals
---

Currently the main way that the distant agent moves is by performing small, random, saccade-like movements. In addition, the entire agent can teleport to a received goal-state in order to e.g. test a hypothesis. We would like to implement the ability to perform larger saccades that are driven by both model-free and model-based signals, depending on the situation.

In the model-free case, salient information available in the view-finder could drive the agent to saccade to a particular location. This could rely on a variety of computer-vision methods to extract a coarse saliency map of the scene. This is analogous to the sub-cortical processing performed by the superior colliculus (see e.g. [Basso and May, 2017](https://www.annualreviews.org/content/journals/10.1146/annurev-vision-102016-061234)).

In the model-based case, two primary settings should be considered:
- A single LM has determined that the agent should move to a particular location in order to test a hypothesis, and it sends a goal-state that can be satisfied with a saccade, rather than the entire agent jumping/teleporting to a new location. For example, saccading to where the handle of a mug is believed to be will refute or confirm the current hypothesis. This is the more important/immediate use case.
- Multiple LMs are present, including a smaller subset of more peripheral LMs. If one of these peripheral LMs observes something of interest, it can direct a goal-state to the motor system to perform a saccade such that a dense sub-set of LMs are able to visualize the object. This is analogous to cortical feedback bringing the fovea to an area of interest.

Such policies are particularly important in an unsupervised setting, where we will want to more efficiently explore objects in order to rapidly determine their identity, given we have no supervised signal to tell us whether this is a familiar object, or an entirely new one. This will be compounded by the fact that [evidence for objects will rapidly decay](../learning-module-improvements/implement-and-test-rapid-evidence-decay-as-form-of-unsupervised-memory-resetting) in order to better support the unsupervised setting. 

Unlike the hypothesis-testing policy, we would specifically like to implement these as a saccade policy for the distant agent (i.e. no translation of the agent, only rotation), as this is a step towards an agent that can sample efficiently in the real world without having to physically translate sensors through space. Ideally, this would be implemented via a unified mechanism of specifying a location in e.g. ego-centric 3D space, and the policy determining the necessary rotation to focus at that point in space.

