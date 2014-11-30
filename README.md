Interactive Control of Physics-Driven Character Animation

### Abstract

This technique assumes that a large portion of keyframe animation is spent reaching for physical realism. Much less so than striving to communicate intent - which is ultimately what animation is all about.

We implement a framework that provides animators with the benefits of physical simulation without sacrifing the level of control present in keyframe-based animation techniques. The combination of animation techniques allows the animator to exploit the advantages of each technique. For example, a performance may ask for one character to put his hand on the shoulder of another character. An animator could then animate the hand in place, and provide the solver with a variable amount of influence over weight and interaction so as to reach a sufficiently accurate, physically-based representation of the interaction - providing the animator with control over the dynamic properties of the interaction along with the ability to transition back into full keyframe driven control at any point in time.

### Introduction

Keyframe animation provides animators with full control over each nuance of a performance, however physical weight and interactions is difficult and time-consuming to achieve. Rigid-body simulation on the other hand provides animators with access to automatic distribution of weight and physical interaction, however it also strips them of control.

In this paper, we present a novel approach to character animation at interactive speeds that bridges these two extremes; by providing animators with the full gamut of physics-based simulation tools and without sacrificing their ability to control their performances. The result is more accurate results in less time and less iterations.

### Previous Work

Traditional methods of combining keyframe animation and physical simulation generally takes place in the interactive domain - games - and typically works like this; keyframe animation fully controls a character until physics simulation is "triggered". At this point, the character is instead under full control of the dynamics solver **and is unable to return**. Endorphin, developed by Natural Motion, provides animators with the ability to specify high-level objectives for their characters and have their peformances simulated using a combination of rigid-body simulation and pre-computed responses. The result is immediate realism, **at the cost of control**.

### Architecture

Weightshift is a character articulation framework for physics-driven animation. It provides the animator with the benefits of current state of the art physical simulation solvers without reducing their level of influence of their final performance. Traditionally, the animator manipulates the `controller` which in turn manipulates a hierarchy of `joints` which finally deforms the `surface` of a character. Weightshift inserts itself inbetween the `joint` and `controller`, evaluating state prior to dispatching the resulting matrix into the `solver`. The `solver` then reverse-computes the given angles provided by the `controller` so as to dynamically *bend* and *flex* the `joints` into the expected position and orientation of the `controller`. The additional evaluation of state may provide additional guidance to the resulting angles and position of joints in scenarios where physical correctness hinders creativity (more on this below).

##### Evaluation of State

Prior to dispatching the resulting angles and positions provided by the `controller` to the `joints`, the character is evaluated. For example, it may be expected for a character to keep standing when lifting one leg; however, if the animator has not compensated the character's center of gravity he would naturally topple. Weightshift provides an additional layer of intelligence to the solver which may or may not append the additional information required to prevent this and keep the character standing.
