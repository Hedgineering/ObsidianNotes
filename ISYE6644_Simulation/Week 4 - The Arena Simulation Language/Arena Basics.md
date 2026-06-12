You can drag "Blocks" onto your canvas to build units that interact with one another.

### **Navigation**
Scroll - move canvas up and down
Shift + Scroll - move canvas right and left
Click + Drag - make selections
Del - delete selected nodes

### **Left Panel Project Bar**
You can right click in the blank space in the **Project Bar** > Template Panel > Attach to add additional blocks and templates to your project bar

You can click and drag blocks from the project bar (commonly from the "Discrete Processing" template group) to put things  onto the canvas, including **Create, Process, and Dispose** blocks which will be commonly used.

To connect blocks together, you can use the Home > Connections > Connect button
The **Home** tab also includes the **play-head and speed control** in the Run section to play the simulations and control their speed.

You can double click on the blocks to open dialog boxes to modify their behavior, and any text input can be right clicked to use **Expression Builder** for more advanced parameter expressions (e.g. expressions of Normal, Uniform, etc. ) to use as inputs for each block.



## Concepts

### Process Module

A "Process" is a Resource + Queue
#### Seize-Delay-Release

You can set one of the following actions in the Process module:

- **Delay:** Spend time in the Process (e.g self-service)

- **Seize-Delay-Release:** Grab at least one resource (server), spend time getting served, and then free the server for the next customer. If an entity tries to seize and the server isn't free, it must wait in a queue.

- **Seize-Delay:** Grab at least one resource and spend time getting served. A resource must be released later at some point from some other block or it may cause a deadlock and cause a huge line.

- **Delay-Release:** Use a previously seized server for a while, then free him for the next entity to use.

If you do a Seize or Release, a dialog box appears asking which and how many resource(s) you want to Add (seize) or Delete (release)

You need to use a "Resource Spreadsheet" to set the "Capacity" or total number of available resources available in your simulation.