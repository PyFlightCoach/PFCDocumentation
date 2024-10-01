.. _usage:

Usage
-----

This section will describe how to analyse an aerobatic flight using FCScore.

FCScore Versions
================

FCScore is in active development and new releases of the analysis code will have an impact on the scores. The version of the client, server and underlying analysis code are displayed on the front page. We keep the UK server updated with the latest version. If you are running the analysis server locally make sure your version matches the one running on the UK server. It is also possible to run development versions by checking out the code directly from github and running everything locally. 


Importing a Flight
==================
In order to analyse a flight it must first be imported into the FCScore client. There are a number of ways that this can be done and all are accessed throught he 'Flight' menu in the top toolbar. 

- Select 'Example' if you don't have any flight logs to hand and just want to see the capabilities of FCScore. 
- Select 'Load' to load flight data from an ardupilot dataflash log (BIN) file and / or a Flight Coach json file.
- Select 'Import' to load a flight from an analysis file previosly exported from FCScore.

If loading flight data from a FC json file or a BIN file there are a few more steps required to prepare the analysis. The flight data loading page contains the tools to do this in two steps:

1 - Load Flight Data and Define the Box.
****************************************

The load data page contains three tabs, 'Info', 'Box' and 'Manoevures', which can be selected using the radio buttons near the top right of the screen. You can only access the tabs when they are available. If you load a bin file the box tab will be available for you to define the box using the box dropdown. If you also load a FC json file the box will be read from that, but you can still edit it in the box dropdown. 

Flight data can also be read from an FC json file if you don't have a bin file. It is better to use a bin file if you have it available though as the axis rates and acceleration data can be read from it directly. If using a fc json file these must be calculated by differentiating the velocity and attitude data and this results in a loss of accuracy.

When you are happy with the flight data and box definition you can open the manoeuvres tab to split the flight into manoeuvres, or edit the manoeuvre splitting if it was loaded from the FC json file. 


2 - Split the Flight Into Manoevures.
*************************************

The 'Manoeuvres' tab will be available once bin or json file has been loaded and contains the tools for splitting the flight up into manoeuvres. If a FC json file was loaded in the last step the manoevure splitting will be loaded from that and you can probably just click 'Setup Analysis' now. Sometimes it is hard to split the flight accurately in the plotter, in that case you can use this page to adjust the manoeuvre splitting. You can also split the flight from scratch here, or load an FC json at this stage to extract the splitting from that.

The first column in the table on the right contains the manoeuvres. The active manoeuvre will have a dropdown that you can open to select the manoeuvre. The second column contains a number which represents the last point of that manoevure in the flight data. The first manoeuvre (Takeoff) is loaded by default, you need to select a manoeuvre in order to show the second column from then on. 

You can adjust the end of the visible range in the 3D plot by sliding the slider, clicking on the ribbon or pressing the 'a' and 'd' keys on your keyboard. 

Adjust the end point of the ribbon in the 3D plot so that only the active manoeuvre is visible, then click on the cell in the table to set the end point. You can also press the 'S' key on your keyboard. 

If you start from the start of the flight and work your way through the next manoevure from the sequence will be selected automatically. You don't have to fly all the manoeuvres in order though, you can even choose manoeuvres from different sequences or disciplines to practise in the same flight. Only the valid manoeuvres (no breaks, takeoffs etc) will be passed on to the analysis stage. 

The competition toggle switch on the top right of the page indicates how a flight will be processed. If its a competition flight it must start with a takeoff, then contain all the manoeuvres from a sequence without breaks, then end with a landing. If you don't follow this rule it should change to a training flight automatically. For a compeition flight the direction the sequence was flown in (left to right or right to left) is defined by the direction of the first manoeuvre, so you'll be zeroed for the rest of the flight if you exit a manoeuvre in the wrong direction and don't fix it within the next manoeuvre. If its a training flight the direction is defined by the start of the manoeuvre being analysed, so you can practise the same manoeuvre in both directions during the same flight and see the scores for both.

When you are happy with the manoeuvre splitting click on 'Setup Analysis' to move on to the next stage.


Analysing the Flight
====================

Analysis Page
*************

Once a flight has been loaded you will be redirected to the analysis page. You can also access this page by clicking on the 'Analysis' button in 'Flight' menu. The analysis page contains a table showing the manoeuvres, k factors, downgrades and scores. If the analysis has not been run yet then the downgrades will be blank and all the scores will be zero. In some cases scores may be pre-loaded if they were available in the fc json file or analysis export file. 

The scores displayed are for a certain version of the analysis code and if they were loaded from a fc json file or an analysis export file they may not be for the version running currently on the analysis server. This means that if you run them again the scores may change. You can view the available versions by opening the flight menu. If scores are available for the current analysis code running on the server it will be tagged with 'Current'. You can only run the analysis for the current version of the code.

To analyse a manoeuvre click on its 'run' button in the table. This will take a few seconds. You can also select 'Run All' in the flight menu to run all the manoeuvres at once, or 'Run Remaining' to run all the manoeuvres that have not been run yet. The Options menu contains some settings that can be adjusted to change the way the analysis is run and the scores to display. 

The 'Easy', 'Medium' and 'Hard' toggle switches in the options menu work like expo curves on the downgrade. A 6 mark downgrade will always give six marks, but smaller downgrades will be reduced. The difficulty function looks like this:

.. code-block:: python

    def difficulty(val, factor):
        b = 1.3 - factor * 0.1
        m = 6 / 6**b
        return m * val**b

Where the factor is 3 for hard, 2 for medium and 1 for easy. The colours in the table will change colour to reflect the difficulty setting.

The 'Truncate' toggle in the options menu truncates downgrades to the nearest 0.5 points before adding them up. This is an attempt to reflect the behaviour of a real judge only downgrades for large errors and might be useful for entry level competitions.

Element Splitting
*****************

The initial manoeuvre level splitting is performed manually either when the flight is imported or in the flight coach plotter, but a further level of splitting is required in order to analyse the flight. This is performed automatically by the server and some understanding of how it works is useful in order to get the most out of FCScore. 

The first stage of the element level splitting is performed by comparing the manoeuvre to a computer generated template flight. Examples of these templates can be viewed in the flight coach plotter. The template flight was generated, so the exact split locations of all the elements are known. A Dynamic Time Warping algorithm is run based on the roll, pitch and yaw rates to generate a warping path between the flown data and the generated template. This warping path is used to copy the split locations from the template to the flown data. This stage is run twice, first with absolute values of the roll and yaw rates to establish the roll and yaw directions chosed by the pilot, then a new template is generated with the correct roll and yaw directions and the elements scaled to match measurements of the flown data. The DTW algorithm is then run again based on the actual axis rates of this new template.

The first stage of element level alignment only produces an initial guess of the split locations. In order to get the best results the Intra element scores are calculated and a local optimiser is run on each split location in order to minimise the downgrade.

