# KiCad-10

Mine bidrag til KiCad 10

## Reset to default Setup

To reset KiCad 10 to its default factory settings, you must close the application and 
clear its global configuration directory. KiCad automatically generates a fresh set of 
default files the next time you open it.

* Follow these steps based on your operating system:
  1. Close KiCad 
     * Make sure KiCad is completely closed before proceeding.
  2. Locate and Clear the 10.0 Configuration Folder
     * Navigate to the directory where KiCad 10 stores its settings.
     * Delete or rename the 10.0 folder to fully reset the application:
     * Windows:
       1. Press Win + R, type %APPDATA%\kicad\, and hit Enter.
       2. Delete or rename the 10.0 folder.
     * Linux:
       1. Navigate to ~/.config/kicad/.
       2. Delete or rename the 10.0 folder.
     * macOS:
       1. Navigate to ~/Library/Preferences/kicad/.
       2. Delete or rename the 10.0 folder.
     * (Note: Renaming the folder to something like 10.0_backup is recommended if you want
    to keep a copy of your custom settings or hotkeys).
  3. Restart KiCad
     * Open KiCad 10. A configuration setup prompt will appear. Choose the option to start with default settings rather than importing configuration data from an older version.

Resetting Only the Libraries (Alternative)

* If you only need to fix broken global symbol or footprint libraries without wiping your entire preferences profile, you can reset them directly within KiCad:
  1. Open KiCad and go to Preferences -> Manage Symbol Libraries (or Manage 
     Footprint Libraries).
  2. Click the Reset Libraries button at the bottom of the dialog window.
  3. Choose the option to Copy default global library table.
  
  For more details on managing initial configurations, check out the official KiCad 10 
  Documentation.
  
  If you are experiencing a specific issue that prompted this reset, please share if it involves 
  missing components, corrupted menus, or UI layout glitches so I can provide targeted 
  troubleshooting steps!
