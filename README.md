# Alpha WoW Grafix - A repository of various fixes and instructions for fixing graphical issues and improving FPS in WoW 0.5.3
The graphics APIs used in World of Warcraft Alpha 0.5.3.3368 have various issues and incompatibilities with modern graphics cards that vary based on GPU brand, model, and driver version. This repository aims to be a common resource containing fixes and instructions for as many different such issues as possible. More fixes will be added over time as different graphics cards are tested.

## Prerequisites
-Make sure you're using the modded WoW client before applying any of these fixes. The modded client can be found [here](https://anonfiles.com/fbi9C0M0y9/Mods_zip), or use this [mirror](https://cdn.discordapp.com/attachments/653374433636909077/1089350731909308466/Mods.zip). Simply replace the files in your WoW alpha root folder with the modded ones from the archive.

-Update your graphics drivers to the latest version! Driver download pages are [here](https://www.nvidia.com/download/index.aspx) for Nvidia, [here](https://www.amd.com/en/support) for AMD, and [here](https://www.intel.com/content/www/us/en/download-center/home.html) for Intel.

-If you've tried other methods of improving FPS or fixing graphical issues/crashes, such as modified opengl/d3d drivers (e.g. dxgi.dll, d3d9.dll, opengl32.dll), remove all of them from the game folder before proceeding.

##  Running in OpenGL mode on unsupported hardware
Modern graphics cards have various issues with old OpenGL versions. On AMD, textures and models will often be stretched, flickering, and generally broken. On Nvidia, the game might not even start in OpenGL mode. Below are instructions on how to run OpenGL mode by translating the API calls to a different API (such as DX12 or DX9).
### For AMD graphics cards:
This method uses the publicly available Mesa driver to translate OpenGL calls to DirectX 12, fixing most of the graphical issues encountered when using OpenGL on an AMD graphics card.

####Instructions:
Step 1: Download the Mesa 23.0.2 package from [here](https://github.com/pal1000/mesa-dist-win/releases/tag/23.0.2). You may try the different packages provided there, but this fix has only been tested with "mesa3d-23.0.2-release-mingw.7z" so I recommend that one.

Step 2: Unpack the mesa driver into a subdirectory of your WoW root folder. (e.g. C:\Users\<user>\Downloads\WoW 0.5.3\mesa3d-23.0.2-release-mingw\)

Step 3: Open a Command Prompt window as Administrator and navigate to the mesa folder (cd <wow root>\<mesa root>\)

Step 4: Type "perappdeploy" and follow the installation utility as listed below:
	
4.1: At "path to folder holding application executable", provide the full path to your WoW root directory (e.g. in my case C:\Users\<user>\Downloads\WoW 0.5.3)

4.2: At "application executable name with or without extension", type in "WoWClient.exe". (without quotation marks)

4.3: Select x86 version when asked. 

4.4: Answer the next few questions the utility will ask as follows (these are in order):

	
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

-Step 5: Close the cmd and open the Config.wtf file located in <wow root>\WTF for editing.

-Step 6: Find the line SET gxApi "direct3d" and change it to SET gxApi "opengl"

-Step 7: Save and close the file, and you're done! Launch the WoW client as usual (either through the optional WoW.bat provided by Grender or your own shortcut containing the same startup switches) and enjoy!

IMPORTANT NOTE: Do NOT delete the mesa driver folder from your wow root after you are done! The original folder is required for the fix to work!

KNOWN BUGS:
-Game might occasionally crash at launch with an access violation. I've found that changing any setting in your Config.wtf and relaunching the client a few times allows the game to start properly, and you shouldn't have issues anymore for a while after that. I'm not sure what causes this yet.

### For Nvidia graphics cards:


## Improving framerates in Direct3D mode 
### Using dgVoodoo2
