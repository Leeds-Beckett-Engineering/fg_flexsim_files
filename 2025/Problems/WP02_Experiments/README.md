# WP.02: Running Experiments

## Introduction

So far we have concentrated on setting up FlexSim in a reasonably static manner: adding objects, and then configuring both objects and connections. We have introduced some uncertainty, or at least randomness, though the use of statistical distributions; but otherwise once the model can been configured we have largely just look at the results.

For some questions we need to answer via the model, this static layout of objects and connections is fine. But for other models we are exploring a _problem space_, where there are multiple potential solutions and we want to use the model to find the most appropriate one.

FlexSim handles the exploration of problem spaces in the _Experimenter_ plug-in, which we will look at here. This will be a very simple experiment, looking at a single variable we can adjust and a single variable summarising the model. Most models will be much more complicated than this: but you should be able to build those more complex models once you complete this workshop.

If you would like to explore the _Experimenter_ plug-in in more detail, you are strongly recommended to look at the [Official FlexSim Tutorial](https://docs.flexsim.com/en/22.2/Tutorials/AdditionalTools/Tutorial4Experimenter/4-1ExperimentJob/4-1ExperimentJob.html) and the [FlexSim Guide to the Experimenter](https://docs.flexsim.com/en/22.2/GettingData/AdvancedDataGathering/RunningJobs/RunningJobs.html). Both extend the concepts introduced in this Workshop, and lay the foundations for other types of experiment that you can also perform.

### Learning Outcomes

- Understanding the use of `Parameters` to add variables to object properties
- Understanding how to extract data from the model using a `PerformanceMeasure`
- Being able to use the _Experimenter_ to create simple scenarios which capture potential model solutions
- Understanding the impact of randomness in interpreting scenario and experimental data
