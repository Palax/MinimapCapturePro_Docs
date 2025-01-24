# MinimapCapturePro_Docs
This is an unreal engine plugin that capture/generate with cartographic design texture maps from your levels 
With these textures you can then use them at runtime in your games to be drawn as minimaps or level maps but this plugin
is not covering any functionality in displaying them ingame.

### Requirements
Target version: UE 5.5
Target platform: Windows

## Installation
Install from the marketplace.
The plugin should reside in your unreal engine /Engine/Plugins/MinimapCapturePro directory. After installation you must activate it by going into the editor Edit > Plugins...

![PluginActivate](https://github.com/user-attachments/assets/947b47b8-efb4-423f-9e6d-e32c7d479809)

## How to use it
Go into your Content Browser > Engine > Plugins > MinimapCapturePro > and drag and drop BP_MinimapProCapture into your level viewport.
![DragAndDrop](https://github.com/user-attachments/assets/ecb1da89-5771-432e-8afa-23d7f2ce29a6)

Once you did this, in your level now resides a blueprint that dictates the capture volume of your level which is the entire level by default, but you can ajust its size if you want.
ALso this blueprint is also keeping your map model serialized in order for you to capture this level more easy after you have made changes to it.
The next thing to do is to activate the MinimapCaptureProTool, a panel that enables the map model to be edited.

![MinimapCaptureProTool](https://github.com/user-attachments/assets/c341e946-288c-40c0-b6e6-f4cb9417ce7d)

In this panel there will be some requirements that needs to be fulfilled, the tool will require the editor to be in "RealTime" mode and also to select which capture blueprint you want to use.
Each of these capture blueprints that you added earlier will have a map model that specifies how to create these captures. Therefore there is a one to one correspondence between a model and a capture blueprint.

![PreModel](https://github.com/user-attachments/assets/69d60adc-ce72-4dbd-9a31-ac9508741fd7)

Once you click these the tool will show you its default ui with no layers on it. And it should look like this:

![EmptyTools](https://github.com/user-attachments/assets/d3566d84-98d6-408e-b297-37de0daad9cf)

## Explaining the Minimap Capture Pro tool
First of all this panel purpose is to stack layers that compose your final cartographic texture map, and its default state its empty.
