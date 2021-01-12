# nodered-screen-debugging

I've noticed a few things about the issue:

1) It's definitely related to pygame. When removing the "import METER.py" line from piplates.TINKERplate, it works. If I then add "import pygame" and "pygame.init()" to the plate_io.py script that Node-RED runs, the issue returns.
2) It's not an error. After the black screen shows up, Node-RED starts just fine and everything works. Additionally, even though the screen is black, if I move the cursor somewhere on the screen where there was a text box/terminal, it changes from a pointer to the insert text mouse.
3) If I hit ctrl+alt+f1 to switch to a terminal, then ctrl+alt+f7 to return to the graphical interface, the black screen is gone and everything works fine.
4) This one is weird. I wanted to make sure the black screen was appearing when pygame.init() was called, and it wasn't something else in plate_io.py that was breaking when pygame initialized it's default modules. So, I removed everything inside of plate_io.py that I could after pygame.init(). When I ran it, instead of getting a black screen, I got the screen for the ctrl+alt+f1 terminal. Despite that, the mouse still behaved as if I was on the screen, and I couldn't type - it was only visual. When I hit ctrl+alt+f1, I could then type, and hitting ctrl+alt+f7 restored everything.
5) After testing that, I realized the black screen is the same thing that pops up when opening an uninitialized terminal screen (anything ctrl+alt+f8 or higher).
