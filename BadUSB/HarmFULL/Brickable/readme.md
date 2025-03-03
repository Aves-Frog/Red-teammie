# Brickable:
Be careful running these scripts in your lab, these (depending on your OS and PC age) may brick your system.

## Windows Explorer series:
This set of scripts focusses on the (temporarily) disabling of Windows Explorer. This will cause some systems to immediately brick, while other systems may immediately stop any file system related proccesses and limit user input. 

To (temporarily) regain control of the system in case of a brick you may use PSExplorerRecover, this script will manually turn explorer back on, this will however only last until the pc is turned off. Nevertheless, this should give you the oppurtunity to recover the system.

The simpelest script is PSExplorerSimple, this simply turns off explorer temporarily, modern systems such as Windows 11 should recover from this by themselves if healthy where other systems may require a reboot. This shouldn't leave any lasting harm to your lab (although it is still your own responsibility), but may cause transfering files to get lost or saving files to corrupt.
