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

## Explaining the basic tool buttons
First of all this panel purpose is to stack layers that compose your final cartographic texture map. What you see above is the empty state of the tool.  
- "**Select layer class**" in order to add a layer on this tool you must pick your layer type, once you select that layer type the...  
- "**Click to add Layer**" becomes green and you can click anywhere on this button to add this type of layer into the layer stack  
- "**Resolution**" will specify what size your final texture will be, at this moment only square textures are supported in range of 128 x 128 to 8192 x 8192  
- "**Texture**" and the subsequent elements represents what is the path to where this final texture will be generated  
Start by clicking Path and then in the second black box specify the texture name  
- "**Capture**" this button will start processing each layer in <ins>top to bottom</ins> order and produce your map; If this button is dark grey and has red text color it means that
there is a layer in the stack that has a prerequisite that is not met. For example, some layers might require an input such as a texture or a mask and you need to specify it in order for
this button to work.

## How the layers work broadly
![LayerArea](https://github.com/user-attachments/assets/89b9b1f4-8ea0-489a-be3b-f2e276c71650)  
In this image you can see an example of layer square with rounded borders, these are visual hints on where such layers are displayed visually and which elements are part of which layer.  
A layer in this tool acts as some sort of "designer" for your map that is tasked to do one type of thing depending on the layer type. Once this layer finishes executing it will create a texture
and one or more masks with its drawings / captures that can be fed up to the layers below it. At the end of this stack of layers the final texture will typically contain all your drawings and the other textures and masks will be dumped. A layer has its own visual space that is a square with rounded borders and can be seen in the image above.
As you see in the image below texture is the left texture and mask is the right texture and nothing else than a name for a alpha map.
![TextureMaskExample](https://github.com/user-attachments/assets/cec7387e-0c38-41ed-8c99-10aa925406e0)  
Every layer will dump at least a [Main] texture and a [MainMask] which represents the alpha channel of the main texture. A mask basically specifies where in the resulted texture the layer has drawn into it. With white areas acting as the pixels drawn and the black areas as not.

