---
title: "Workflow Automation"
description: ""
lead: ""
date: 2025-12-14T11:07:06+03:30
lastmod: 2025-12-14T11:07:06+03:30
draft: false
images: []
type: docs
weight: 321110
---

## Workflow Editor

The Workflow Editor is the central environment for creating, managing, testing, and executing workflows. Within the `IDmelon Workflow Automation` app, you can design and refine workflows, then export them as JSON files for use with the Workflow Runner.

### Workflows

A workflow is a structured sequence of actions or tasks that are executed step by step to achieve a specific outcome. In automation systems, a workflow typically consists of a list of predefined activities—such as reading data, clicking buttons, entering text, or processing information—that are carried out in order, one after another. Each action depends on the completion of the previous step, creating a logical flow from start to finish.

#### Export a Workflow

To export a workflow in the `IDmelon Workflow Automation` app, select it from the list and click the `Export` button. The workflow will be saved as a JSON file, which you can store on your PC or other systems. This file is required for running the workflow via Accesskey scripts.

### Actions

There are two types of actions:

1. **Element-based actions**: These actions depend on UI elements and operate on a specific element (e.g, clicking a UI element).
2. **Non-element-based actions**: These actions are not tied to UI elements and perform operations directly on the system (e.g, running an application).

Available actions are listed below.

#### Click on UI element

Simulates a mouse click on a specified UI element, allowing the automation process to interact with buttons, links, checkboxes, or other clickable components within the application interface.

**Input Parameters:**

| Argument   | Optional | Accepts                               | Default Value | Description                  |
| ---------- | -------- | ------------------------------------- | ------------- | ---------------------------- |
| Click type | No       | Left click, Double click, Right click | Left click    | The kind of click to perform |

#### Hover mouse over UI element

Moves the cursor over a UI element.

**Input Parameters:**

No Inputs needed.

#### Populate text field

Fills a text field with the entered text.

**Input Parameters:**

| Argument        | Optional | Accepts | Default Value | Description                        |
| --------------- | -------- | ------- | ------------- | ---------------------------------- |
| Text to fill-in | No       | Strings | Empty         | The text to fill in the text field |

