# WP.00: Identifying Bottlenecks

## Introduction

For this Workshop we will change the focus from _how_ we use FlexSim to _why_ we use FlexSim. Rather than begin a model from scratch, we will instead look at an existing model, and see if we can gather evidence for changes to the current process.

In this model we will look at the process of manufacturing three types of products in a factory. In our simulation model, we will associate an item type value with each of the three product types. These three types all arrive intermittently from another part of the factory. There are also three machines in our model. Each machine can process a specific product type. Once products are finished at their respective machines, all three types of products must be tested at a single shared testing station for correctness. If they have been manufactured correctly, they are sent on to another part of the facility, leaving our simulation model.

If they were manufactured incorrectly, they must return to the start of the simulation model to be re-processed by their respective machines. If we run the model, we can see that we have a problem at the end of the production run: `Queue2` is full of product which hasn't been tested, and so can't be released. This will cause issues down steam, as we may start to run out of items for later processes to use.

The goal of the investigation is to find where the bottleneck is. Is the testing machine causing the three other machines to back up, or is it being starved because the three machines can't keep up with it? Is the amount of buffer space before the tester important? What decisions can we make to improve matters?

### Learning Outcomes

- Understanding the concept of a _bottleneck_ and the reasons for focusing on this phenomena in process models
- Being able to analyse simple models for bottlenecks, and make recommendations for their resolution
- Being able to create _Dashboards_ to support the gathering of evidence for decision making

### Applying the Model to Different Industries

While we are using the manufacturing industry for this example, the same simulation model can be applied to other industries. Take a copy shop for example. A copy shop has three main services: black and white copies, colour copies, and binding. During business hours, there are three employees working. One employee handles black and white copy jobs, another handles colour copy jobs, and the third handles binding jobs. There is also a cashier to ring up finished orders. Each customer that enters the copy shop gives a job to the employee that specializes in his type of job. As each job is finished, it is placed in a queue for the cashier to finalize the sale and give to the customer. However, sometimes the customer is not satisfied with the job that was done. In such cases, the job must be given back to the appropriate employee to be done again. This scenario represents the same simulation model as the one described above for the manufacturing industry. Here, though, you may be more concerned with the customer queue and the time they spend waiting, as slow service can be very costly to a copy shop's business.

Here's another example of the same simulation model applied to the transportation industry. Commercial shipping trucks travelling over a bridge from Canada into America must go through a customs facility before being allowed to enter the country. Each truck driver must first get the proper paperwork necessary, and then pass through a final inspection of the truck. There are three general categories of trucks. Each category has a different type of paperwork to fill out and must apply at a different department of the customs facility. Once paperwork is finished, all categories of trucks must go through the same inspection process. If they fail the inspection, then they must go through more paperwork, etc. Again, this situation contains the exact same simulation elements as the manufacturing example, only applied to the transportation industry. Here, you may be interested in how far the trucks back up across the bridge. If they back up for miles and thus block traffic into the neighbouring Canadian city, then you may need to change how the facility operates.
