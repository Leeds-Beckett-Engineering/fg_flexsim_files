# WC.01: Task Executors

## Introduction

_Workshop WC.00_ introduced the creation of a basic model in FlexSim, and concentrated on the core flow of items through the model. In many real processes, though, we also need to model 'auxiliary' or 'helper' tasks: these don't contribute directly to the item flow, but are needed for the overall process to work correctly. Examples might be good that are transported from one part of the factory to another: the items are moving, but you also need a transport to _do_ the moving.

In FlexSim, these 'helper' tasks are added through a secondary layer of connections. We will look in _Workshop WC.01_ specifically the the `Operator` and `Transporter` entities, as these are the most common. However the general concept of a `Dispatcher` and of central ports is something you will see in most realistic model.

_Workshop WC.01_ also introduces additional graphical and statistical output options, and begins the task of preparing to export useful data from the model. We will look at data export in more detail later, but the concept of a _data dashboard_ introduced here is used more widely to explore and fine-tune models within FlexSim itself.

**Note:** Make sure you have completed _Workshop WC.00_ before starting _Workshop WC.01_, as _Workshop WC.01_ will use the model from _Workshop WC.00_ as a starting point. You should have something that looks like @fig:WC01_Model1 **before** you begin.

### Learning Outcomes

- The use of the 'centre' ports in a FlexSim model (and how these differ to the 'normal' connectors from _Workshop WC.00_)

- How, and why, the `Dispatcher`, `Operator`, and `Transporter` objects are used within a model

- How to access and modify object properties

- How to view object statistics while the model is running

### New Objects

In this Workshop you will be introduced to the `Dispatcher`, `Operator`, and `Transporter` objects.

### Approximate Time to Complete this Lesson

This Workshop should take about 30--45 minutes to complete.