> **Note**: This action can be applied only to Inputs.
>
> **Note**: You can use [variables](#variables) instead of static strings. For more details.

#### Focus UI element

Sets the focus on a UI element.

**Input Parameters:**

No Inputs needed.

#### Get details of the UI element

Gets the value of a UI element's attribute.

**Input Parameters:**

| Argument         | Optional | Accepts                                               | Default Value | Description                                     |
| ---------------- | -------- | ----------------------------------------------------- | ------------- | ----------------------------------------------- |
| Attribute name   | No       | UI element Attributes (Id, Name, Class, Enabled, ...) | Id            | The attribute name                              |
| Bind to variable | No       | Variable name (String)                                | Empty         | The variable where the attribute will be stored |

#### Wait for UI element

Waits until a UI element appears on screen.

**Input Parameters:**

| Argument | Optional | Accepts         | Default Value | Description                                                 |
| -------- | -------- | --------------- | ------------- | ----------------------------------------------------------- |
| Timeout  | No       | Number (Double) | 10            | Maximum number of seconds to wait for the element to appear |

#### Send mouse click

Sends a mouse click event.

**Input Parameters:**

| Argument    | Optional | Accepts                               | Default Value | Description                                                         |
| ----------- | -------- | ------------------------------------- | ------------- | ------------------------------------------------------------------- |
| Mouse event | No       | Left click, Double click, Right click | Left click    | Specify what form of mouse event to send                            |
| Delay       | No       | Number (Int)                          | 0             | The time to delay before sending the mouse event (in milliseconds)  |

#### Send keys

Send keys to the UI element that is currently focused.

**Input Parameters:**

| Argument     | Optional | Accepts      | Default Value | Description                                          |
| ------------ | -------- | ------------ | ------------- | ---------------------------------------------------- |
| Text to send | No       | String       | Empty         | The text to send to the focused UI element or window |
| Delay        | No       | Number (Int) | 10            | Delay between keystrokes in milliseconds             |

> To simulate a key press, use curly brackets {} (e.g., {enter}). For key combinations, wrap both keys in {} (e.g., {leftctrl}({A})).
>
> **Note**: You can insert modifiers or special keys using two combo boxes at the bottom of the text field.
>
> **Note:** For the modifiers, the second key must be added inside a ({}) (e.g., {leftctrl}({A}) for the select all shortcut).
>
> **Note:** You can also use variables as a part of the text (See [Variables](#variables)).

Some common shortcuts:

- **Select all**: {leftctrl}({A})
- **Copy**: {leftctrl}({C})
- **Paste**: {leftctrl}({V})
- **Shift Delete**: {rightshift}({delete})

#### Run application

Executes an application.

**Input Parameters:**

| Argument                 | Optional | Accepts                                                      | Default Value        | Description                                                  |
| ------------------------ | -------- | ------------------------------------------------------------ | -------------------- | ------------------------------------------------------------ |
| Application path         | No       | String                                                       | Empty                | The executable file path                                     |
| Command line arguments   | Yes      | String                                                       | Empty                | Extra arguments that would go after the executable file name |
| Working folder           | Yes      | String                                                       | Empty                | The full path of the folder to work out of, if applicable    |
| Window style             | No       | Normal, Hidden, Minimized, Maximized                         | Normal               | The application's window style                               |
| After application launch | No       | Continue immediately, Wait for application to load, Wait for application to close | Continue immediately | Specify whether the next action executes immediately or waits until the program loads or completes |

> If you want to use the **ExitCode** variable for next actions, set the **After application launch** option to **Wait for application to load**.

#### Close window

Close a specific window.

**Input Parameters:**

| Argument         | Optional | Accepts                                 | Default Value        | Description                                                  |
| ---------------- | -------- | --------------------------------------- | -------------------- | ------------------------------------------------------------ |
| Find window mode | No       | By window UI element, By title or class | By window UI element | Specify whether to look for the window using a UI element or a combination of window title/class |
| Window           | No       | UI element (of type Window)             | Empty                | The window UI element                                        |
| Window title     | Yes      | String                                  | Empty                | The window title. Wildcards can be used, like '?' or '*'     |
| Window class     | Yes      | String                                  | Empty                | If there are two windows with the same title, window class might help differentiate between them |

#### Set variable

Set the value of a new or existing variable, create a new variable or overwrite a previously created variable.

**Input Parameters:**

| Argument | Optional | Accepts           | Default Value | Description                                            |
| -------- | -------- | ----------------- | ------------- | ------------------------------------------------------ |
| Variable | No       | String            | Empty         | A previously generated variable or a new variable name |
| Value    | No       | String/Int/Double | Empty         | The value to assign to the variable                    |

#### Increase variable

Increase the value of a variable by a specific amount.

**Input Parameters:**

| Argument      | Optional | Accepts             | Default Value | Description                                                  |
| ------------- | -------- | ------------------- | ------------- | ------------------------------------------------------------ |
| Variable name | No       | String              | Empty         | The numeric variable to increase                             |
| Increase by   | No       | Number (Int/Double) | Empty         | A numeric value, or a previously created variable containing one, to increase the variable by |

> **Note**: Negative numbers can also be used to decrease the value of a variable.

#### Wait

Suspends the execution of the flow for a specific amount of seconds.

**Input Parameters:**

| Argument | Optional | Accepts         | Default Value | Description             |
| -------- | -------- | --------------- | ------------- | ----------------------- |
| Duration | No       | Number (Double) | Empty         | The duration in seconds |

> In addition to numbers, you can also make use of variables (See [Variables](#variables)).

#### Stop flow

Terminates the flow.

**Input Parameters:**

| Argument    | Optional | Accepts          | Default Value | Description |
| ----------- | -------- | ---------------- | ------------- | ----------- |
| With status | No       | Success, Failure | Success       | Status      |

#### Lock workstation

Locks the workstation's display.

**Input Parameters:**

No Inputs needed.

#### Call Accesskey function

Calls an Accesskey function and retrieves its result.

**Input Parameters:**

| Argument      | Optional | Accepts                            | Default Value | Description |
| ------------- | -------- | ---------------------------------- | ------------- | ----------- |
| Function name | No       | Function name defined by Accesskey | Empty         | -           |

> **Note**: To test the function and see the result, after selecting the function and filling the required inputs, click on the **Test** button next to the function's drop-down.
>
> **Note**: A badge (or a card) must be cached during execution. So you need to tap your badge on the reader before testing this action.

#### Run PowerShell script

Executes some custom PowerShell script and retrieves its output into a variable.

**Input Parameters:**

| Argument | Optional | Accepts         | Default Value | Description                                                  |
| -------- | -------- | --------------- | ------------- | ------------------------------------------------------------ |
| Script   | No       | String          | Empty         | The PowerShell code to execute. Variables can be included within the script since they evaluate before the execution of the PowerShell code |
| Timeout  | No       | Number (Double) | 0             | The maximum number of seconds to wait for the script to complete (0 for indefinitely) |

#### Conditional Actions

Conditional actions are workflow steps that execute only when specific conditions are met. They are useful for tasks like checking the value of a variable, verifying whether an element exists on screen, or controlling the flow of automation based on dynamic criteria.

##### If

Check if two values match.

**Input Parameters:**

| Argument       | Optional | Accepts                                                      | Default Value | Description                                                  |
| -------------- | -------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| First operand  | No       | String                                                       | Empty         | Enter a value name defined by a previous action, text, number or expression to compare with the second operand |
| Operator       | No       | Equal to (=), Not equal to (!=), Greater than (>), Greater than or equal to (>=), Less than (<), Less than or equal to (<=), Contains, Does not contain, Starts with, Ends with, Is empty, Is not empty | Equal to (=)  | The relationship of first operand to the second operand      |
| Second operand | No       | String/Number                                                | Empty         | Enter a value name produced by a previous action, text, number or expression to compare with the first operand |

##### Else if

Starts a block of actions that run only if earlier `If` or `else if` checks fail, and this condition is true.

**Input Parameters:**

| Argument       | Optional | Accepts                                                      | Default Value | Description                                                  |
| -------------- | -------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| First operand  | No       | String                                                       | Empty         | Enter a value name defined by a previous action, text, number or expression to compare with the second operand |
| Operator       | No       | Equal to (=), Not equal to (!=), Greater than (>), Greater than or equal to (>=), Less than (<), Less than or equal to (<=), Contains, Does not contain, Starts with, Ends with, Is empty, Is not empty | Equal to (=)  | The relationship of first operand to the second operand      |
| Second operand | No       | String/Number                                                | Empty         | Enter a value name produced by a previous action, text, number or expression to compare with the first operand |

##### Else

Starts a block of actions when none of the preceding `if` or `else if` conditions are met.

**Input Parameters:**

No Inputs needed.

##### If UI element exists

Checking whether a UI element exists on the screen.

**Input Parameters:**

| Argument      | Optional | Accepts              | Default Value | Description                                                  |
| ------------- | -------- | -------------------- | ------------- | ------------------------------------------------------------ |
| If UI element | No       | Exist, Doesn't exist | Exist         | Specifies the condition to check for the UI element’s presence or absence on the screen. |
| Timeout       | No       | Number (Double)      | 2             | Search timeout in seconds                                    |

#### Loops

Loops are workflow actions that repeat a set of steps until a specified condition is met. They are useful for iterating over collections or repeatedly checking values.

##### For loop

Iterates a block of actions for a specific number of times.

**Input Parameters:**

| Argument     | Optional | Accepts      | Default Value | Description                                                  |
| ------------ | -------- | ------------ | ------------- | ------------------------------------------------------------ |
| Start from   | No       | Number (Int) | Empty         | Set the starting point of the loop counter (variables are allowed) |
| End to       | No       | Number (Int) | Empty         | Set the ending point of the loop counter (variables are allowed) |
| Increment by | No       | Number (Int) | Empty         | Set the increment that the loop counter is increased by (variables are allowed) |

> **Note**: You can use variables instead of static numbers. For more details, see [Variables](#variables).

##### Loop condition

Iterates a block of actions as long as a specific condition proves to be true.

**Input Parameters:**

| Argument       | Optional | Accepts                                                      | Default Value | Description                                                  |
| -------------- | -------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| First operand  | No       | String                                                       | Empty         | Enter a value name defined by a previous action, text, number or expression to compare with the second operand |
| Operator       | No       | Equal to (=), Not equal to (!=), Greater than (>), Greater than or equal to (>=), Less than (<), Less than or equal to (<=), Contains, Does not contain, Starts with, Ends with, Is empty, Is not empty | Equal to (=)  | The relationship of first operand to the second operand      |
| Second operand | No       | String/Number                                                | Empty         | Enter a value name produced by a previous action, text, number or expression to compare with the first operand |

##### Next loop

Uses inside a loop to skip the current iteration and jump directly to the next one when certain conditions are met.

##### Exit loop

To immediately terminate a loop when a specific condition is met, stopping any further iterations.

### Variables

Variables are placeholders that store values during workflow execution. These values can be reused across different actions, making workflows dynamic and flexible. To reference a variable in another action, use the format `%variable_name%`, which substitutes the stored value at runtime.

You can see the list of the current generated variables by clicking on the variables icon at the top right corner of the menu items.

#### Define a variable

To define a new variable, select the `Set variable` action from the actions list.

To increase/decrease the value of a numeric variable, select the `Increase variable` action from the actions list.

> **Note**: Variable names are case sensitive.
>
> **Note**: The value of a variable can be updated multiple times in different flows.
>
> **Note**: When setting the value of a variable, make sure that its type is selected correctly according to its use. For example, in loops, the variable must be of type numeric. Otherwise, you will encounter an error when executing the workflow.
>
> **Note**: When choosing a variable name, make sure that you have not created it before; otherwise, it will be assigned the last value set for it and may cause interference.

#### Predefined variables

The variables `CardId` and `UserId` are reserved; one is the ID of the card tapped on the reader, and the other is the username of the card’s owner.

#### Variables scope

To use a variable in an action, it must be defined in the previous flows.

When a variable is defined, it will be alive throughout the workflow execution.

#### Variable types

A variable can be of type string, number, or a JSON string. A variable can be assigned different values during the execution of a workflow.

#### Using variables in actions

To access a variable in an action, use the format `%var_name%`. For example, if you have defined a variable named `username`, and you want to fill it in a text field, select the `Populate text field` action from the actions list, and set the value of the `Text to fill-in` to `%username%`. When the workflow is running, it will be replaced with the actual value of the username.

### UI Elements

A UI element (User Interface Element) is a distinct, identifiable component of a software interface that an automation process can recognize, interact with, and manipulate during execution.
For example, buttons, text boxes, hyperlinks, or images are UI elements.

You can add these elements to the workflow and do actions(like clicking) on them.

To add UI elements to the workflow, click on the **Add UI element** button at the top right of the workflow editor window.

#### UI Element Picker

When you click on the **Add UI element**, the UI element picker window will appear.
You can add UI elements by hovering the mouse cursor over the element. When the red rectangle appears around the element, hold the keyboard **Control** key and click on the element to add it to the elements list.

> Some UI elements contain multiple parts; for example, a button may contain a text element inside it. So make sure that you select the desired element correctly by checking the element name that appears on the top of the red rectangle.

#### Supported UI elements

The supported UI elements are listed below:

- Button
- Input
- Hyperlink
- Check Box
- Combo Box
- Radio Button
- Split Button
- Text
- Image
- Menu
- Menu Item
- Tree
- Tree Item
- Tab
- Tab Item
- List
- List Item
- Data Item
- Header Item

#### UI Element Selector

UI elements are organized in a hierarchical tree structure, where each element is represented as a node with parent–child relationships. The root node typically corresponds to the application window, and all interface components—such as panels, buttons, and text fields—are arranged beneath it in nested layers. To access or interact with a specific element, the automation process must traverse the correct path through the tree, starting from the root and moving step by step down to the target node.

Therefore, in order to find an element properly, it is necessary to set attributes that will help in the process of finding it. For example, to find a browser window, we can find it by searching for the process name and title of that window.

To edit selectors of a UI element, double-click on the element that exists in the elements tree. The element selector window will appear, which contains:

- **Elements**: Contains all intermediate elements—such as window, panels, groups, or parent controls—that lead to the target element.
- **Attributes**: Contains all attributes of the selected element that help in finding the correct element. You can edit the desired attribute's value.

> **Note**: To exclude an element from the search operation, uncheck it.
>
> **Note**: To exclude an attribute from matching during search, uncheck it.
>
> **Note**: If you want to create a workflow that is going to run on other systems, be careful when choosing the selector and attributes so that the values are not dependent on your system.
> For example, when the root view of an element is a window, the title of that window is tab-dependent, so you can uncheck the Name attribute to avoid mismatches.

#### Test selectors

To make sure the selector is set correctly, click the `Test` button at the top of the selector window. If the element is located on the screen, a red rectangle will appear around it.

#### Deleting a UI element

To delete a UI element from the tree, select the element and press the keyboard **Delete** key or select the delete option from the context menu.
To delete all UI elements of a window, select and delete the window.

> **Note**: When you delete an element, all related flows (element-based actions) will be deleted too.

### Flows

A flow is an action with specific parameters and conditions that will be executed one by one when the workflow starts.

You can see the list of flows in the middle section of the workflow editor.

Some commands are defined for flows that you can use by clicking the right mouse button on a selected row or rows.

Below is the list of the available commands:

#### Run from here

To test the workflow from a specific row, select the target row and then select **Run from here**.

#### Moving flows

You can move a flow up or down using the **Move up** and **Move down** commands.

> You can also move one or multiple flows to any rows using mouse drag and drop.
> For some special actions like **Else if**, **Else**, **Next loop**, **Exit loop**, and **End** block, the order of the flows must be respected.

#### Copy-Paste flows

You can create duplicates from a flow using **copy** and **paste** commands (or Ctrl+C and Ctrl+V shortcuts).

> **Note**: For actions that contain variables, make sure variables with the same name do not cause conflicts.

#### Disable/Enable flows

If you want to disable a flow temporarily to prevent it from running, select **Enable/Disable action**.

> **Note**: A disabled flow will be ignored from the exported workflow.

#### Run/Stop workflow

To test the workflow, click on the **Run** button, and to stop it, click it again.

#### Test Selected Action

To test the selected flow, click on the **Test selected action** button.

> **Note**: When you select multiple flows, this event only affects the first selected flow.

## Workflow Runner

Workflow Runner is a tool for running the workflow when the Accesskey Automate Access command triggers.

> The app exists in the installation directory in: **C:\Program Files\IDmelon\Workflow Automation\IDmelonWorkflowRunner.exe**

### Using the Workflow Runner in in the Accesskey script

To run a workflow on the security key presence (card tap) trigger, do the following steps:

1. Export the desired workflow from the Workflow Editor.

2. Make a new text file, fill it with the following lines, and save it as a **.ps1** file.

   ```bash
   # Launches the workflow runner if it's not running
   $processName = "idmelonworkflowrunner"
   if (-not (Get-Process -Name $processName -ErrorAction SilentlyContinue)) {
       Start-Process $processName
   }
   # Runs the workflow on each card tap
   # Timeout is the maximum time (in milliseconds) that the Accesskey will wait for the automation result
   accesskey access-automation automation-app execute automate -p "WORKFLOW_JSON_FILE_PATH" -t TIMEOUT_IN_MILLISECONDS
   ```

3. Enter the following command in PowerShell to activate the automation.

   ```bash
   accesskey config access-automation set --trigger-condition onSecurityKeyPresence --script-path "PS1_SCRIPT_PATH"
   ```

   > To disable the automation, enter the following command in PowerShell:
   >
   > ```bash
   > accesskey config access-automation remove all
   > ```

### Test Workflow

When you configured everything in the previous section, the workflow must be run when you tap your badge on the reader.

## Workflow Runner CLI

Workflow Runner CLI is a tool for running a workflow directly from the command line. This method is usable for the Accesskey script.

### Arguments

| Argument               | Value                      |
| ---------------------- | -------------------------- |
| \-\-workflow-path (-w) | The workflow path          |
| \-\-variables (-v)     | Variables as a JSON string |

To run a workflow using the command line, enter the following command in PowerShell:

```bash
workflowrunnercli -w "WORKFLOW_JSON_FILE_PATH"
#Example
workflowrunnercli -w "C:\workflows\login.json"
```

Pass variables to the workflow as a JSON string using **-v** in the command line.

```bash
workflowrunnercli -w "C:\workflows\login.json" -v "{\"username\": \"xyz\"}"
```

> See [Variables](#variables) for details.

### Using the Workflow Runner CLI in the Accesskey script

You can run workflows directly using the Workflow Runner CLI in the Accesskey scripts. Do the following steps to run the workflow on the badge tap.

1. Export the desired workflow from the Workflow Editor.

2. Make a new text file, fill it with the following lines, and save it as a **.ps1** file.

   ```bash
   workflowrunnercli -w "WORKFLOW_JSON_FILE_PATH"
   ```

3. Enter the following command in PowerShell to activate the automation.

   ```bash
   accesskey config access-automation set --trigger-condition onSecurityKeyPresence --script-path "PS1_SCRIPT_PATH"
   ```

    > To disable the automation, enter the following command in PowerShell:
    >
    > ```bash
    > accesskey config access-automation remove all
    > ```
