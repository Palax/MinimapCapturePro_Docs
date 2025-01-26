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
Each layer will also contain their type, their layer name followed by a small gray button with bin icon that can enable deleting this layer from the layer stack.

# Detailed information about layer types
## DirectRender
![DirectRenderWithTex](https://github.com/user-attachments/assets/c3a919a6-3834-492d-9163-7035e2b9c3af)  
As the name implies, this layer will capture your level map into the texture on how it looks in your viewport with the default materials and textures on your landscape and meshes.
It is probably a typical starter on your layer stack as you can use it to feed this render into your other layer to achieve more interesting maps.
At this moment in the plugin version 1.0, the lighting is captured into the texture as it is in your viewport.
- "**Max View Distance Override**" if > 0, sets a maximum render distance override.  Can be used to cull distant objects from a reflection if the reflecting plane is in an enclosed area like a hallway or room, Default value is -1
- "**Capture Source**" dictates the render type, with Final Color and Base Color being the most useful as it renders with materials and lighting or materials only  
![DirectRenderComp](https://github.com/user-attachments/assets/3450e668-9a6c-4f31-be87-a7fa0a14bc28)
- "**Always Persist Rendering State**" Whether to persist the rendering state even if bCaptureEveryFrame==false.  This allows velocities for Motion Blur and Temporal AA to be computed. Default is true
- "**Use Ray Tracing if Enabled**" as its name implies will render with this feature on in drawing the Final Color in RGB
- "**Transparent Background**" Whether to make the part of the texture that shows no scene captured pixels as transparent  Default = true; This could be used for composition; On false will make the texture to show black background
- "**Hide Sky**" If this is set to true the sky will no longer be captured into the final texture, this is mostly expected to be true as the sky breaks the transparent background detection
- "**Sky Name**" you can use this name to specify the possible names of the sky sphere that must be hidden during the capture process, use this name in case your sky has a different name
- "**Sky Tag**" in case the above parameter fails in detecting the sky you can add a tag on the sky sphere in order for this plugin to hide it during the capture process
- "**Hide All Sky Atmosphere**" atmospheres are not intended to be captured so all of these component types into your map will be hidden during capture
- "**Hide All Sky Lights**" these will be hidden during capture as they might not render correctly
- "**Capture Just Landscapes**" can be used to only render the landscape, nothing else will be captured
At the end of capturing this layer it will export a [Main] texture and a [MainMask] texture that can be pluged in into the other layers. 
## HeightGradients

![HeightGradientsWithTex](https://github.com/user-attachments/assets/730c5e57-ef46-407d-ba93-765e9d246c2d)  
With this layer you can capture your level map based on the heights and draw them into colored or black and white gradients. For example if your intended cartographic design is to draw the height based terrain maps, you could put blue at the lowest height to represent water, then yellow at the mid to represent typical ground then followed by green to signal vegetation ending with brown at the top to signal mountains. This layer is perfect for this purpose. It supports up to 8 gradient ramps, each ramp having a low and a high color and its drawn within a specific height range.
In the image above you can see that a black and white colors are used to cover the entire drawing range of heights and it ends up drawng a typical  black and white height map.
This layer also supports drawing colored outlines on these gradient ramps for a more stylized look.
- "**Transparent Background**" does the same thing as in the Direct Render layer type, when off it will draw black or a different color for the background parts of the texture, by background it means areas of the scene that have nothing to render and will be typical sky rendering in the viewport, most of the time this flag is useful as true
- "**Height Range**" this is a very important parameter for this type of layer as it specifies the range of heights at which to draw. There are two options here: "Use Minimap Capture Bounds" and "Define Exact World Coords". The first uses the capture blueprint box and it covers the entire map height range, or for a more fine grained control use the latter option and adjust the range according to your needs. Use the viewport in tandem with these parameters as the tool will draw you a wire that represents the "**Top**" and "**Bottom**" part of this height range.  
![Height Range1](https://github.com/user-attachments/assets/4668947f-9f84-4f4b-a58a-c2e2b4090530)

- "**Gradients**" another important parameter that dictates how the maximum of 8 possible ramps of color are drawn and where. Each of these ramps contain a "**High Color**" and a "**Low Color**" to specify the two colors bordering the slice of the height they represent. Each time you add a gradient ramp into this list, the whole Height Range is used to include all of the ramps in this list. Use "**Coordinate WZ**" to specify the high point of each ramp, and each ramp is drawn between the ramp below it high point and current coordinate. The topmost ramp, the one at index 0 lacks this parameter as its coordinate is exactly the top **Height Range**. If no other ramp is added into the list it will basically use the entire **Height Range** to draw itself, with the Bottom coinciding with Low Color and Top with High Color. "**Color Bias**" can be used to push the color drawing to be non linear in that gradient ramp.  
![HeightGradientsRampExamples](https://github.com/user-attachments/assets/8781999f-4044-475c-aad7-487c30c1178d)

