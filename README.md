<h1 align="left">
  <br>
  <img src="./img/hei-en.png" alt="HEI-Vs Logo" width="350">
  <br>
  HEI-Vs Engineering School - DLS / Automation in Development and Production
  <br>
</h1>

Course DLS/ADP


Author: [Cédric Lenoir](mailto:cedric.lenoir@hevs.ch)

# Lab 03, Machine Interface

:warning: There is a constant evolution of the UI, some widgets could have new properties.

**Prerequisites**
[ADP Module 05_Machine Interface](https://github.com/hei-dls-adp/adp-docs/tree/main/ADP_Module_05_Machine_Interface)

**Duration**
2h

In this practical exercise, we will take our first steps with a robot. It is important to keep in mind that we are not working with a commercial robot, but with a prototype robot. This robot is regularly programmed and used in various configurations by different students.

## Table of Contents
[[_TOC_]]

## Check your knowledge

### Function
  
:bulb: In the lab, you will need to add functions like this one. The corresponding JavaScript code is provided for each function.

<div align="center">
<figure>
  <img src="./img/Node_Red_Function.png"
     alt="Image lost: Node_Red_Function"
     width="200"><br>
  <figcaption>Node-RED function</figcaption>
</figure>
</div>

Check if you can insert code for a function of this type:

<div align="center">
<figure>
  <img src="./img/Node_RED_Learning_Function_Hello_World.png"
     alt="Image lost: Node_RED_Learning_Function_Hello_World"
     width="400"><br>
  <figcaption>Node-RED function with 'Hello World!'</figcaption>
</figure>
</div>

```js
// Function Hello World
msg.payload = 'Hello World'
return msg;
```

### Insert json script
:bulb: In the lab you will have to insert/import a portion of json code in your flow.
Check if you are able to import this file from the repository of the lab: `..\adp_lab_03_2026\node_red_base\To Insert in your flows\HelloWorld.json`


---

# First part: at home.
In order to monitor and control the robot using Node-RED, you need first to create a _dashboard_ (using _Dashboard 2_).

> Note: 
> 
> This is a homework for you and needs to be done before passing to the second part of the laboratory!

> Note: In the second part of the laboratory we are going to set up the commands to monitor and control the robot using the _data layer_.

The Dashboard is split into three groups:
1.  A group for the machine UI: _Commands and states_ (monitoring)
1.  A group to control the _axes of the robot_ (to move the robot)
1.  A group for the _gripper_ (allowing the robot to grasp objects)

## Dashboard Page Setup
Changes to be done on your dashboard page:

- Base layout: ``Notebook``
- Add 3 groups:
  - Commands and states
  - Gripper
  - Robot Axes
---

## Commands and states
Replicate the content of the _Command and state_ group similar to the following UI (User Interface):

<div align="center">
<figure>
  <img src="./img/UI_CommandsAndStates.png"
     alt="Image lost: UI_CommandsAndStates"
     width="600"><br>
  <figcaption>Commands and states</figcaption>
</figure>
</div>

> Note: You don't need to build exactly the same UI, but the **functionality** needs to be the same!

### Some Construction Hints
- All widgets have a size of 3x1.
- Buttons payload is ``TRUE.``
- Select widget has value ``true`` for manual mode and ``false`` for auto mode
- ``Select Mode``, ``PackML Mode`` and ``PackML State`` are text widgets.

:bulb: you can find [material icons here](https://pictogrammers.com/library/mdi/). You can choose any icon.

---

## Gripper
Create a similar UI for the _gripper_:

<div align="center">
<figure>
  <img src="./img/UI_Gripper.png"
     alt="Image lost: UI_Gripper"
     width="600"><br>
  <figcaption>Gripper</figcaption>
</figure>
</div>

###  Construction Hints
- Size of SetOpen and SetClose is ``3x1``. Payload is ``true``.
- Size of TimeOut Limit, Limit Open and Limit Closed is ``2x1``.
- Inject 400 once for TimeOut and tooltip is ``Base is 400``.
- Inject 1 once for Limit Open and tooltip is ``Base is 1``.
- Inject 9 once for Limit Closed and tooltip is ``Base is 9``.
- Payload of Enable Gripper is ``true``, ``false`` for Disable Gripper.
- See below for radio buttons settings.

<div align="center">
<figure>
  <img src="./img/RadioButtonSettings.png"
     alt="Image lost: RadioButtonSettings"
     width="300"><br>
  <figcaption>Radio button settings</figcaption>
</figure>
</div>

---

## Robot axes
Create a similar UI for the _Robot axes_:

<div align="center">
<figure>
  <img src="./img/UI_Robot_Axes.png"
     alt="Image lost: UI_Robot_Axes"
     width="600"><br>
  <figcaption>Robot axes</figcaption>
</figure>
</div>

### Construction Hints
- Size of value + label X,Y,Z axis absolute position is ``2x1``.
- Size of sliders is ``4x1``
- Step of all sliders is ``1``, limits are ``-150`` to ``150``. Thumb ``On Drag``, 

---

## Finally
Before you start the lab, your _flows.json_ should display something like that:

<div align="center">
<figure>
  <img src="./img/UI_Robot_Overview.png"
     alt="Image lost: UI_Robot_Overview"
     width="600"><br>
  <figcaption>Robot overview</figcaption>
</figure>
</div>

---

# Second part: in the lab.
As we now have the dashboard on Node-RED ready, we will put in place the communication between Node-RED and the robot in order to monitor and control it.

## First step: Load a basic PLC program into the robot.
The `adp_lab_03_2026\plc_ctrlX\Adp_lab_03_Robot_v_3_6.projectarchive` file contains the PLC program to be run on the ctrlX connected to the robot. This program is already complete and fully functional and needs only be uploaded.

> Note: See lab 01 to download the program to the machine. *If we have time we will do it for you*.

The PLC program allows to:
  - Change the modes and states of the machine.
  - Move the axes with a slider
  - Open or close a gripper. 

In Node-RED we will connect to the PLC program, read/write the properties provided via the _Data Layer_ and monitor and control the robot.

---

# Working with 'Node-RED - ctrlX Automation Package'
You should be able to connect to the machine using the [node-red-contrib-ctrlx-automation](https://flows.nodered.org/node/node-red-contrib-ctrlx-automation) package.

<div align="center">
<figure>
  <img src="./img/node-red-ctrlx-automation-nodes-01.png"
     width="150"><br>
  <figcaption>Nodes to access PLC over Data Layer</figcaption>
</figure>
</div>

## Add 'Alarms' Monitoring
Insert the `DisplayAlarmsflows.json` from `\node_red_base\To Insert in your flows`, in a new group with label alarms, so you can have an overview of alarms and warnings if any.

## Add 'Robot State' Monitoring
Let's start with getting the actual state of the robot. For this, we are going to read the properties `mode`, `time` and `state` on the PLC. 

Use _Data Layer Subscribe_ nodes to receive the actual property value. Browse to `plc/app/Application/sym/PackTag/hevsUI/uiModeMasterCurrent`, then `plc/app/Application/sym/PackTag/hevsPackTime/Date_and_time_string` and `plc/app/Application/sym/PackTag/hevsUI/uiStateMasterCurrent`.

At this point, you need to provide:
- Address: **192.168.0.200**.
- Username: **boschrexroth**.
- PW: **we will give it to you...**.

<div align="center">
<figure>
  <img src="./img/Get_state_mode_and_date.png"
     alt="Image lost: Get_state_mode_and_date"
     width="600"><br>
  <figcaption>Get state mode and date</figcaption>
</figure>
</div>

**TODO:** Deploy the application and check on the dashboard if you are able to receive the `mode` and `state` of the robot. Also the `time ` should continuously update its value.

## Add Commands to Control the Robot 
Now we want to change the mode of the robot between _Manual_ and _Auto_ from our Dashboard. We also want to send the commands _Clear_, _Reset_, _Start_ and _Stop_ to the robot. 

Use the flow below as inspiration:

<div align="center">
<figure>
  <img src="./img/Commands_and_states.png"
     alt="Image lost: Commands_and_states"
     width="600"><br>
  <figcaption>Commands and states</figcaption>
</figure>
</div>

To send the commands to the robot use _Data Layer Request_ nodes with `Method` _WRITE_.

The properties to write on the PLC program:
- `plc/app/Application/sym/PackTag/hevsUI/uiModeManual`
- `plc/app/Application/sym/PackTag/hevsUI/uiModeProduction` (for Auto)
- `plc/app/Application/sym/PackTag/hevsUI/uiCmdStart`
- `plc/app/Application/sym/PackTag/hevsUI/uiCmdClear`
- `plc/app/Application/sym/PackTag/hevsUI/uiCmdReset`
- `plc/app/Application/sym/PackTag/hevsUI/uiCmdStop`

Some code snippets to help you correctly format the message to be sent:

```js
// Function bool8 TRUE
msg.payload = {type:"bool8", value:true};
return msg;
```

```js
// BOOL Select
var newMsg = msg;

if (msg.payload) {
    newMsg.payload = {type:"bool8", value:true};
}
else{
    newMsg.payload = {type:"bool8", value:false};
}

return newMsg
```

```js
// NOT BOOL Select
var newMsg = msg;

if (msg.payload) {
    newMsg.payload = {type:"bool8", value:false};
}
else{
    newMsg.payload = {type:"bool8", value:true};
}

return newMsg
```

---

# Working With the Robot's Axes
## Read Robot's Axes Position
Now we can move on and start working with the robot’s three axes. First, let’s read the current values:

<div align="center">
<figure>
  <img src="./img/Read_Axis_Position.png"
     alt="Image lost: Read_Axis_Position"
     width="700"><br>
  <figcaption>Read axes positions</figcaption>
</figure>
</div>

Connect to: 
- `plc/app/Application/sym/PRG_Unit/emRobot/cmModuleAxis_X/absIsPosition`
- then `plc/app/Application/sym/PRG_Unit/emRobot/cmModuleAxis_Y/absIsPosition`
- and `plc/app/Application/sym/PRG_Unit/emRobot/cmModuleAxis_Z/absIsPosition`

We do not want to display the exact values provided. So we round it using the `round` function provided by the JavaScripts _Math_ package:
```js
// Round axis value
var newMsg = msg;
newMsg.payload = Math.round(msg.payload);
return newMsg;
```

## Set Robot's Axes Position
Now we can use the sliders to adjust the robot's axes:

<div align="center">
<figure>
  <img src="./img/Set_Axes_Positions.png"
     alt="Image lost: Set_Axes_Positions"
     width="900"><br>
  <figcaption>Set axes_positions</figcaption>
</figure>
</div>

You need to subscribe to the actual properties, then write new values provided by the sliders to same properties:

- `plc/app/Application/sym/PRG_Unit/emRobot/lrAbsPosition_X`
- `plc/app/Application/sym/PRG_Unit/emRobot/lrAbsPosition_Y`
- `plc/app/Application/sym/PRG_Unit/emRobot/lrAbsPosition_Z`


```js
// Set value to double
var newMsg = msg;
newMsg.payload = {type:"double", value:msg.payload};
return newMsg;
```

> Note: 
> 
> At this point, if the machine is in _manual_ mode and _execute_ state, it should be possible to move the robot!

---

# Working with an I_Interface
You should be able to connect and activate the gripper using an _interface_.

```mermaid
---
title: I_Gripper
---
classDiagram

class FB_Gripper {
    + InOp : BOOL
    + IsClosed : BOOL
    + IsOpen : BOOL
    + M_EnableDevice(Enable : BOOL) : DINT
    + M_SetOpen(rLimitOpen_mm : REAL, udiTimeOut_ms : UDINT) : DINT
    + M_SetClose(rLimitClose_mm : REAL, udiTimeOut_ms : UDINT) : DINT
}

```

This UML block tells you that a Function Block `FB_Gripper` has 3 properties and 3 methods.

---

## Connect to Gripper's Properties
Now we want to read the actual properties of the gripper:
<div align="center">
<figure>
  <img src="./img/Get_Gripper_Property.png"
     alt="Image lost: Get_Gripper_Property"
     width="800"><br>
  <figcaption>Get Gripper Property</figcaption>
</figure>
</div>

Property `InOp` is located here: `plc/app/Application/sym/PRG_Unit/emRobot/cmModuleGripper/fbI_Gripper/InOp`.

After `InOp`, you can link also `IsClosed` and `IsOpen`, each using this conversion function:

```js
// Get Property
var newMsg = {};
if (msg.payload) {
  //  block of code to be executed if the condition is true
  newMsg.payload = 1
} else {
  //  block of code to be executed if the condition is false
  newMsg.payload = 2  
}

return newMsg;
```

> **Properties** from an external interface point of view are like variables with restricted access for read or write.

> **Methods** are more or less like functions. Here for example, we use methods with arguments to open or close a gripper. The arguments are variables set with limits, see below.

---

## Connect Gripper's Limits
Limits does not send data to the PLC, they store local variables. As we still did not speak about local variables, you can simply **import** `adp_lab_03_2026\node_red_base\To Insert in your flows\TimeAndLimitsflows.json` and link nodes to your widgets.

The flow you import will set internal variables. These internal variables will be used by the methods below.

<div align="center">
<figure>
  <img src="./img/ContextWithLimits.png"
     alt="Image lost: ContextWithLimits"
     width="600"><br>
  <figcaption>Variable inside one flow</figcaption>
</figure>
</div>

:bulb: It is sometimes necessary to refresh the context. Note that these variables, like any message in Node-RED, will only be written when the node in question has received at least one message.

---

## Connect Gripper's Methods

### M_EnableDevice

<div align="center">
<figure>
  <img src="./img/M_EnableDevice.png"
     alt="Image lost: M_EnableDevice"
     width="700"><br>
  <figcaption>M_EnableDevice</figcaption>
</figure>
</div>

We enable the gripper, like a *Power On* or *Power Off* with a method and an argument. The argument is an object with the name of the argument. See code below for

```js
// M_EnableDevice
var newMsg = {};
newMsg.payload = {
    type: "object",
    value: {
        "Enable": msg.payload
    }
}
return newMsg;
```

### M_SetOpen and M_SetClosed

<div align="center">
<figure>
  <img src="./img/M_SetOpen_M_SetClosed.png"
     alt="Image lost: M_SetOpen_M_SetClosed"
     width="700"><br>
  <figcaption>M SetOpen and M_SetClosed</figcaption>
</figure>
</div>

Use the function below. Note that these functions use internal variables for limits for which you have inserted a flow.

```js
// M_SetOpen
var newMsg = {};

newMsg.payload = {
    type: "object",
    value: {
        "rLimitOpen_mm": flow.get('rLimitOpen_mm'),
        "udiTimeOut_ms": flow.get('udiTimeOut_ms')
    }
}
return newMsg;
```

```js
// M_SetClosed
var newMsg = {};

newMsg.payload = {
    type: "object",
    value: {
        "rLimitClosed_mm": flow.get('rLimitClosed_mm'),
        "udiTimeOut_ms": flow.get('udiTimeOut_ms')
    }
}
return newMsg;
```

At this point, we should have a fully functional program to monitor and control the robot! 

Congratulations. You have done it!

---

## Program Validation
We still need to test and document the implemented functionality to be sure that everything is working as expected.

> Note:
> 
>  <b style='color:red;'>A code without test has strictly no value!</b>

**TODO:** Check and validate the functionality of your program using the test protocol below:

|Function|UI done |Linked|Tested|
|--------|---|------|------|
|Set Mode|               |||
|Read Mode|              |||
|Read Time|              |||
|Set Clear|              |||
|Set Reset|              |||
|Set Start|              |||
|Set Stop|               |||
|Read State|             |||
||||
|Set Open|               |||
|Set Close|              |||
|Set TimeOut|            |||
|Set Limit Open|         |||
|Set Limit Closed|       |||
||||
|Enable Gripper|         |||
|Disable Gripper|        |||
|Read InOp|              |||
|Read IsClosed|          |||
|Read IsOpen|            |||
||||
|Read x position|        |||
|Read y position|        |||
|Read z position|        |||
|Set x position|         |||
|Set Y position|         |||
|Set z position|         |||

# Delivery 
At the end of this work, you have to send your flow to the supervisor.

<!-- First steps with a robot-->