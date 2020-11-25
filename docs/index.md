## Project Overview

In 2008 I started building a mechanized sand table that was directly inspired by the artwork of
[Bruce Shapiro](http://www.taomc.com).
My goal was to turn his artistic medium into a functional piece of furniture that could serve as both
an input and output device for networked information. This project was developed as part of
my Master's degree in the Arts Computation Engineering program at the University of California, Irvine.

<div style="text-align: center"><iframe src="https://player.vimeo.com/video/52132828?title=0&byline=0&portrait=0&color=ff9933" class="banner" width="500" height="281" frameborder="0" scrolling="none" webkitAllowFullScreen mozallowfullscreen allowFullScreen></iframe></div>

I started building my project from pieces of a large-format printer, without any knowledge of
<abbr title="Computer Numerical Controlled">CNC</abbr> devices. Based on a
few images I had seen of Sisyphus tables, I started building a radial plotter design. My approach
required a slip ring and unfortunately
I wasn't able to fabricate or source one at that time. My attention turned to finishing the written portion of
my thesis, and I never completed my table.

<hr />

I kept my incomplete work, always hoping to have the opportunity to resume the project. Between 2008 and 2018
many advances were made in electronics, noteably 
[Arduino boards](https://www.arduino.cc/en/main/boards) and the [Raspberry Pi](https://www.raspberrypi.org/products/),
as well as knowledge-sharing platforms like [YouTube](https://youtube.com).

In 2016 Bruce ran a very successful [Kickstarter](https://www.kickstarter.com/projects/1199521315/sisyphus-the-kinetic-art-table)
campaign to turn his Sisyphus table into a consumer-oriented furniture piece. I actually didn't learn about the Sisyphus Kickstarter campaign until May of 2018. It motivated me to start
looking into what it would take to resume my project and that's when this iteration of the project was born.

**Goals For This Project:**

 [x] Build a working plotter into my existing table
 [x] Create algorithmic sand patterns and open source the code used for creating them
 [ ] Create a public web site that allows visitors to send patterns to the table to be drawn
 [ ] Link static and/or dynamic patterns to represent dynamic data sources (part of the original vision for [Gravitable](https://markroland.com/portfolio/gravitable))
 [x] Add LED lighting to the table
 [x] Share my research and development process along the way

Since resuming my work I have found many other sand table builds, and if there's one thing that's certain,
it's that there is no single way to build one. Everyone has access to different budgets,
supplies and tools. I've seen tables made from 3D printed parts and
others made from balsa wood and hot glue. With limited access to machining tools, it's important for my project
to use as many pre-fabricated components as possible. Additionally, I prefer to use supported and well-documented
components over inexpensive, undocumented ones, so I have prioritized those over cheapers options.


### Components

#### Plotting Mechanism

In my research I've come across 3 main different types of plotting mechanisms: XY/Rectangular/Cartesian, Radial/Polar, SCARA.

The Sisyphus table by Bruce Shapiro uses a polar plotting mechanism which they call the [Sisbot](https://sisyphus-industries.com/about/).

The [Sandsara table](https://www.kickstarter.com/projects/edcano/sandsara/description) by Ed Cano uses a [SCARA](https://en.wikipedia.org/wiki/SCARA) mechanism.

My build uses an XY Plotter, primarily because I have very limited machining resources. In fact I was able to build
mine with only a few cuts through extruded aluminum beams.

Within the XY plotter family there are some options for drive configurations. I used a [belt-drive](https://openbuildspartstore.com/v-slot-nema-17-linear-actuator-bundle-belt-driven/)
for the X-axis and a [lead screw](https://openbuildspartstore.com/v-slot-nema-17-linear-actuator-bundle-lead-screw/)
drive for the Y-axis. I made this choice because I came to the build with no experience and wanted
to try out each type of linear actuator.

Here's a great [Instructables project](https://www.instructables.com/id/Low-cost-linear-actuator/_) for creating a DIY linear actuator.

Another XY plotter configuration uses the [Core XY](https://corexy.com/index.html) layout. The primary advantage
to this design is that the motors are mounted in such a way that they aren't required to move, resulting in the ability
to rapidly move the plotting head.

 - [Core XY design ideas](https://openbuilds.com/builds/printair-corexy.2718/) on Open Builds

##### Stepper Motors

Behind every design that I've come across is the the use of high-precision [https://en.wikipedia.org/wiki/Stepper_motor](Stepper Motors).

For now, here are some great introductory articles on Stepper Motors. However, in the end you will want to use a Motor Driver
so that you aren't directly coding the voltage level changes required to drive the motors.

 - [Arduino Stepper Library Tutorial](https://www.arduino.cc/en/Tutorial/StepperOneRevolution)
 - [Concepts by Tom Igoe](http://www.tigoe.com/pcomp/code/circuits/motors/stepper-motors/)
 - [Driving a Stepper by Adafruit](https://learn.adafruit.com/all-about-stepper-motors/driving-a-stepper)
 - [Stepper Motor Quickstart Guide by Sparkfun](https://www.sparkfun.com/tutorials/400)
 - [Misconceptions about Stepper Motors Explained](https://www.machinedesign.com/motorsdrives/misconceptions-about-stepper-motors-explained)
 - [Controlling Stepper Motors using Python with a Raspberry Pi](https://medium.com/@Keithweaver_/controlling-stepper-motors-using-python-with-a-raspberry-pi-b3fbd482f886)
 - [How to Run a Stepper Motor with an Arduino + L293D IC](https://www.youtube.com/watch?v=hZNF7tAJmfk)
 - [Step Motor Basics](https://www.geckodrive.com/support.html)
 - [Step Timing](https://www.embedded.com/design/mcus-processors-and-socs/4006438/Generate-stepper-motor-speed-profiles-in-real-time)

##### Motor Controllers

As mentioned above, stepper motors are great, but they require specific voltage levels, and a motor driving
circuit and firmware simplifies this tremendously.

 - [Big Easy Driver](https://learn.sparkfun.com/tutorials/big-easy-driver-hookup-guide)
    - [Source Code](https://github.com/sparkfun/Big_Easy_Driver)
    - [Manual](http://www.schmalzhaus.com/BigEasyDriver/BigEasyDriver_UserManal.pdf)
    - [The Big Easy Stepper Motor Driver + Arduino](http://bildr.org/2012/11/big-easy-driver-arduino/)
 - [Adafruit Motor Driver Shield](https://www.adafruit.com/product/1438) - Drives 2 stepper motors
 - DQ542MA Stepper Motor Driver
  - [Arduino + DQ542MA Video](https://www.youtube.com/watch?v=IRHB-g4kaS4)
 - [Step Size Calculator](https://blog.prusaprinters.org/calculator_3416/)

Future research: [Arduino AccelStepper Library](http://www.airspayce.com/mikem/arduino/AccelStepper/)

##### CNC (Computer Numeric Control)

Now that you have a motorized plotter, you need a way to turn paths into instructions for the motor drivers. This
is where the concept of Computer Numeric Control or CNC comes in.

One of the most universally popular CNC languages is [G-code](https://en.wikipedia.org/wiki/G-code), and this is
what my build uses with the help of a G-code parser specially designed for use with Arduino - [GRBL](https://github.com/gnea/grbl).

- [Official GRBL Site](https://github.com/gnea/grbl)
- [GRBL Overview](http://www.diymachining.com/grbl/)
- GRBL Shield
  - [Using grblShield](https://github.com/synthetos/grblShield/wiki/Using-grblShield)
  - [DIY CNC Controller: How to Setup Your Arduino & gShield](http://www.diymachining.com/diy-cnc-controller-how-to-setup-your-arduino-gshield/)
  - [Arduino CNC Machine with GRBL Shield - Setup Tutorial on YouTube](https://www.youtube.com/watch?v=1ioctbN9JV8)

###### Control Software

Besides the hardware, you'll need a software interface for sending the G-Code file to the CNC machine. During my process
I relied heavily on [Universal Gcode Sender](https://winder.github.io/ugs_website/) for testing, but in order to control
your device remotely from the command line you'll need to write your own software for sending the commands.

- [Which CNC Control Software Should I Use?](https://www.scan2cad.com/cnc/which-cnc-controller-software-should-i-use/)
- [Universal Gcode Sender](https://winder.github.io/ugs_website/)
- [LinuxCNC](http://linuxcnc.org)
- [GRBL Python Script Example](https://github.com/grbl/grbl/blob/master/doc/script/stream.py)

##### Image Printing

Once your machine is built and controllable using a CNC interface, you'll need a path to plot. There's a variety
of tools for this that are covered in this section.

###### Converting Image and Vector Artwork to G-code

- [CNC Guide: Which Files Can I Convert to G-Code?](https://www.scan2cad.com/cnc/which-files-convert-to-g-code/)

####### Vector Artwork

- [Convert your SVG files to CNC cutting paths with this tool](http://jscut.org)
- [PyCam - STL, DXF, SVG Files to G-Code](http://pycam.sourceforge.net)
- [CNCJS - A web-based interface for CNC milling controller](https://cnc.js.org)

- [G-Code Q'n'dirty toolpath simulator](https://nraynaud.github.io/webgcode/)
- [Inkscape Gcodetools plugin](https://www.cnc-club.ru/forum/viewtopic.php?t=35)
- [An Intro to G-code and How to Generate It Using Inkscape](https://www.norwegiancreations.com/2015/08/an-intro-to-g-code-and-how-to-generate-it-using-inkscape/)

- [Image to Vector to G-CODE Playlist on YouTube](https://www.youtube.com/playlist?list=PLpsggAEL6ib3Sv_-TQCD0YMOku2ia47uD)

####### Bitmap (Pixel) Images

- [Convert JPG to G-Code](https://www.scan2cad.com/cnc/convert-jpg-to-g-code/)


#### Sand Table Interface

##### Magnet

 https://www.kjmagnetics.com/selectasize.asp
 
 - [DE8](https://www.kjmagnetics.com/proddetail.asp?prod=DE8)

 - Mounting/Cup Magnets (https://www.kjmagnetics.com/blog.asp?p=mounting-magnets)

##### Sand

 - Type
   - Shuffle Board Wax (https://robdobson.com/2017/02/a-line-in-the-sand/#comment-48467)
   - https://www.sandtastik.com

     Weight: 1.465 oz per volumetric oz.


   - Baking Soda (https://www.v1engineering.com/forum/topic/does-this-count-as-a-build/page/5/#post-37253)

 - Depth

    - Area is PI * (25.5/2)^2) = 510.71 square inches
    - Depth of 0.25" = 510.71 * 0.25 = 127.68 cubic inches = 70.75 fluid oz.

##### Steel Ball

 - 1/4"
 - 1/2"
 - 3/4"
 - 1"

##### Lighting

###### Adafruit Dotstar LED Strip

 - https://learn.adafruit.com/circuitpython-essentials/circuitpython-dotstar
 - https://learn.adafruit.com/adafruit-dotstar-leds/python-circuitpython
 - https://learn.adafruit.com/circuitpython-on-raspberrypi-linux/installing-circuitpython-on-raspberry-pi


#### Original Drawing Creation

##### Pattern Generators

- [My Own Sand Table Pattern Generator](https://github.com/markroland/sand-table-pattern-maker)
- [Sandify](https://jeffeb3.github.io/sandify/)
- [Sisyphus for the Rest of Us](https://github.com/markyland/SisyphusForTheRestOfUs) and [here](https://www.reddit.com/r/SisyphusIndustries/comments/83ryo8/sisyphusfortherestofus_is_ready_instructions_in/)
- [JSisyphus](https://github.com/SlightlyLoony/JSisyphus)
- [Python Turtle](https://docs.python.org/3.3/library/turtle.html)


Line Drawings
 - http://sunnybala.com/2018/09/10/python-etch-a-sketch.html
 - https://create.arduino.cc/projecthub/iot_lover/arduino-drawing-via-web-using-step-motor-controller-cb5f33
 - https://mathematica.stackexchange.com/questions/133280/draw-image-as-a-continuous-line-drawing

 - Travel Salesman Problem (TSP) Art
  - http://www2.oberlin.edu/math/faculty/bosch/making-tspart-page.html

Patterns
 - https://en.wikipedia.org/wiki/Parametric_equation
 - http://mathworld.wolfram.com/Spirograph.html
 - http://www.mathematische-basteleien.de/spirographs.htm
 - [Hypotrochoid](https://www.openprocessing.org/sketch/649561)
    - https://www.openprocessing.org/sketch/677658
    - https://c.ymcdn.com/sites/www.amatyc.org/resource/resmgr/Summer_Reading_2015/Hypocycloids-Sept2014.pdf
    - http://math.hws.edu/lasseter/teaching/S14/CPSC120/assign/hw5.html

2D Curves - http://www.2dcurves.com

50 Famous Curves  - https://elepa.files.wordpress.com/2013/11/fifty-famous-curves.pdf
Curve Family Tree  - http://xahlee.info/SpecialPlaneCurves_dir/Intro_dir/familyIndex.html#Curve%20Family%20Tree

https://www.desmos.com/calculator/qf1zfdjewu
https://www.desmos.com/calculator/3plby3pgqv


Cycloids/Roulettes/Guilloches 
http://2008.sub.blue/blog/2008/10/10/guilloche.html
https://www.cnccookbook.com/guilloche-rose-engines-jeweling-engine-turning-artistic-machining/

Drawing Machine Simulator
https://krazydad.com/blog/2015/07/12/cycloid-drawing-machine-simulation/ 
https://wheelof.com/sketch/


Rotate a Sine wave
https://math.stackexchange.com/questions/852530/whats-the-intuition-behind-the-2d-rotation-matrix
https://processing.org/discourse/alpha/board_Contributions_Beyond_action_display_num_1112719128.html


### Other Documented Builds

Throughout my build I've found many other sand tables documented across the Internet. I'll list these here at the
top of my documentation as a jumping-off point.

 - [Sandsara Table by Ed Cano](https://www.kickstarter.com/projects/edcano/sandsara)
 - [V1 Engineering Zen XY](https://www.v1engineering.com/zenxy/)
 - [Mark Rehorst's Build](https://drmrehorst.blogspot.com/2018/10/a-3d-printed-sand-table-spice-must-flow.html)
   - [Open Builds Forum Post](https://openbuilds.com/builds/the-spice-must-flow-a-corexy-sand-table.7807/)
 - [Michael Dubno's Build](http://dubno.com/sandtable/index.html)
 - [Rob Dobson's](https://robdobson.com/2017/02/a-line-in-the-sand/)
 - [Sand Plotter by The Mechatronics Guy](https://tinkerings.org/2016/07/21/my-coffee-table-is-a-robot-the-sand-plotter/)
 - [MakrToolbox Build](https://www.instructables.com/id/Zen-Garden-CNC-End-Table/)
 - [Always Tinkering](https://alwaystinkering.com/2020/01/14/diy-kinetic-sand-art-table/)
- [Arduino Sand Table](https://blog.arduino.cc/2018/12/27/create-mesmerizing-designs-in-the-sand-with-this-arduino-controlled-zen-table/)

### Forums

 - [Sandify discussion on V1 Engineering](https://www.v1engineering.com/forum/topic/does-this-count-as-a-build)
 - [Sisyphus Industries on Reddit](https://www.reddit.com/r/SisyphusIndustries/)

### Videos

**Here's a playlist of sand table videos I've discovered throughout my research**
<iframe width="560" height="315" src="https://www.youtube.com/embed/videoseries?list=PLvB5haKJbU3gAIbd7pgwSGlissy1iIUOg" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

- [Sand Table Playlist](https://www.youtube.com/playlist?list=PLvB5haKJbU3gAIbd7pgwSGlissy1iIUOg)

### Other Related Projects

- [Sahara Table](https://saharatable.com)
- [Mechanised Chess Board](https://github.com/2083008/GhostChess)
- [iBoardbot](https://www.jjrobots.com/the-iboardbot/)
- [Simon Hallam's Zen Table on Kickstarter](https://www.kickstarter.com/projects/fnbrit/zen-table)
- [Python Etch-a-sketch](http://sunnybala.com/2018/09/10/python-etch-a-sketch.html)
- [Arduino networked drawbot](https://create.arduino.cc/projecthub/iot_lover/arduino-drawing-via-web-using-step-motor-controller-cb5f33)
- [CoreXY Drawbot build](http://www.arnabkumardas.com/cnc.html)
- [Laser Engraver Build](https://www.instructables.com/Low-Cost-Reliable-Powerfull-Laser-Engraver/)
- [Arduino CNC Projects](https://create.arduino.cc/projecthub/projects/tags/cnc)

### Contact
[Send me a message](https://markroland.com/contact)
