# How to edit SoundShift's executable

Download dnSpy from https://github.com/0xd4d/dnSpy/releases - grab the net472 version or similar. Extract it to a folder.  
Open dnSpy-x86.exe.  
Once the program loads, File -> Open... the SoundShift.exe.  
The program will be decompiled.  
Browse the tree to the following:  
* Sound Shift (1.0.5828.30520)  
    * Sound Shift.exe  
        * Sound_Shift  
            * MainForm @02000004  
                * .ctor() : void @0600000F  
   
Scroll to line 54. The line should read:

`Array.Sort<MainForm.Vehicle>(this.carData, (MainForm.Vehicle a, MainForm.Vehicle b) => a.Alias.CompareTo(b.Alias));`

Right-click and select "Edit Method (C#)..." This will bring up an editable window.  
Replace the contents of line 60 with the following:

`Array.Sort<MainForm.Vehicle>(this.carData, (MainForm.Vehicle a, MainForm.Vehicle b) => string.CompareOrdinal(a.Alias, b.Alias));`

If you want to change the title of the window to make a note that you have modified the file, two lines above is where the title bar is set.  
**IMPORTANT**: At this point, delete the line that reads `base..ctor();` (line 56). Without this, the code will not compile, since it tries to reconstruct itself in the constructor.  
Click 'Compile'.  

Right-click the item at the top of the tree that reads Sound Shift (1.0.5828.30520), and click Edit Assembly...  
Change the final number in the version to a greater value than 30520. This provides an extra way of checking that your modifications worked, by checking the file properties.  
Click OK.  

Finally, click File -> Save Module...  
Pick a filename (it's a good idea to not overwrite the original SoundShift EXE), and click OK.  
Congratulations, you have successfully modified the Sound Shift program sort method.  

While I was here, I also chose to modify the checkBoxGears.Checked state in the InitializeComponent() method to `false`, so that this would be the default.  
SoundShift only loads data about your settings from the vehicles.ini once per car, after that it always reads your modified settings from the registry. These include the status of gear count, beep enable and simple RPM mode. However, if you have the gear count enabled, it will automatically save that on a car-by-car basis to the registry. The car names, however, are always read from the vehicles.ini.
