.. _usage:

Usage
-----

This section will describe how to analyse an aerobatic flight using FCScore.


Importing a Flight
==================


In order to analyse a flight it must first be imported into the FCScore client. There are a number of ways that this can be done and all are accessed throught he 'Flight' menu in the top toolbar. 

- Select 'Example' if you don't have any flight logs to hand and just want to see the capabilities of FCScore. 
- Select 'Load' to load flight data from an ardupilot dataflash log (BIN) file and / or a Flight Coach json file.
- Select 'Import' to load a flight from an analysis file previosly exported from FCScore.

If loading flight data from a FC json file or a BIN file there are a few more steps required to prepare the analysis. The flight data loading page contains the tools to do this in two steps:

1 - Load Flight Data and Define the Box.
========================================

The load data page contains three tabs, 'Info', 'Box' and 'Manoevures', which can be selected using the radio buttons near the top right of the screen. You can only access the tabs when they are available. If you load a bin file the box tab will open for you to define the box using the box dropdown. If you load a FC json file the box will be read from that. 

Flight data can also be read straight from an FC json file if you don't have a bin file. It is better to use a bin file if you have it available though as the axis rates and acceleration data can be read from it directly. If using a fc json file these must be calculated by differentiating the velocity and attitude data and this results in a loss of accuracy.

When you are happy with the flight data and box definition click on 'Create States'. This sends the raw flight data and box information to the server, where it is processed and returned in a form that is more suitable for the analysis. 


2 - Split the Flight Into Manoevures.
=====================================

The 'Manoeuvres' will be available once the State data has been generated and contains the tools for splitting the flight up into manoeuvres. If a FC json file was loaded in the last step the manoevure splitting will be loaded from that and you can probably just click 'Setup Analysis' now. Sometimes it is hard to split the flight accurately in the plotter, in that case you can use this page to adjust the manoeuvre splitting. You can also split the flight from scratch here, or load an FC json at this stage to extract the splitting from that.

The first column in the table on the right contains the manoeuvres. The active manoeuvre will have a dropdown that you can open to select the manoeuvre. The second column contains a number which represents the last point of that manoevure in the flight data. The first manoeuvre (Takeoff) is loaded by default, you need to select a manoeuvre in order to show the second column from then on. 

You can adjust the end of the visible range in the 3D plot by sliding the slider, clicking on the ribbon, pressing the 'a' and 'd' keys on your keyboard. 

The idea is that you adjust the end point of the active manoeuvre in the 3D plot, so that only the active manoeuvre is visible, then click on the cell in the table to set the end point. You can also press the 'S' key on your keyboard. 

If you start from the start of the flight and work your way through the next manoevure from the sequence will be selected automatically. You don't have to fly all the manoeuvres in order though, you can even choose manoeuvres from different sequences or disciplines to practise in the same flight. Only the valid manoeuvres (no breaks, takeoffs etc) will be passed on the the analysis stage. 

The competition toggle switch on the top right of the page indicates how a flight will be processed. If its a competition flight it must start with a takeoff, then contain all the manoeuvres from a sequence without breaks, then end with a landing. If you don't follow this rule it should change to a training flight automatically. For a compeition flight the direction the sequence was flown in (left to right or right to left) is defined by the direction of the first manoeuvre, so you'll be zeroed for the rest of the flight if you exit a manoeuvre in the wrong direction and don't fix it within the next manoeuvre. If its a training flight the direction is defined by the start of the manoeuvre being analysed, so you can practise the same manoeuvre in both directions during the same flight and see the scores for both.

When you are happy with the manoeuvre splitting click on 'Setup Analysis' to move on to the next stage.




