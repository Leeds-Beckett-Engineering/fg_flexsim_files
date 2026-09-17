# WC.04: Labels and Triggers

## Introduction

This _Workshop_ looks at the use of `Labels` and `Trigger`s in a bit more detail. You will have seen some `Trigger`s in the earlier 'Core' _Workshop_s, for instance in \_Workshop WC.00_ (_Using FlexSim for Simulation_) we used a `Trigger` to assign a type to the item 'produced' by the `Source`. This 'item type' is also a `Label` for the object, and we will talk a bit about how and why `Label`s are used to model more complex processes.

In the first part of the _Workshop_, though, we will focus on `Trigger`s; as typically `Trigger`s and `Labels` work closely together. We have already said that FlexSim simulations are based on a network of _discrete events_. FlexSim is also a specific type of discrete event simulator called _object-oriented_. So this means that broadly `Trigger`s are used to customise the behaviour of objects and models either by

1. Modelling the _events_ that occur during the simulation run; for instance when an item is created, modified, or destroyed.
2. Modelling the _messages_ that get sent between objects; for instance calling an operator, requesting a specific action, or updating a variable.

This short _Workshop_ introduces both uses of `Trigger`s in creating a simulation of a cardboard box assembly line for a packing process. Operators will be called to respond to specific events, and we will also modify the behaviour (and appearance) of the box as it move through the assembly line.

### New Objects

This _Workshop_ isn't specifically about the `Multiprocessor`, but is a good example of a `Multiprocessor` in use. So if you haven't seen the `Multiprocessor` object, this is a good introduction.

We will also discuss the use of `Label`s and `PerformanceMeasures`: which are not 'objects' as such but are closely associated with them. Learning about `Label`s will also enhance your understanding of how objects you have seen so far can be customised and adapted.

### Learning Outcomes

- Understand the use of `Trigger`s to modify the behaviour or appearance of an object in FlexSim

- Understand the use of `Trigger`s to create or respond to events in a FlexSim model

- Understand the use of `Label`s in a model, and how these can be created, updated, and used by `Triggers` within the FlexSim model

### Approximate Time to Complete this _Workshop_

This _Workshop_ should take about 30--40 minutes to complete.
