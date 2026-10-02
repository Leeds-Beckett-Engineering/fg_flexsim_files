# WC.03: Fluid Modelling

## Introduction

This _Workshop_ introduces most of FlexSim's _fluid objects_. You will learn how they interact with each other and how to include them in a model with the _discrete objects_. Building a model with the fluid objects is more involved and requires more attention to detail than a model with the discrete objects. For that reason, it is recommended that you feel comfortable building models with the other objects before you begin to learn about the fluid objects.

We have already seen the use of _discrete_ time in creating a process model: fluids essentially model a _continuous_ time flow of objects. In FlexSim, 'fluids' tend to be used for one of two reasons

1. Some materials are actually 'fluid' in the normal sense, and here the model of discrete flow becomes more challenging. Fluid modelling usually requires representations that can easily replicate continuous flow: fluids do not naturally divide into discrete chunks. For instance there is no easy mechanism for moving half a flow item in FlexSim. So we need a different mechanism for the model of the process flow.

2. In some cases there are so many discrete items in the systems that it becomes impractical for reasons of time, efficiency or resources to fully model the underlying behaviour. For example, thousands of bottles in a filling line will slow down a model that uses a flow item for each bottle. In these cases it may be better to use an alternative representation of the items in the process: and fluid modelling is one such approach.

In this _Workshop_, we will concentrate on the first use case of fluid modelling as the most common requirement in modelling. We will talk about the conversion of discrete (time) items into fluids, and back again, and these techniques can also cover the second case. FlexSim also has specific support for common aspects of the second case, for instance the recent introduction of the [Mass Flow Conveyor](https://www.flexsim.com/videos/mass-flow-conveyor) for modelling large numbers of items moving between two points. Some of the later \_Workshop_s will also pick-up on cases where fluid flow is helpful in either reducing the complexity of the modelling, or more easily expressing the intent of the model.

_Note:_ This _Workshop_ assumes that the model is set up to use _litres_ as the default unit for the fluid flow. This should be the default on the lab machines (and on those on the Remote Access Server): but you may need to check and change the units for your model.

### Learning Outcomes

- How to model fluid material with FlexSim

- How to convert flow items into fluid material

- How to transfer and store fluid material

- How to use level marks on a tank to control material flow

- How to mix fluid materials together

- How to convert fluid material into flow items

### New Objects

In this _Workshop_ you will be introduced to the `FluidTicker`, `ItemToFluid`, `FluidPipe`, `FluidTank`,`FluidMixer`, `FluidProcessor` and `FluidToItem` objects.

### Approximate Time to Complete this _Workshop_

This _Workshop_ should take about 45--60 minutes to complete.

