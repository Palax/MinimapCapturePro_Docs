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
this button to work. When you click this button you will notice under the button a blue bar will be displayed to showcase the progress of this process, during this time is recommended to not interfere with the
editor and also do not minimize unreal as will minimize computational resources and therefore take forever to finish.

## How the layers work broadly
![LayerArea](https://github.com/user-attachments/assets/89b9b1f4-8ea0-489a-be3b-f2e276c71650)  
In this image you can see an example of layer UI square with rounded borders, these are visual hints on where such layers are displayed visually and which elements are part of which layer.  
A layer in this tool acts as some sort of "designer" for your map that is tasked to do one type of thing depending on the layer type. Once this layer finishes executing it will create a texture
and one or more masks with its drawings / captures that can be fed up to the layers below it. At the end of this stack of layers the final texture will typically contain all your drawings and the other textures and masks will be dumped. A layer has its own visual space that is a square with rounded borders and can be seen in the image above.
As you see in the image below texture is the left texture and mask is the right texture and nothing else than a name for a alpha map.
![TextureMaskExample](https://github.com/user-attachments/assets/cec7387e-0c38-41ed-8c99-10aa925406e0)  
Every layer will dump at least a [Main] texture and a [MainMask] which represents the alpha channel of the main texture. A mask basically specifies where in the resulted texture the layer has drawn into it. With white areas acting as the pixels drawn and the black areas as not.
Each layer will also contain their type, their layer name followed by a small gray button with bin icon that can enable deleting this layer from the layer stack.

# Detailed information about layer types
## Layer Direct Render
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
## Layer Height Gradients

![HeightGradientsWithTex](https://github.com/user-attachments/assets/730c5e57-ef46-407d-ba93-765e9d246c2d)  
With this layer you can capture your level map based on the heights and draw them into colored or black and white gradients. For example if your intended cartographic design is to draw the height based terrain maps, you could put blue at the lowest height to represent water, then yellow at the mid to represent typical ground then followed by green to signal vegetation ending with brown at the top to signal mountains. This layer is perfect for this purpose. It supports up to 8 gradient ramps, each ramp having a low and a high color and its drawn within a specific height range.
In the image above you can see that a black and white colors are used to cover the entire drawing range of heights and it ends up drawng a typical  black and white height map.
This layer also supports drawing colored outlines on these gradient ramps for a more stylized look.
- "**Transparent Background**" does the same thing as in the Direct Render layer type, when off it will draw black or a different color for the background parts of the texture, by background it means areas of the scene that have nothing to render and will be typical sky rendering in the viewport, most of the time this flag is useful as true
- "**Height Range**" this is a very important parameter for this type of layer as it specifies the range of heights at which to draw. There are two options here: "Use Minimap Capture Bounds" and "Define Exact World Coords". The first uses the capture blueprint box and it covers the entire map height range, or for a more fine grained control use the latter option and adjust the range according to your needs. Use the viewport in tandem with these parameters as the tool will draw you a wire that represents the "**Top**" and "**Bottom**" part of this height range.  
![Height Range1](https://github.com/user-attachments/assets/4668947f-9f84-4f4b-a58a-c2e2b4090530)

- "**Gradients**" another important parameter that dictates how the maximum of 8 possible ramps of color are drawn and where. Each of these ramps contain a "**High Color**" and a "**Low Color**" to specify the two colors bordering the slice of the height they represent. Each time you add a gradient ramp into this list, the whole Height Range is used to include all of the ramps in this list. Use "**Coordinate WZ**" to specify the high point of each ramp, and each ramp is drawn between the ramp below it high point and current coordinate. The topmost ramp, the one at index 0 lacks this parameter as its coordinate is exactly the top **Height Range**. If no other ramp is added into the list it will basically use the entire **Height Range** to draw itself, with the Bottom coinciding with Low Color and Top with High Color. "**Color Bias**" can be used to push the color drawing to be non linear in that gradient ramp.  
![HeightGradientsRampExamples](https://github.com/user-attachments/assets/8781999f-4044-475c-aad7-487c30c1178d)  

- "**Hide Sky**" is the same parameter as in Direct Render layer, and it should hide the sky during capture
- "**Capture Just Landscape**" same as in Direct Render layer it dictates if it should use only the landscape or the entire scene as source of heights for this layer
- "**Hide Foliage Actors**" vegetation can break the clear lines of the height gradient ramps as vegetation could have a wide range of height values, use this flag to hide them
- "**Outline Tickness**" is enables color outline drawing for each of the gradient ramps, this value is in pixels in the final texture, a value of 0 disables this feature, a value of 32 pixels is the maximum
- "**Outline Intersections**" is a subtle feature that dictates how the outlines blend when they intersect each other in case that happens
- "**Outline Color**"" this value is part of each ramp and dictates what color is drawn for that ramp outlines  
![HeightGradientOutlines](https://github.com/user-attachments/assets/4cba80c8-ffa9-46a5-b783-ef6396b5ddf7)  

At the end of capturing this layer it will export a [Main] texture as you see above, a [MainMask] that contains all the drawn pixels of this layer and a list of [GradientMask_[1-8]] that only represent the drawn pixels for each of the gradient ramps added into **Gradients** list.
## Layer Draw Navigation
![NavDrawLayer](https://github.com/user-attachments/assets/10794a42-3b2e-4658-912f-cda19168f328)  

Is a useful type of layer that can give you the ability to enhance accesible areas to the players in your map by drawing the navigation. This layer requires NavMeshBoundsVolumes into your level so that the Navigation system has a valid mesh built. If you see this layer as red, make sure to add at least one such NavMeshBoundsVolumes into your map, sometimes navigation requires an editor restart.
- "**Toggle Show Nav Wire Mesh**" is a button that you can use to display the navigation mesh in your viewport, this mesh will be used to be drawn into the texture generated by this layer, use this button to also make sure that there is a mesh to be drawn. Note that this might slow the editor, so make sure to hide it when you don't need it by clicking this button again.
- "**Display Nav Wire Mesh"** cannot be edited directly as it is toggled by the button above, when this is true it means that it will draw the mesh in viewport
- "**Draw Using Default Nav Area Colors**" when true it will use editor default color values for each of the NavArea class types or if this is false it will show the...
- "**Nav Area Colors**" list that can permit specifying the color for each of these classes of navigation areas  
![NavMap_Parameters](https://github.com/user-attachments/assets/1804677b-495c-4194-88de-c6a834fea48c)
- "**Draw Only Largest Patch**" this parameter when true it will be used to draw only the largest navigation patch (meaning all meshes that share common edges or nav links) by area on the map.
This is useful in case you want to render parts of your map that are not reachable by the player. As you see in the image above when this is on, small patches of unconnected patches are not drawn.
Note that if this is false it will show you another parameter called "**Min Area Patch Limit**" that gives you control to not draw patches with area smaller than this value.

## Layer Draw Text
![DrawTextsLayer](https://github.com/user-attachments/assets/3ad631d6-cad1-43fb-b056-8a7ca9d33c0d)  
As its name implies, this layer is intended to be used to draw text at specific locations within the texture.Note that the DirectRender layer in the image is used as input for our text layer in the parameter named "**Base**".
This is the first example of this type of layer that has input another texture from a different layer.    
What you will notice is that the text is also drawn in the viewport, and not only that, you can use your mouse to change the text position by using drag-clicking, the text that can be moved will flicker white to its color when your mouse is near the text.
- "**Base**" So, everytime you use this layer make sure to have some other layer on top that generates a texture to be used as input for the text to be drawn upon, then when you click the button will show you a list of all texture inputs that you can use.
- "**Blend Type**" this is a very important parameter as the drawn text will be blended against the **Base** texture, the blending options are mostly typical blending options you find for example in photoshop and by default the Normal type will be used and text is shown in the texture exactly as you see it in the viewport.
- "**Texts**" The main parameter of this layer is Texts, its a list that you can add texts one at a time that contains all the parameters needed to change the looks of this text instance, such as color and size and the rest
- "**World Position**" one of the parameters of these texts is this position and is within the world of map, you can change the values and you will be able to see the text shifting location within the viewport and your rendered texture
- "**Text**" here you can specify what is exactly the string name of the text displayed
- "**Color**" this is an obvious parameter, the entire color of the text
- "**Font**" the shape and looks of the text is represented by the font, make sure at this time to drag the font into this parameter and not select it from the drop down list as it fails to render properly
- "**Shadow Color**" if the alpha is beyond zero into this color it will enable shadow rendering of this text, by default the alpha is 0 and therefore the shadow is not drawn
- "**Shadow Offset**" dictates how the shadow is positioned from the text, this is only relevant if text shadows are enabled
- "**Outlined**" represents the text outline with the color specified in...
- "**Outline Color**" 
- "**Horizontal Spacing Adjust**" represent the distance between text symbols
- "**Scale**" enables scaling of the text, but beware that larger than 1 text makes the text render blurry, for proper sizing use the font
## Mask Effects  
![MaskEffects1](https://github.com/user-attachments/assets/dab091ff-f891-4b11-ba9d-a5f8b8a1a518)  

You can think of this layer as something similar with Layer Syles in photoshop. The mask implies inputs to this layer are needed as a form of masks to dictate where these effects are drawn upon. The red outline of the layer visible in the image above is the fact that no option is picked for such effects yet and therefore nothing will be drawn even if capture button would be able to proceed.
- "**Click to add output texture:**" and "**Click to remove out texture:**" are two states of the same button that specifies which texture is used as input for this layer, when the text is red it means you have to select a valid textures from the available texture outputs from the layers above this one, the "**Unselected**" button will give you a list of all those possible valid textures that can be selected, when you did that the text on the button will become green and you have to click it to commit this texture to this layer, after this second click the main input texture will be displayed in light blue (can also be seen in the image as "HeightGradients_973")
- "**Click to add mask:**" and "**Click to remove commited mask:**" are text representing the states of the added masks to this layer, you can add multiple masks in this list by clicking the green text, and will activate a second line that can be specified
After you have successfully commited output and the masks to this layer, you need to specify one of the 4 possible effect types by clicking one or all of the 4: "**Add Fill**", "**Add Outline**", "**Add Inner Glow**" and "**Add Outer Glow**" 
![MaskEffectsFillColor](https://github.com/user-attachments/assets/8f8dae0c-5854-4171-9bb8-5d2e5ae1454a)

As you may notice, the layer border is now displayed as grey which is a good thing as this layer can actually render something, and that something is visible in this image.
- "**Add Fill Color**" will draw a uniform color to cover the entire masks applied over this layer, these masks are visible as the black and white texture in the mid of the above image.
- "**Fill Color Distance**" when this value is 0 it means that all pixels within the mask will be drawn as this color, but a non zero value it means that any pixel that is within this distance from any margin of the mask it will not draw.

   
Once you click the "**Add Outline**", the Outlines parameters will be displayed on the panel, this feature is almost the same feature found in the Height Gradients layer but here it is more powerful as it can draw outlines from different incoming masks into this layer.
![OutlineThickness](https://github.com/user-attachments/assets/f202c1db-7043-4034-9f0d-d2839cb08774)  
- "**Outline Thickness**" is how thick the outlines are rendered in a range from 0 to 32 pixels wide, a value of 0 disables this feature
- "**Outline Intersections**" is how these outlines are blended in case they are rendered one on top of the other
- "**Outline Color**" the color of the outline

In cleanup section there are two options intended to fix stranded pixels that act more as noise or debris and these can be removed by "**Cleanup Small Pixel Patches**" and "**Cleanup On Pixels**".  

![InnerGlowExamples](https://github.com/user-attachments/assets/630d4af5-7439-4cf1-813a-34242034eadc)  
Inner Glow section is activated if you click on the "**Add Inner Glow**". This feature as its name implies is a glow that is drawn  inside the masks on its margins.
- "**Inner Glow Margin Color**" represents the color that touches the margins of the masks
- "**Inner Glow Inner Color**" represents the color on the inside of the mask
- "**Inner Glow Blend**" represents how the this glow blends against the **Out texture** (texture that is always visible on top of this layer properties with blue text)
- "**Inner Glow Thickness**" is a value between 1 and 64 pixels and represents how wide is this glow, to have a value of zero you must unclik the **Add Inner Glow**, values larger than 64 pixels are not available at this time

![OuterGlowExamples](https://github.com/user-attachments/assets/64700ef4-4883-47cd-8613-caf49a2d87fb)
Outer Glow section is activated if you click on the "**Add Outer Glow**". This feature as its name implies is a glow that is drawn on the outside of the masks.
- "**Outer Glow Margin Color**" represents the color drawn on the outer part the margin of the masks
- "**Outer Glow Inner Color**" represents the color that touches the margin of the masks
- "**Outer Glow Blend**" represents how the glow blends with the **Out texture**
- "**Outer Glow Thickness**" represents how wide is the glow in a range of 1 to 64 pixels from the masks margins

## Layer Blend   
![BlendLayer](https://github.com/user-attachments/assets/8da4336d-bc08-4304-89cd-fc33d999bd2f)  
This type of layer is very simple in purpose, is to blend two texture outputs from layers above, as you can see in this image, there are two top layers, one that renders the scene as Base Color and one that renders a Height Map, and this blend layer blends the top texture against the bottom and finally outputs this into a new texture.
- "**Base:**" is the parameter that specifies the bottom texture
- "**Top:**" is the parameter that specifies the texture that is blended against the base
- "**Blend:**" is the type of blend, and these options are: Normal, Add, Subtract, Multiply, Darken, Screen, Overlay, HardLight and SoftLight; Note that these do the same things as layer types you find Photoshop
- "**Opacity:**" how much of this blend happens in percentage, a value of 0 does nothing

### Captured Texture Meta Data
This is a very important topic. Each texture exported by this plugin has a requirement on another plugin to keep some metadata.  Therefore make sure you have this plugin also at runtime into your game so you can read these values.
![Datasmith](https://github.com/user-attachments/assets/5547e8c4-e357-4a52-8986-9f3895fc374c)  
With this plugin activated you can parse each of these generated textures at runtime.  
![Metadata](https://github.com/user-attachments/assets/ae80e095-ecd0-4af3-9550-e02554499f48)  
As you can also see in this image, in the metadata you will find **Map** which represents which map this texture is related to. The **TextureCenterWorldPositionX** and **TextureCenterWorldPositionY** represents the world axis x and y coordinates of the center of this image, for the Z coordinate  you can assume its zero so its not written here.  
**TextureWorldSizeX** and **TextureWorldSizeY** represents how much of the map units is captured in the texture, so if for example you hava a 2048x2048 texture representing a 10000 uu, it means each pixel in the texture is representing 10000/2048 ~= roughly 5 uu per pixel.  
At this time, this plugin captures the texture aligned with the world axis, therefore the X axis of the texture is aligned with world X axis, also the same thing goes for the Y axis.
With this information here you can draw this texture inside any minimap widget.  
To find out how to parse this metadata, use the unreal engine documentation [link](https://dev.epicgames.com/documentation/en-us/unreal-engine/using-datasmith-metadata-in-unreal-engine?application_version=5.4)
