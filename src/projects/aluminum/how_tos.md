(Needs proofreading! Written by: Dhruva (this was copied from stuff in the Aluminum repo))

# How to Make Common Changes

### Displaying custom values on the dashboard

- The NTValuesDisplay widget (lib/widgets/nt_values_display.dart) is used to display different widgets that show things like numbers or booleans in NT.
- There are some useful widgets already added, such as ones which show a number, boolean, string, or a number and change color depending on the value.
- These widgets are passed as a list in the constructor. This is called in lib/screens/main_dashboard.dart. You can search for NTValuesDisplay. The code from the general branch should have good examples.

### Displaying custom status icons on the dashboard

- Status lights are displayed on the right side of the dashboard screen.
- Right now, they're all set up in lib/screens/main_dashboard.dart. This might get moved out to another widget later if it gets complex enough.
- For now just add more widgets to the list of children (should be labeled with a comment saying "Status icons" or something like that)

### Changing the information shown for different states

- Go to lib/widgets/state_bindings.dart
- Edit the map at the top of the file

### Adding to the soundboard

- add the desired sound to the sounds/ directory
- add the name and path to the sound to the list at the top of lib/screens/soundboard.dart

### Adding to the image gallery

- upload image/gif files into images/gallery
- add the file name to the list in the top of widgets/image_gallery.dart
