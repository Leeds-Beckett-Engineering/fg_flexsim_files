# WP.03: Using Labels to Refine Models and Data Outputs

\pagetab{WP.03}

## Introduction

In _Workshop WP02_ (_Running Experiments_) we looked at the use of the _Experimenter_ plugin to set-up scenarios for us, and to gather data from those simulated runs. Last time, however, we used more or less the 'native' properties of FlexSim objects; in much the same way as we have done previously for _Dashboards_.

For most realistic models, however, we need to have some ability to gather metrics of interest that may not be built-into the FlexSim objects by default. Or we may need to gather data from the model in a slightly different way than we would normally be permitted to do by FlexSim.

This is most easily accomplished in Flexsim by the use of `Label`s, which allow us to 'tag' objects, processes and other metrics to gather the data we are interested in. For instance using a label to track the progress of an item or part as it moves though the production process.

Using `Label`s with the _Experimenter_ plugin is particularly useful, as it give us an easy way to extract metrics of interest from the model runs. Or when using the _Experimenter_ to set-up specific targets as model `Parameters` for optimisation studies, using `Label`s makes it easier to set-up those targets in a way which makes sense for the model context.

In this Workshop we will return to the experiment run last time, but will use `Label`s to extract data needed to answer the following question: "_how much does the rework affect the overall cost of the product?_". We will do this by setting up a simple cost model, and then using the output metrics to derive a per unit cost at each of the rework scenarios from 10% to 20%.

This will give us a much more realistic idea of the data that can be extracted from a FlexSim model, and used to support management decisions. After all if the level of rework has little impact on the overall item cost, then it makes little sense to invest very much in improving the process. However if the cost changes dramatically, then there may be very good reasons to look at investments which can change the overall level of rework undertaken.

### Learning Outcomes

- Understanding the use of `Labels` to extract data from FlexSim Models
- Understanding the use of `Labels` to customise metrics of interest; possibly from synthesised metrics, model `Parameter`s, or `PerformanceMeasure`s

## Further Investigations: Impacts of Rework on the per-Unit Cost

### The Rejection Rate and Average Costs

