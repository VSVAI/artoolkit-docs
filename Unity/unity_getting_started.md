#Getting Started with ARToolKit for Unity

##Installation
ARToolKit for Unity is distributed as a Unity package. A package is an archive of files which can be imported and unpacked into your Unity project. In this case, the package contains the plugin, scripts and resources necessary to integrate ARToolKit with your Unity application.

The ARToolKit for Unity package is available for download from ARToolworks. Download and store this package on your machine.

You can import the package into a fresh or existing project. Select menu `Assets -\> Import Package -\> Custom Package...` and browse to the location where you have stored the ARToolKit package. Select the package, and Unity will ask which files to import. Simply import all files at this stage.

[Assets -\> Import Package -\> Custom Package...][menu_screenshot]

[Use the default (Import all)][import_all]

Your Unity project now contains the necessary files for augmented reality with ARToolKit.

##Package Overview
The package contains:
-   Unity scripts: A set of C\# scripts that handle communication between Unity and the native plugin. These are the scripts that developers will use to access ARToolKit functionality. They are contained inside the file `ARToolKit5-Unity.dll`.
-   Unity Editor scripts: A second set of C\# scripts that extend the Unity editor itself to simplify development with customised editor panels and 3D gizmos. These are contained inside the file `Editor/ARToolKit5-UnityEditor`.
-   ARToolKit plugins: The native plugin implementation. There are various versions for different supported platforms, such as Windows, Mac OS X, Android and iOS. This is stored in the `Assets/Plugins` directory (and subdirectories).
-   ARToolKit data files: The default camera parameters file and two sample patterns are included in the `Resources/ardata directory`. A sample NFT dataset is included in the `Assets/StreamingAssets` directory.
-   Android Activity: A customised version of the Unity player for Android (packaged as a JAR file). Also required is a custom Manifest.xml file, and Android resources in the "res" subdirectory.
-   Simple examples: A set of very basic example scenes which you can use as starting points for various AR techniques are found in `Example scenes`.

[Unity scripts are revealed by turning down the reveal arrow on the "ARToolKit5-Unity" object][editor_screenshot]

##Scene Setup

### Basic Setup
The key script inside the package is named `ARController`, and it manages the overall initialisation, setup, running and shutdown of ARToolKit. One instance of this script should be added to the scene. For simplicity, it is recommended to add it to an empty GameObject in the root of the scene.

[Dragging an instance of the "ARController" script from the Asset browser onto an empty GameObject.][arcontroller_setup]

The ARController script will handle the creation and management of the AR tracking, including the video background. All the developer needs to provide is the Unity [layer][layer] in which to display the video. Layers are used to separate out parts of
the scene so only certain parts are visible to certain cameras. In this case, the video background will be in its own background layer. In the same way, each marker's content will also have its own layer.

To start with, create a new layer *Background* and one for a single marker *Foreground*.

[Choose "Edit layers..." in Unity.][edit_layers]
[Choose two of the User layers and give them appropriate names.][name_layers]

Set the "Layer" property on the "ARController" object to "AR background" (or whatever you named it).

You should now be able to run the scene and see the live video.

### Tracking
Tracking requires markers, so drag the `ARMarker` script onto the ARController GameObject. The script will automatically locate pattern files that have been placed in the project's `Resources/ardata/markers` directory. The default installation will include the standard Hiro and Kanji patterns. These will appear in the dropdown list in the Marker's properties. Select the pattern you want to track. Give this marker a tag (a unique name to identify it within your project).

At this point you can run the scene again. Although no content appears on the marker yet, you should notice console messages appearing to notify that the marker has been found or lost.

To add content to the marker, add a new GameObject to the root. This object will act as a group to contain the sub-scene that will appear on the marker. By default, markers appear in the scene standing vertically (like a billboard) at the origin. Usually however, you want the marker to lie flat on the ground, and this is what we'll do in this case. Select the GameObject you just added, and in the Inspector, change its "Rotation: X" value to 90. This will rotate the child objects of this
GameObject by 90 degrees about the X axis (in a left-hand sense).

[Setting the rotation on the scene root to orient the marker flat on the ground layer.][rotating]

Next, into this group, add:
-   A Unity Camera object: This camera will actually render the sub-scene. (You can reuse the default camera if it exists, or otherwise delete it).
-   A Cube: This will be the initial simple scene. Set the scale to `0.08, 0.08, 0.08`, and position to `0, 0.04, 0`, to sit on the marker correctly.

In order to control the position of the Camera object we just added, drag an `ARCamera` script onto the new camera. This script will update the camera's pose to match the pose of your webcam relative to the marker, thus producing augmented reality. To link this ARCamera to the correct marker, enter the matching tag in the ARCamera's properties.

Select the new group you created and assign it to the Foreground layer created earlier (applying the change to all children when prompted). Finally, select the new camera, and set its Culling Mask property to the Foreground layer also. This step will ensure that the camera only displays the sub-scene. If you add more markers later on, you would repeat this process, but using a different layer (e.g. Foreground2).

*A note on units*: By default the plugin operates in meters. The default marker size is therefore 0.08 (8cm) and the camera has near and far planes of 0.1 and 5.0 respectively (10cm - 5m). These can be changed in the ARController and ARMarker properties.

##Data Files
In a standard ARToolKit application, there are several data files required, such as camera calibrations and patterns. When working with the Unity plugin, these files are included in the projects Assets directory, under the Resources/ardata directory. In order to be recognised by Unity, the files must follow a particular naming scheme.
-   The camera parameters file, `camera_para.dat`, should be stored in `Assets/Resources/ardata/` as `camera_para.bytes`.
-   Pattern files should be stored in Assets/Resources/ardata/ with a .txt extension (e.g. `patt.hiro.txt`).

These files can still be generated using the standard ARToolKit utilities, it is simply the filenames and locations that are important for the Unity plugin.

[menu_screenshot]:/File:Unity_import_package.png "wikilink"
[import_all]:/File:Unity_import_ARToolKit_2012-06.png "wikilink"
[editor_screenshot]:/File:ARToolKit_for_Unity_scripts.png "wikilink"
[arcontroller_setup]:/File:Unity_drag_ARToolKit_script_onto_empty_gameobject.png "wikilink"
[layer]:http://unity3d.com/support/documentation/Components/Layers.html
[edit_layers]:/File:Unity_-_Edit_layers.jpg "wikilink"
[name_layers]:/File:Unity_-_AR_layers.jpg "wikilink"
[rotating]:/File:ARToolKit_for_Unity_-_Setting_scene_root_rotation.png "wikilink"