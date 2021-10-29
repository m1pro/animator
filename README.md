# Animator Macro
FreeCAD Macro to animate a model.  Run the macro to create an Animator feature python object.  Select the available properties from a list and double click the object to start the animation.  Selected properties will be incremented (or decremented if a negative Step value is chosen) each time through the animation loop.

## Toolbar icon
<img src="Animator.svg" alt="toolbar icon"><a href="Animator.svg">Download</a> the toolbar icon.

## Installation
Not yet available in the Addon Manager.  Install by copying the Animator.FCMacro file into your macro folder.  On first run it will offer to create a file (animator.py).  This new file is needed in order for FreeCAD to be able to load the class definitions needed by the Animator objects when opening a file containing one of the saved objects.  When you uninstall Animator you must manually remove the animatory.py file.

## Properties
Like all feature python objects the Animator objects are controlled largely by editing their properties in the property view.  Some of the boolean properties act as triggers for commands.  When toggling a trigger from False to True the command is executed and the trigger sets itself back to False, awaiting the next trigger.

## Animator section
### Frames (integer)
Default: 100.  This is how many frames / iterations there will be in the animation loop.  Internally a counter begins at 1 and is incremented by 1 each time through the loop.   The animation ends with the counter reaches 100 or if the user double clicks the Animator object or toggles Stop Animation property to True.
### Initial Delay (float constraint)
Default: 0 milliseconds.  This is the initial delay (time between triggering the animation to begin and the animation beginning).  Default is 0, so the animation starts immediately.  (Actually, there is a tiny delay of 50/1000 seconds in order to avoid some error messages appearing related to recomputes happening while properties are still being toggled.)  Use this if there are some things you want to do before the animation starts, such as setting up a screen capture.  This property is in milliseconds, so you need to use, for example, 5000 for a 5 second delay.
### Refresh (boolean toggle)
Default: False.  Toggle this to True in order to execute the Refresh command.  Refresh will find all objects in the current document and list their supported properties in the VariableNNN enumerations.  Supported properties are generally properties that can be modified by adding or subtracting a numerical value to them.  They must also be visible and not readonly with the exception of Spreadsheet properties, which is how we gain access to the aliases, as they are stored as hidden, readonly properties of the Spreadsheet objects.  (A note on spreadsheets: any expressions used in a cell that is being animated are going to be lost and not recovered when the animation is complete.  For all other property types, including named constraints in sketches, the expressions are restored upon completion of the animation.)
### Show Progress (boolean)
Default: True.  If True progress of the animation is shown in the Description / Label2 column of the feature python Animator object.  It is in the form of counter/Frames, for example, 325/400 if the counter is at 325 and there are 400 Frames in the animation.  Toggling this to False might marginally speed things up if you are finding they are going too slowly.
### Sleep (float)
Default: 0.  The amount of time (in seconds) to sleep during each Frame.  Normally you will want to leave this at zero, but if you want to slow down the animation this is a way to do it.  For example, if you want to add 1/4 second to each frame, then use 0.250 as the Sleep property.  (Note: this unit differs from the Initial Delay, which is in milliseconds because one uses Qt the other uses python.)
### Start Animating (boolean trigger)
Default: False.  Toggle this to True to begin the animation once you have selected a property (or multiple properties) to animate.  You can also double click on the Animator object in the tree view to Start Animating and to Stop Animating.
### Stop Animating (boolean trigger)
Default: False.  Toggle this to True to stop the animation or double click the Animator object in the tree view.
### Variable Count (integer constraint)
Default: 3.  Minimum: 0.  Maximum: 100.  This is how many variables to have available for animating properties.  One property per Variable.  (But you can also link multiple properties to one another with FreeCAD expressions, if you like.)  If you change this variable some VariableNNN properties will be created or removed from the Animator object.  If more are created, existing VariableNNN properties will retain their current settings.  Newly created VariableNNN properties will have default values.  (Note: 100 is the maximum, but if you find you need more, you can add more as an expression.  Press = key and enter the number you want in the dialog.)
## Animator Variables section
In this section are the variables.  Each variable will have 4 associated properties: the property to animate, whether to skip some frames, the initial value for the property to be animated, the step (increment/decrement) to be applied each iteration through the frame loop.
### VariableNNN (enumeration)
NNN is a placeholder for a 3-digit number assigned to each variable, beginning with 001.  The enumeration is a list of strings, each in the form of objectname.property.  In some cases a property can have subproperties.  For example, placement properties will have something like: Body.Placement.Base.x. For named constraints in a sketch, an example would be: Sketch002.Constraints.Height.  "Select Property" is the first item in the enumeration, as a prompt for you to select a property.  If the selection is "Select Property", then that VariableNNN is unused during the animation loop.  If all VariableNNN properties are on "Select Property", then an error is displayed and there is no animation loop.  If you cannot find a property in the list it might mean the property or object it belongs to was created after the Animator object was created.  Toggle the Refresh trigger property to True to refresh the property list.  If that doesn't work, then it might mean the property type is unsupported.  You can try adding it to the Supported property in the Settings Advanced section.  It might also be the property is readonly or hidden.  This can be remedied in some cases by using obj.setEditorMode("property name", 0) in the python console where obj is the name of the object an dproperty name is the name of the property.
