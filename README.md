# nodered-screen-debugging

My progress on the issue so far:

1) When running Node-RED with node-red-contrib-pi-plates a full installation of raspbian, a black screen appears.
2) I wanted to see if it was related to pygame (it's a graphical issue) so I adjusted plate_io.py to remove the TINKERplate dependency (the only plate that uses pygame). The black screen didn't appear. However, the structure of TINKERplate was also different. To verify that wasn't the issue, I removed the "import METER.py" line from piplates.TINKERplate, keeping the TINKERplate functionality but removing pygame. Once agian, the black screen doesn't appear. If I then add "import pygame" and "pygame.init()" to the plate_io.py script that Node-RED runs, the issue returns.
3) pygame works just fine normally.
4) It's not an error. After the black screen shows up, Node-RED starts just fine and everything works. It can be accessed from another computer. Additionally, even though the screen is black, if I move the cursor somewhere on the screen where there was a text box/terminal, it changes from a pointer to the insert text mouse.
5) If I hit ctrl+alt+f1 to switch to a terminal, then ctrl+alt+f7 to return to the graphical interface, the black screen is gone and everything works fine.
6) This one is weird. I wanted to make sure the black screen was appearing when pygame.init() was called, and it wasn't something else in plate_io.py that broke when pygame initialized it's default modules. So, I removed everything inside of plate_io.py that I could after pygame.init(). (Leaving the lines "while True:\sleep(2)" to verify the process was still running with ps -aux) When I ran it, instead of getting a black screen, I got the screen for the ctrl+alt+f1 terminal. Despite that, the mouse still behaved as if I was on the screen, and I couldn't type - it was only visual. When I hit ctrl+alt+f1, I could then type, and hitting ctrl+alt+f7 restored everything.
7) After testing that, I realized the black screen is the same thing that pops up when opening an uninitialized terminal screen (anything ctrl+alt+f8 or higher).
8) I restored the plate_io.py script, and I kept getting the ctrl+alt+f1 terminal on node red startup. Only after I restarted the pi did it return to a black screen.
9) Running the same test script that created the black screen in node red on its own, nothing special happened. I wanted to find out what was different about running pygame in node-red compared to running it in a normal terminal. The first (and most obvious one) was that it was running at the same time as node-red. So, I ran a python script that initialized node-red, then slept until it was interrupted. Then, I tried starting up node-red (without the TINKERplate lines). I wanted to see if the screen was because something else in node-red worked incorrectly when pygame modules had been initialized. The black screen didn't appear.
10) The only other differences I can think of between my tests and when it has an error is that it's a child process of node-red and it runs at the same time as node-red startup. I don't know how to test either of these things.
11) (Just to really quickly verify it was pygame, I replated the plate_io.py script with the following:  

import time  
import pygame  
pygame.init()  

while True:  
   time.sleep(1)  

and that was enough to replicate the issue when started by the node-red process).
