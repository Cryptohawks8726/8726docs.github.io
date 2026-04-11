(Needs proofreading! Written by: Dhruva)
(TODO: FINISH)

# Aluminum

Aluminum is an app made by the team. It should be kept up to date on all team laptops.
If it is not installed or is out of date, please let someone know or update it yourself.

Aluminum has a variety of utilities and may have more features added in the future.
It can communicate with robots through [NetworkTables](../programming/nt.md), allowing
the app to observe and change data sent from the robot. It can also connect to a
simulated robot.

The app will switch between red and blue themes depending on the robot's current
alliance.

## Using Aluminum

Aluminum will open to the dashboard screen, with a field view and
information displays that would be useful to a driver during matches.
To get to other parts of the app, click on the logo in the bottom
right and select a page to go to. The auto selector is also located
here, which is used to set the auto routine the robot will run if it
is activated in the autonomous period. You can find the version number
at the bottom of this panel.

Above the logo button is an expand button. Clicking on this will toggle between
the window being docked above the driver station window. When the
[driver station](../software/ds.md) is open (preferably in its docked mode at the bottom of the screen)
and this button is clicked, the app will automatically resize itself to take up all of the
remaining screen area above the driver station and the window bar above it will disappear.
Clicking the button again returns the window to its normal state.

### Settings

Aluminum has some settings you may need to change. Settings can also be saved to a
JSON file and loaded onto another computer if needed.

> [!Important]
> Make sure to click the "Save Changes"
> button in the top right after making any changes!

Aluminum has several settings which must be set correctly to connect to the robot. The default settings
should work for a real robot. The team number and port must be set correctly - the default port used for
NetworkTables is 5810, and our team number is (obviously) 8726. If the "Use server name instead of team
number when connecting" option is enabled, instead of attempting to connect to 10.TE.AM.1:PORT, the app
will attempt to connect to the specified port at whatever address you put in the server name field.
This can be used to connect to a simulated robot by connecting to
["localhost"](https://www.geeksforgeeks.org/computer-networks/what-is-local-host/).

Cameras are also configured from settings. Aluminum can display any number of MJPEG streams on the dashboard
screen. The IP address of each stream must be set to the correct IP or you will not see anything
on the dashboard. See [the page on cameras](../parts/cameras.md)
for more information about where to find these IPs.

### The Dashboard

The dashboard is the first screen you see in Aluminum and has several pieces of
information. The live feed from any connected cameras is shown on the left.
On the right, you can find the match number, game timer, and alliance.
There is also a field view, which shows the robot's position on the field and has
a button to reset the robot's gyro if needed. Information about the robot's current state
and specific values can be displayed below. Finally, there is a network connection
indicator in the bottom right which will turn green if connected to a robot.

### The Debug Panel

### Testing Motors

Aluminum provides an interface to make working with the MotorTester code
easier. For more information on this feature see [the section on MotorTester](motortester.md).
