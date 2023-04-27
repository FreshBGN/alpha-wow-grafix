# Alpha WoW Grafix - A repository of various fixes and instructions for fixing graphical issues and improving FPS in WoW 0.5.3
The graphics APIs used in World of Warcraft Alpha 0.5.3.3368 have various issues and incompatibilities with modern graphics cards that vary based on GPU brand, model, and driver version. This repository aims to be a common resource containing fixes and instructions for as many different such issues as possible. More fixes will be added over time as different graphics cards are tested.
***
Automatic scripts now available. Read the "Prerequisites" and "Requirements" sections below, then get the latest release from our [releases tab](https://github.com/FreshBGN/alpha-wow-grafix/releases).
***
Special thanks to:

-The Alpha Project, for getting me interested in exploring the World of Warcraft alpha, and for providing an amazing emulator for 0.5.3.

-Lenny, for working with me on testing out many of these fixes on his Nvidia graphics card, as I do not own one. Thanks, man!

## Prerequisites
-Make sure you're using the modded WoW client before applying any of these fixes. The modded client can be found [here](https://anonfiles.com/fbi9C0M0y9/Mods_zip), or use this [mirror](https://cdn.discordapp.com/attachments/653374433636909077/1089350731909308466/Mods.zip). Simply replace the files in your WoW alpha root folder with the modded ones from the archive.

-Update your graphics drivers to the latest version! Driver download pages are [here](https://www.nvidia.com/download/index.aspx) for Nvidia, [here](https://www.amd.com/en/support) for AMD, and [here](https://www.intel.com/content/www/us/en/download-center/home.html) for Intel.

-If you've tried other methods of improving FPS or fixing graphical issues/crashes, such as modified opengl/d3d drivers (e.g. dxgi.dll, d3d9.dll, opengl32.dll), remove all of them from the game folder before proceeding.

## Requirements for the automatic scripts
-The batch file must be put in your wow root folder (e.g. in my case C:\\Users\\\<user>\\Downloads\\WoW 0.5.3) before running it.

-Make sure your wow root folder is in a location that does not require administrator privileges (unless you know how to PROPERLY run batch files as an administrator).

-WinRAR OR 7-Zip installed in "C:\Program Files" (depending on the version of the batch file you choose).

<b>NOTE:</b> If your WinRAR or 7-Zip is installed in a different location, you may edit the batch file with Notepad and change JUST the path to WinRAR/7-Zip in the "PATH=" line.

##  Running in OpenGL mode on unsupported hardware
Modern graphics cards have various issues with old OpenGL versions. On AMD, textures and models will often be stretched, flickering, and generally broken. On Nvidia, the game might not even start in OpenGL mode. Below are instructions on how to run OpenGL mode by translating the API calls to a different API (such as DX12 or DX9).
***
### For AMD graphics cards:
This method uses the publicly available <b>Mesa</b> driver to translate OpenGL calls to DirectX 12, fixing most of the graphical issues encountered when using OpenGL on an AMD (and possibly others) graphics card.

<b>Instructions:</b>

Step 0: If you're using one of our batch files for AMD graphics cards (from the releases section), ONLY follow the instructions in Step 5. All other steps are done automatically by the script.

Step 1: Download the Mesa 23.0.2 package from [here](https://github.com/pal1000/mesa-dist-win/releases/tag/23.0.2). You may try the different packages provided there, but this fix has only been tested with `mesa3d-23.0.2-release-mingw.7z` so I recommend that one.

Step 2: Unpack the mesa driver into a subdirectory of your WoW root folder. (e.g. in my case C:\\Users\\\<user>\\Downloads\\WoW 0.5.3\\mesa3d-23.0.2-release-mingw\\)

Step 3: Open a Command Prompt window as Administrator and navigate to the mesa folder (cd \<wow root>\\\<mesa root>\)

Step 4: Type `perappdeploy`.

Step 5: Follow the installation utility as listed below:
	
    -At "path to folder holding application executable", provide the full path to your WoW root directory (e.g. in my case C:\Users\<user>\Downloads\WoW 0.5.3)

    -At "application executable name with or without extension", type in "WoWClient.exe". (without quotation marks)

    -Select x86 version when asked. 

    -Answer the next few questions the utility will ask as follows (these are in order):
	
	    Q: Do you want Desktop OpenGL drivers? 
	    A: y
          
	    Q: Do you need OpenGL ES support?      
	    A: n
          
	    Q: Do you need off-screen rendering?
	    A: y
          
	    Q: Deploy video acceleration support?
	    A: y
          
	    Q: More Mesa deployment?
	    A: n

Step 6: Close the cmd and open the Config.wtf file located in \<wow root>\\WTF for editing.

Step 7: Find the line `SET gxApi "direct3d"` and change it to `SET gxApi "opengl"`

Step 8: Save and close the file, and you're done! Launch the WoW client as usual (either through the optional WoW.bat provided by Grender or your own shortcut containing the same startup switches) and enjoy!

<b>IMPORTANT NOTES:</b> 

-Do NOT delete the mesa driver folder from your wow root after you are done! The original folder is required for the fix to work!

-This fix may also work on certain Intel or Nvidia graphics cards. If other fixes fail for your non-AMD GPU, try this one as well!

<b>KNOWN BUG:</b> Game might occasionally crash at launch with an access violation. I've found that changing any setting in your Config.wtf and relaunching the client a few times allows the game to start properly, and you shouldn't have issues anymore for a while after that. I'm not sure what causes this yet.
***
### For Nvidia graphics cards:
This method uses the publicly available <b>QindieGL</b> wrapper that translates OpenGL calls to DirectX 9. While this might not significantly improve your FPS over native Direct3D mode, you may then be able to further translate the DirectX 9 calls to DirectX 12 using a second wrapper (This "double wrapping" is currently being tested, and is not supported, but you can still try it).

<b>Instructions:</b>

Step 0: If you're using one of our batch files for Nvidia graphics cards (from the releases section), you do NOT need to do any of this - the script will do it all automatically.

Step 1: Download the latest release of QindieGL from [here](https://github.com/crystice-softworks/QindieGL/releases/tag/1.0).

Step 2: Extract the archive somewhere and open the resulting folder.

Step 3: Copy `opengl32.dll` to your WoW root folder (e.g. in my case C:\\Users\\\<user>\\Downloads\\WoW 0.5.3).

Step 4: Find the line `SET gxApi "direct3d"` and change it to `SET gxApi "opengl"`

Step 5: Save and close the file, and you're done! Launch the WoW client as usual (either through the optional WoW.bat provided by Grender or your own shortcut containing the same startup switches) and enjoy!

Step 6 (optional, for advanced users): Use a DX9 to 12 wrapper to further translate the calls to a newer API. This may or may not work!

<b>NOTE:</b> This method does not work on AMD graphics cards, and has not been tested on Intel ones.

## Improving framerates in Direct3D mode 
Native Direct3D mode has been found to be incredibly slow on most graphics cards, usually providing only 20 to 30 FPS depending on the exact GPU used. There are a few ways to improve the framerate by forwarding the DirectX calls to a newer version such as DirectX 11 or 12.
***
### Using dgVoodoo2:
dgVoodoo2 is an emulator/wrapper program that emulates a Voodoo graphics card using DirectX 11-12 to allow the user to run various games using the DirectX 9- and 3dfx Glide graphics APIs. It can be used to translate the old Direct3D calls used in WoW 0.5.3 to DirectX 11 or 12 to achieve higher framerates in Direct3D mode. This method should work on most graphics cards, but has been known to crash on specific Nvidia models.

<b>Instructions:</b>

Step 0: If you're using one of your batch files for Direct3D (from the releases section), you ONLY need to follow Steps 5.1 to 5.5. All other steps are done automatically by the scripts.

Step 1: Download the latest regular release of dgVoodoo2 from the [Dege's stuffs](http://dege.freeweb.hu/dgVoodoo2/dgVoodoo2/) website.

Step 2: Extract the archive somewhere and open the resulting folder.

Step 3: Copy `dgVoodooCpl.exe` and `dgVoodoo.conf` from the dgVoodoo2 root folder to your WoW root folder (e.g. in my case C:\\Users\\\<user>\\Downloads\\WoW 0.5.3).

Step 4: Copy `D3D9.dll` from \<dgVoodoo2 root>\\MS\\x86 to your WoW root folder.

Step 5: Run dgVoodooCpl from your WoW root folder.

Step 5.1: In the <b>General</b> tab, select your <b>Output API</b> (`Direct3D 12 (feature level 12.0)` recommended).

Step 5.2: In the <b>General</b> tab, under <b>Adapter(s) to use/enable</b> select your main graphics card that you want to play the game with.

Step 5.3: In the <b>DirectX</b> tab, change <b>VRAM</b> to the desired value (up to your graphics card's physical VRAM).

Step 5.4 (optional): In the <b>DirectX</b> tab, uncheck "dgVoodoo Watermark" to not show a watermark when playing the game.

Step 5.5: Click the "Apply" button.

Step 6: Open the Config.wtf file located in \<wow root>\\WTF for editing and make sure you have `SET gxApi "direct3d"` instead of `SET gxApi "opengl"` (unless you're trying to combine this method with the Nvidia OpenGL one!)

Step 7: Save and close the file, and you're done! Launch the WoW client as usual (either through the optional WoW.bat provided by Grender or your own shortcut containing the same startup switches) and enjoy!
