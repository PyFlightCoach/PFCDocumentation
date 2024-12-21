.. _fcscore:

FCScore
=======

FCScore is a tool for automatic judging of precision aerobatics. You can access FCScore from the following link:

https://pyflightcoach.github.io/FlightCoachScore/

The documentation provided here is supplementary to the help available by clicking on the question mark at the top of the page and provides more insight into the way the scoring process works and how to use the tool effectively.


Difficulty and Truncate Settings
********************************

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

The initial manoeuvre level splitting is performed manually either when the flight is imported or in the flight coach plotter, but in order to judge the flight each manoeuvre must be split up into elements. This is performed automatically by the server and some understanding of how it works is useful in order to get the most out of FCScore. 

The first stage of the element level splitting is performed by comparing the manoeuvre to a computer generated template flight. Examples of these templates can be viewed in the flight coach plotter. The template flight was generated, so the exact split locations of all the elements are known. A Dynamic Time Warping algorithm is run based on the roll, pitch and yaw rates to generate a warping path between the flown data and the generated template. This warping path is used to copy the split locations from the template to the flown data. This stage is run twice, first with absolute values of the roll and yaw rates to establish the roll and yaw directions chosen by the pilot, then a new template is generated with the correct roll and yaw directions and the elements scaled to match measurements of the flown data. The DTW algorithm is then run again based on the actual axis rates of this new template.

The first stage of element level alignment only produces an initial guess of the split locations. In order to get the best results the Intra element scores are calculated and a local optimiser is run on each split location in order to minimise the downgrade. 

All this happens when a manoeuvre is analysed for the first time for a given analysis version and generally the correct result is achieved, or at least one that is within a few tenths of a point of the correct result. In this case correct is defined as the kindest score the judging algorithm can give for a manoeuvre. 

In some cases the process described above can fail, in this case you will almost certainly receive a zero for the manoeuvre, or at least a much lower score than you were expecting. This happens when the initial alignment process fails and the local optimisers get stuck in local minima from that point. 

We are treating the multivariant global optimisation problem as lots of single variable local optimisations, which is usually a reasonable assumption as the initial guess from DTW should be close to the final result. Sometimes it isn't though and it gets stuck in a local minima. In this case you need to give it some help on the alignment tab. You can either adjust the splits and press score to just calculate the scores with the new splits, or press optimise to start the local optimisation from the new initial position. 

