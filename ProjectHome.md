Donate via <a href='https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=kubicek%40gmx%2eat&lc=US&item_name=Open%20Source%20developments&currency_code=EUR&bn=PP%2dDonationsBF%3abtn_donate_LG%2egif%3aNonHosted'>Paypal</a>
<a href='https://flattr.com/submit/auto?user_id=bkubicek&url=http://code.google.com/p/grecode&tags=googlecode&category=software'><img src='http://api.flattr.com/button/flattr-badge-large.png'><a />

For many CNC machining applications it might be required to modify the gcode.<br>
<br>
grecode can shift, rotate, mirror, align gcode.<br>
<br>
It is called from the command line. By executing it without parameters, a hopefully usefull help message will be displayed:<br>
<hr />
<b>The most current version is nowadays situated here: <a href='https://github.com/bkubicek/grecode'>https://github.com/bkubicek/grecode</a></b>

<hr />
Usage:<br>
<code>grecode &lt;operation [optional value]&gt;  [-o output_gcode.ngc] [ input_gcode.ngc]  [-g output.gnuplot]</code>
<h3>Operations:</h3>
<ul><li>-xflip<br>
<ul><li>inverts all X coordinates and expressions<br>
</li></ul></li><li>-yflip<br>
<ul><li>inverts all Y coordinates and expressions<br>
</li></ul></li><li>-xyexchange<br>
<ul><li>just replace the X and Y coordinates and expressions. Also I and J of arcs<br>
</li></ul></li><li>-cw<br>
</li><li>-ccw<br>
<ul><li>clockwise or counter-clockwise rotation by 90 degree.<br>
</li></ul></li><li>-rot <i>angle</i>
<ul><li>Counter-clockwise Rotation by free angle in degree. Expressions are not allowed<br>
</li></ul></li><li>-scale <i>factor</i>
<ul><li>Scales the geometry by a factor.<br>
</li></ul></li><li>-shift <i>xshift</i> <i>yshift</i>
<ul><li>Moves into +x +y by the values in mm.<br>
</li></ul></li><li>-align <i>alignx</i> <i>alingy</i>
<ul><li>calculates the bounding box by g1 and g0 moves. Arcs are ignored.  Alignments are min,middle,max for the G1 and G0 total bounding box; cmin,cmiddle,cmax for the G1 bounding box. Also 'keep' is valid for no shift.<br>
</li></ul></li><li>-killn<br>
<ul><li>removes all N Statements<br>
</li></ul></li><li>-parameterize <i>minoccurence</i> <i>variablesStartnumber</i>
<ul><li>This will scan for re-occuring values in X, Y and Z words.  If the occure more often than minoccurence, they will be substituted by variables.  Their numbers are starting from the specified number<br>
</li></ul></li><li>-overlay <i>XPointA</i> <i>YPointA</i> <i>XPointB</i> <i>YPointB</i>  <i>XNewPointA</i> <i>YNewPointA</i> <i>XNewPointB</i> <i>YNewPointB</i>
<ul><li>This will shift and rotate the the gcode so that PointA and PointB move to the new locations.  Distance mismatches beweeen A-B and newA-newB are compensated.<br>
</li></ul></li><li>-knive <delay mm><br>
<ul><li>This should compensate partially for foil cutters, where the cutting point is lagging. The lagging distance should be specified in mm. Arc movements could be problematic currently. The implementation is not very good.<br>
</li></ul></li><li>-copies <i>amountOfCopiesX</i> <i>amountOfCopiesY</i> <i>shiftx</i> <i>shifty</i>
<ul><li>Creates multiple copies of the original code. They are aligned in an n times m grid. Optimal for creating batches of parts. However, End program statements and such should be removed by the -comment option<br>
</li></ul></li><li>-makeabsolut Recalculate paths from relative moves to absolute moves.<br>
</li><li>-comment <i>Word</i>
<ul><li>Comments out words: Example -comment M03 will replace all M03 by (M03)<br>
</li></ul></li><li>-zxtilt <i>angle</i>  or -zytilt <i>angle</i>
<ul><li>shear-transform z values so that the x-y area is tilted do the angle</li></ul></li></ul>

<h3>Input/Output:</h3>
<ul><li>The program reads input from the console and outputs to the console<br>
</li><li>If an input file is specified by -i, it is read instead.<br>
</li><li>If an output file is specified by -o, the output is written there.<br>
</li><li>To facilitate viewing of the gcode, a gnuplot-readable output file can be specified by -g. It can be viewed by gnuplot and entering: splot output.gnuplot u 1:2:3:4 w lp palette</li></ul>


<h1>WARNING</h1>
The outputed gcode needs to be validated by YOU. The software is not perfect, and I am not liable to any harm or damage done by defective gcode!<br>
<br>
<h2>Example usage:</h2>
<ul><li>./grecode -xflip tests/testcode.ngc -o tests/out.ngc<br>
</li><li>./grecode -xflip tests/maxboard_cut.ngc  -o tests/testcut.ngc<br>
</li><li>./grecode -align cmin cmin tests/maxboard_cut.ngc  -o tests/testalign.ngc<br>
</li><li>cat tests/testcode.ngc |./grecode -xflip >tests/out2.ngc<br>
</li><li>./grecode tests/max232_kombi.ngc    -rot 45 -o tests/test.ngc -g tests/test.xy<br>
</li><li>./grecode tests/arctest.ngc -killn -rot 180 -o tests/test.ngc -g tests/test.xy<br>
</li><li>./grecode tests/abstest.ngc -knive 0.1 -o tests/test.ngc -g tests/test.xy