![Relating the Rejection Rate to the Overall Queue Depth](media/p3_graph.png){ #fig:WP03_P3End width=75% }

At the end of _Problem Workshop 3_ we noted that by reducing the rejection rate in our production process we could decrease the maximum content (depth) of `Queue2` as show in [@fig:WP03_P3End]. We then _claimed_ that this reduction in the depth of `Queue2` represented an overall increase in efficiency, due to the removal of the bottleneck.

But can we validate this claim? Currently the claim that the _overall_ process is improved can only be made _indirectly_ through the claim we have eliminated (or at least reduced) a process bottleneck. Can we _directly_ link some aspect of the production metrics in the model to this change in the underlying production process?

To make a more direct argument, we need to instrument the underlying process model a bit more. We will add `Label`s to the items flowing through the production system, and then use these `Label`s to determine

1. **Overall Output**. How many items we have made (and tested) during the model run
2. **Overall Cost**. How much these items have cost to make during the model runs

Together the total cost and total output allow us to determine the final metric of _mean cost per item_ defined as

$$\text{Mean Cost per Item} = {\frac{\text{Total Cost}}{\text{Total Output}}}$$ {#eq:AvgCost}

Our 'cost model' in this _Problem_ will be very simple: every time an item crosses a `Processor` the cost goes up by `10`. Therefore in an ideal case where an item is made, tested, and accepted the unit cost is `10`. But if it is made, tested and rejected, then remade and accepted the unit cost will be `20` (`10 + 10`).

What we want to know how the _average cost_ changes as the _rejection rate_ increases. Ideally we should see the average cost approach the base cost of `10` if the rejection rate is `0`. But as the rejection rate increases, we would expect the average cost to rise to take into account the fraction of products being reworked. In this case a reduction in the _rejection rate_ is worth investing in, as it should decrease the average cost of production for each item.

Conversely, if the average cost stays roughly the same as the rejection rate rises, then there appears to be little incentive for changing the process. For even if the bottleneck is removed, if the average cost is our concern, then this improvement to the the reject rate may not matter.

So let's see what we can do to model the impact of the reject rate on the average cost. We will then have much more _direct_ evidence that the process is really 'more efficient' at lower rejection rates, and bringing the per unit cost down.

### Loading the Model

On the module web site their should be a FlexSim model called '_Problem Workshop 4 Template_'. Download this from the module site and open it in FlexSim.

The template for _Problem Workshop 4_ is very similar to the model you ended up with at the end of _Problem Workshop 3_: the `RejectRate` has been set for you (and the `Processesor`s modified), but we have removed the `PerformanceMeasure`s. If you want to start where you left off, you may want to delete the old `PerformanceMeasure`s before you begin.

### Step 1: Labelling `Item`s on Entry

We have seen `Label`s before in FlexSim, as we are currently using the `"Type"` label to distinguish between three different kinds of items that can be produced (represented by the blue, green, and red boxes). In addition to the 'standard' labels used so far, though, we can also create our own labels: pretty much as we need to.

In this case, in addition to the `"Type"` label for each item, we also want to set a 'cost'. This cost will initially be zero, but will then allow us to track how that cost per item changes as the item (box) moves through the model.

1. Find the `Source`, and open the _Properties_ to edit the _Triggers_. You should already see the trigger is marked as _On Creation_ and has the value _Set Label and Color_. Select the icon immediately to the right of the trigger (shown as a small hand) as shown in @fig:WP03_NewLab.

![Modifying an Existing Trigger](media/image1a.png){ #fig:WP03_NewLab width=45% }

1. The _Set Label and Color_ trigger is already set, and should have the `Label` named `"Type"` already set-up as shown in @fig:WP03_NewTrig.

2. Use the green '`+`' (plus) icon to add a new trigger, selecting _Data_ and then _Set Label_ (**note:** '_Set Label_' and not another '_Set Label and Color_': we have already set the colour, and just need another label). We will call this new `Label` `"Cost"` and simply assign it the value `0`. This will represent the cost of the item as it enters the model.

![Adding the `"Cost"` `Label` to the Trigger](media/image1.png){ #fig:WP03_NewTrig width=45% }

The final trigger for the `Source` show now look like @fig:WP03_NewTrig. Close the dialogue to return to the model.

### Step 2A: Reading the `Item`s on Exit

We have now set up the item _entering_ the model with a placeholder for the cost of that item. We will look at setting the cost shortly, we first we will look at _reading_ that per item cost as it the item leaves the model; creating the aggregate values that we need.

![The New Process Metric Labels for the `Sink`](media/image2.png){ #fig:WP03_SinkLab width=45% }

1. On the `Sink`, open the _Properties_ and locate the _Labels_ area (which should be empty).

2. Click on the green '`+`' (plus) icon, and then _Add Number Label_ to create a new label called `TotalCost`.

3. Repeat the process to add a `TotalOutput` `Label` to the `Sink`. The final result should look something like @fig:WP03_SinkLab.

Once the new `Label`s have been created, we need to update these as items exit the model (representing the end of the production process). This means creating new _Triggers_ for the `Sink` as follows

1. In the _Triggers_ area of the `Sink`, Click on the green '`+`' (plus) icon to create a new trigger _On Entry_. Now we can update the `Sink` labels as item _enter_ the `Sink`: but _leave_ the model.

2. Using the green '`+`' (plus) icon _for the trigger_, add a new trigger by selecting _Data_ then _Increment Value_.

![Adding an _Increment_ Trigger to an Entity](media/image10.png){ #fig:WP03_AddIncTrig width=45% }

We haven't used this style of trigger before, but as you might guess the _increment_ trigger allows us up to update a `Label` (or more correctly the _data_ in that label) by incrementing the current value of the `Label`. The properties of this trigger are therefore the _name_ of the `Label` we want to change (increment), and the quantity to adjust the label by.

We will use this new trigger type in two ways in the next steps, which should result in something like @fig:WP03_IncTrig when we finish the next set of steps

![Creating the Increment Triggers](media/image4.png){ #fig:WP03_IncTrig width=45% }

1. Set the value of the _Increment_ to the `Label` to `current.labels["TotalCost"]`. A quick way to set this correctly is to use the down arrow next to the entry box and select _Labels_ which should bring up the current list of defined labels

   **Note:** Being able to select existing labels is convenient, and is why we did that first: even though logically the values of the `Label`s reflect the triggers. In general it is easier to create all the `Label`s you need, then add the actions (a bit like defining your variables first, before then using them).

   The `Label` `"TotalCost"` should be in this list: selecting it should add the full parameter required to the entry box.

2. Set the _by_ of the new increment to `item.Cost`. This will increment the `"TotalCost"` `Label` of the `Sink` by the value of the `"Cost"` label of the item entering the sink. The overall effect should be a running total of the item costs for the `Sink`.

3. Now we need to record the _total number_ of items entering the `Sink`. Click on the green '`+`' (plus) icon in the trigger box to create a new increment trigger (i.e. selecting _Data_, then _Increment Value_) as shown in @fig:WP03_AddIncTrig. This trigger will \_Increment\_ the label `"TotalOutput"`, so either type in `current.labels["TotalOutput"]`; or select it from the list of known `Labels` as before.

4. The increment for the `"TotalOutput"` is just `1`, as we only need to record that a new item is entering the `Sink`. So we will leave the _by_ value as `1`.

### Step 2B: Resetting the `Sink` `Label`s

If you look at the _Labels_ area in the _Properties_ of the `Sink` you will see a small check box marked _Automatically Reset_, as shown in @fig:WP03_AutoReset.

![Setting the Automatic Reset of the `Label`s](media/image5.png){ #fig:WP03_AutoReset width=45% }

When we _Reset_ the model, we (usually) also want `Label`s that hold data from the previous model run to be returned to a default value. This means that successive runs of the model will start from the same place: which is very important if we are using something like the _Experimenter_. FlexSim can reset the value of `Label`s automatically: and checking this box will allow it to do just that.

But the default results are now always what you want: for instance you may want values set to a non-zero value or to be set to a random seed value. So we will alter the reset procedure for this model to show you how to do this yourself. The end result of our model reset should be like @fig:WP03_OnReset below, which will mirror the default settings

![Resetting+ the Values for `Label`s on Model Resets Using Triggers](media/image6.png){ #fig:WP03_OnReset width=45% }

1. Go to the _Triggers_ area and click on the green '`+`' (plus) icon. This time select the _On Reset_ trigger type as shown in @fig:WP03_OnResetTrigger, so that we can capture requests to reset the model values.

   ![Creating an _On Reset_ Trigger](media/image11.png){ #fig:WP03_OnResetTrigger width=45% }

2. Next, click on the green '`+`' (plus) icon next to the _On Reset_ entry box and select _Set Label_ to force a `Label` to a specific value when the reset is triggered. Either enter `"TotalCost"` for the _Label_, or use the small arrows next to the label box to select the `Label` as we did before. Leave the _Value_ for this as `0`, so that we reset `"TotalCost"` to zero on a model reset.

3. Then click on the green '`+`' (plus) icon at the bottom of the dialoge box, and create a new _On Reset_ trigger for the `"TotalOutput"` `Label`. Again leave the _Value_ for the `"TotalOutput"` `Label` as `0` as shown in @fig:WP03_OnReset.

### Step 3: Setting the `PerformanceMeasures`

Now that we have the `TotalCost` and `TotalOutput` set on the `Sink`, we will use the values of these labels to report these metrics as the output of the model. As in _Workshop 3_, we will use the `PerformanceMeasure` to track the model output: which will also then be incorporated into the output of the _Experimenter_. This will then allow us to calculate the final value of a per unit cost using the final data exported from the model.

![`PerformanceMeasures` Exported from the Model](media/image14.png){ #fig:WP03_PerfM width=45% }

1. To add the required `PerformanceMeasures`, go to the _Toolbox_ and navigate to the section entitled _Performance Measure Tables_ and double-click on the entry _PerformanceMeasures_. This should bring up the current `PerformanceMeasure`s.

2. Add the two `PerformanceMeasures` in @fig:WP03_PerfM: change the \_Name* of the first `PerformanceMeasure` to `ProcessCost`, and add a second with the \_Name* `ProcessThroughout`.

   Now we need to attach the new `PerformanceMeasures` to the `Label`s we have just created for the `Sink`, as shown in @fig:WP03_AddLab.

   ![Attaching a `Label` to a `PerformanceMeasure`](media/image15.png){ #fig:WP03_AddLab width=75% }

3. Click on the _Value_ field for `ProcessCost`, and use the down arrow to bring up the dialogue box allowing us to attach the _Value_ to the label. Use the 'eyedropper' next to the _Reference_ field, and select `Sink` from the 3D model, change the _Value_ to _Label by Individual Object_ and select the `TotalCost` from the drop down list. This should look like @fig:WP03_AddLab at this point. Close the dialogue box, and the \_Value\_ of `ProcessCost` should reflect the value of the `TotalCost` `Label` from the `Sink` (probably `0` at this point).

4. Follow the same process to attach the label `TotalOutput` to the `PerformanceMeasure` `ProcessThroughout`. This should update the _Value_ of the `ProcessThroughout` to the current value of the `TotalOutput` `Label` for the `Sink`.

### Step 4: Counting the Cost

We should now have a `Label` on each item generated by the `Source` that can track a `Cost` through the model. When the item reaches the `Sink`, the `Cost` on each item should now be added to the running total held in the `Label` `TotalCost` of the `Sink`; and the `Sink` also keeps a running tally of the number of items in the `Label` `TotalOutput`. Both the `Label`s for `TotalCost` and `TotalOutput` at the `Sink` are also reported as `PerformanceMeasures` for the model, so we can easily track progress.

But at the moment we haven't _actually_ updated the `Cost` label on the item at any point. So if you run the model you should see the `TotalOutput` (and the `ProcessThroughout` `PerformanceMeasure`) change --- but the `TotalCost` won't.

For our very simple cost model, all we need to do to fix that is to change the `Processor`s to add a fixed cost (`10`) to each item that passes through. This isn't very realistic, but can be made more sophisticated relatively easily.

1. Select the _Properties_ of `Processor1` and use the green '`+`' (plus) icon in the _Triggers_ area to create a new trigger. Select the _On Exit_ type for the trigger, as shown in @fig:WP03_OnExitTrig.

   ![Adding an _On Exit_ Trigger to the Model](media/image7.png){ #fig:WP03_OnExitTrig width=33% }

   As implied by the name, the _On Exit_ trigger fires every time an item leaves `Processor1`. We will use this to update the cost of the item, or more properly to update the _value_ of the items `Cost` `Label`

2. Updating the value for a label should be fairly familiar now. Click on the green '`+`' (plus) icon to add a new trigger, then select _Data_, and finally _Increment Value_ to bring up the dialogue box in @fig:WP03_OnExitValue allowing us to update the `Label`s value.

   ![Updating the Cost of the Item Leaving the `Processor`](media/image7b.png){ #fig:WP03_OnExitValue width=75% }

3. Change the _Increment_ box to read '`item.labels["Cost"]`', and the _by_ to '`10`' as shown in @fig:WP03_OnExitValue. Note that because the `Label` for the item has been set by us, FlexSim probably won't find it in the default list: so it is easier just to type this expression in by hand.

Once you have finished with `Processor1`, add triggers for `Processor2` and `Processor3` in the same way. Now every `Processor` should update the `Cost` `Label` of an item when it has finished processing.

### Step 5: Check Your Work

At this point it is worth _Running_ the model to see what happens. You should find the `TotalCost` and `TotalOutput` values of the `Sink` change as the model runs, and as item pass through the `Sink`, as shown in @fig:WP03_TestRun. These values for each run at the `Sink` should also be reflected in the `ModelParameters`: and indeed should be \_exactly\* the same if the `ModelParameters` are set correctly.

![A Test Run of the Model at the `Sink`](media/image3.png){ #fig:WP03_TestRun width=45% }

If nothing happens, or the `ModelParameters` are _not_ updating **check your work**! You _must_ get this to work before going any further, as FlexSim will rely on the `ModelParameters` for the next step.

### Step 6: Setting up the Experiment

Once the `Label`s have been set up, the _Experimenter_ set up should be reasonably straightforward. We have already undertaken most of the required steps for set up in _Problem Workshop 3_: all we will really change is the number of replications. FlexSim will automatically add the `ModelParameters` to the results of the _Experimenter_, so no further changes are needed there: at least assuming the `ModelParameters` are set correctly... Again _make sure_ the `ModelParameters` update successfully before going any further!

![The FlexSim Experimenter Set Up for the Model](media/image17.png){ #fig:WP03_FlexExp width=75% }

1. From the _Statistics_ menu, choose the _Experimenter_ to open the dialogue box shown in @fig:WP03_FlexExp.

2. We again want to run a series of experiments where the `RejectRate` varies from `10` to `20`, so we will add a new _job_ using the green plus button, selecting the _Range Based_ job. This should create something like @fig:WP03_FlexExp.

3. Next we need a _Name_: this can be whatever you want and is just used to distinguish between different jobs. Either keep the default, or add your own. We have chosen '`Cost_and_Throughput`' in @fig:WP03_FlexExp.

4. Now make sure the _Parameters_ are set to adjust the `RejectRate`. Since the `RejectRate` is the only `Parameter` we have set-up, FlexSim should add this automatically. If not, use the green plus button to add a new `Parameter` and select `RejectRate` to match @fig:WP03_FlexExp.

5. Finally we need to set the _Stop Time_ to `50000`: the same length of time as the 'manual' experiment we ran earlier in _Problem Workshop 1_, and the last experiment in _Problem Workshop 3_.

6. This time we will also alter the _Replications per Scenario_ value, to smooth the results of individual experiments. We will choose a value of `50`, which will allow a bit more insight into the underlying data and ensure that outlier values do not affect our conclusions too much.

### Step 7: Running the Experiment

When everything is ready, select the _Run_ tab and FlexSim should start running the model using the experiment parameters you have just set-up. This may take a bit of time, as FlexSimm will need to run each experiment `50` times before moving on to the next value of the `RejectRate`.

Let FlexSim run the experiment to its conclusion, and the click on _View Results_ when the bar turns fully green and all the results have been calculated.

### Step 8: Viewing the Results

Clicking on the _View Results_ button and FlexSim will show a _box and whisker_ plot of the simulation results. Because we have two `ModelParameters`, FlexSim will generate two diagrams, like the ones in @fig:WP03_BoxWiskCost and @fig:WP03_BoxWiskThroughput. You can select which graph to display by choosing the relevant `PerformanceMeasure` at the top-left of the dialogue box.

Click on _Generate Report_, and dump the raw data to Excel as a worksheet. You can now use the raw data to calculate the average per item cost as given in @eq:AvgCost, and as we have summarised in the graph below.

## Questions

1. From the graph in [@fig:WP03_BoxWiskThroughput], how does the overall _throughput_ change as the value of the `RejectRate` decreases? How confident are you in your answer? Why, and what would increase your confidence in the data generated?

   ![A Box and Whisker Plot of the Throughput](media/image19.png){ #fig:WP03_BoxWiskThroughput width=0.75\textwidth}

2. From the graph in [@fig:WP03_BoxWiskCost], how does the overall _cost_ change as the value of the `RejectRate` decreases? How confident are you in your answer? Why, and what would increase your confidence in the data generated?

   ![A Box and Whisker Plot of the Cost](media/image18.png){ #fig:WP03_BoxWiskCost width=0.75\textwidth}

3. Look at the final average costs in [@fig:WP03_AvgCostGraph]. Would, or could, you use this as evidence for small process improvements that only reduce the `RejectRate` from `20%` to `18%`? What confidence do you have in your answer, and why?

   ![A Graph of Average Cost as the `RejectRate` Changes](media/p4_graph.png){ #fig:WP03_AvgCostGraph width=0.75\textwidth}

4. Following on from Question (3), would you use @fig:WP03_AvgCostGraph as evidence for making larger process improvements that changes the `RejectRate` from `20%` to `15%`? What confidence do you have in your answer, and why?

5. What further experiments could you conduct that would find an 'optimum' change in the `RejectRate` that balances investment required against a drop in the average per unit cost?
