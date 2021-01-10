Procedural Environment Generator (WFC) is based on WaveFunctionCollapse algorithm and allows to generate environments of the user-specified dimensions built from the prepared modules with connection rules.

In the plugin description you can see the screenshots made after putting WfcActorWithBlueprint (Content/Demo) into the scene and pressing button "Generate in Editor" in the WfcActorWithBlueprint details panel.

Playlist with tutorials - https://www.youtube.com/playlist?list=PLsrW-AKNS-U5YTcoE5yfGFMcC4f5kdJGS

Version 6.5
 - Fix the ability to use plugin in Runtime, update Module type and introduce the separate module with editor-only functionality.

Version 6.4
 - Support Export and Import of WfcActor configurations via json files

Version 6.3
 - Auto Map Components to Actors checkbox maps WfcVoxel components and custom Blueprints which have the same name

Version 6.2
 - Fix the bug with actors placement
 - Make possible to set different heights for different Z levels

Version 6.1
 - Enable the use of "AddInstancedStaticMesh" function in growing environments
 - Make possible to map WfcVoxel-s to real actors directly in details panel
 - Render mapped actors instead of static meshes in Blueprint preview

Version 6.0
 - Added symmetric constraints for X and Y axes, they work also in growing environment mode
 - "Grows By" parameter is removed. Instead you can use the "Grows" flag which now indicates that the X and Y sizes
   are maintained around the position of grow. So your X and Y work now as if you have "Grows By" set to X/2 and Y/2
 - "Ground", "Air Border" and "Floor Border" fields are deprecated. They are replaced by more advanced
   "Fixed Components" constraint which allows to set any kind of border. In the demo content the same borders are
   recreated using "Fixed Components".

Version 5.3
 - Allow to map custom external actors to WfcComponents via "Map Meshes to Actors" blueprint function (example in WfcUnlimited)
 - Introduce "Starting Point" parameter to start generation around specific position (example in WfcUnlimited)
 - Introduce "Walkable Paths" together with "IsWalkable" properlies for X/Y/Z in WfcComponents. These are the constraints
   that are checked during the generation. They automatically check that the specified paths are all fully walkable and restart
   the generation until the proper level is generated (more advanced implementation will come later). Limited to non-growing environments.

Version 5.2 Changes
 Fixed bug with wholes in unlimited map when you go in Y direction.

Version 5.1 Changes
 Checkbox "Clean Up After Grow" added. It allows to remove older parts of the unlimited map after
 it has grown by clicking on the Actor in editor or with "Increase from Point"/"Increase from Point Async"
 blueprint function. It is now turned on in the example provided in WfcUnlimited blueprint (and in WfcLevel map).

Version 5.0 Changes
 - Unlimited environment is supported via step by step expansion.
   Available via:
    a) Clicking on existing actors in editor when "Grows By" parameter is > 0
    b) Increase from Point blueprint function allows to start growing map from the specified Location
   The example of both approaches is demonstrated in WfcUnlimited blueprint class

Version 4.2 Changes
 - new restriction settings are added:
   a) fixed components: you can now set components names and their exact locations with rotation and they will be always put there
   b) components maximum count: you can limit the maximum amount of certain components

Version 4.1 Changes
 - improved undo
 - borders can now be set separately without failing generation
 - seed is stable, fixed bug when the same seed led to different environment

Version 4.0 Changes
 - optimizations for large generated areas:
   a) faster generation
   b) faster rendering (with IstancedStaticMesh-es)
   b) better 'undo' for almost infinite loops
 - generation in background, cancelling, progress log:
   a) "Clean generated" cancels unfinished generation in editor
   b) "Clear All" blueprint function cancels generation in play mode (letter 'C' in example blueprint)
   c) progress is now shown in the "Output Log" tab of UE

Version 3.0 Changes
 - over 20 times faster generation

Version 2.0 Changes
-------------
 - added "Generate Meshes" and "Generate Meshes Async" blueprint functions. They enable the possibility to run generator at any time
   point and handle the results in the blueprint
 - WfcActorWithBlueprint from the Demo content now has examples in EventGraph how to generate meshes and spawn them.
   To generate synchronously - press "G", to do that in background - press "L". Clear everything by "C".
 - replication is now supported via "Spawn Actor" blueprint function from WfcActor, the examples replicate generated actors by default
 - memory optimization
 - minor bugs fixed

How to use this Plugin
-------------

Check out the video tutorial about the basics: https://www.youtube.com/watch?v=0t3A4YZM6ks

Demo
-------------
First check out the demo Blueprint where everything is already set up.
You have to enable "Show Engine Content" for the content browser and open WfcEnvironmentGenerator Content -> Demo -> WfcActorWithBlueprint

With the example Blueprint you can generate the level right away.
To do that just press the button "Generate in Editor"!

Inside this Blueprint you can find:
1. "Generator" properties of WfcActor:
   - "X", "Y", "Z" are the dimensions of the generated level.
   - "Ground" is set to the name of WfcVoxel which will be used for the whole 0 floor. Empty field means no restrictions.
     In the example it's set to "solid".
   - "Floor Border" - restriction for the 1 floor edges (only for 1 voxel from the border). Empty field means no restrictions.
     In the example it's set to "floor".
   - "Air Border" - restriction for all floors except 0 and 1 (only for 1 voxel from the border) and for the whole top floor.
     Empty field means no restrictions. In the example it's set to "empty".
   - "Voxel Size" is the size of your mesh side. It is set to 200 which is the size of the mesh used in example.
   - "Last Seed" shows you the seed used to generate the last level. Save it and use in "Seed" field to recreate you level.
   - "Seed" field should be empty most of the time unless you want to recreate the level you generated earlier.
   - "Shadow Material" is used to replace the Element 0 material. It is used only to show the connected meshes (hints) in Blueprint.
     Don't set it if you don't want to see connected meshes.
   - "Generate in Play Mode" - select it to see interactive level generation after you press the Play button
2. WfcActor contains a set of WfcVoxels (based on UStaticMeshComponent class), each of them has additional properties split in 2 groups:
 2a. "WfcVoxel" properties:
   - "Weight" helps to adjust the frequiency of the mesh in level, has to be >= 1.
   - "X/Y/Z Plus/Minus Direction" is the number of connector used at this direction.
   - "X/Y Plus/Minus Direction Symmetry" has 3 values:
     - "Connects to Itself" means that this connector is symmetric.
     - "Straight" connects only with "Flipped" with the same number and vice versa.
   - "Z Plus/Minus Direction Symmetry" has 5 values:
     - "Any Rotation" is self explanatory.
     - "Rotate <T> Times" means that this voxel connects in this direction only when it's rotated T times.
       For example when first WfcVoxel has "Z Plus Symmetry" = "Rotate One Time" and second - "Z Minus Symmetry" = "Rotate Three Times"
       that means that the first one has to be rotated twice to match be connected to the second one.
 2b. "WfcVoxel Restrictions" properties:
   - "X/Y/Z Plus/Minus Excluded" - names of other WfcVoxels (neighbors) that you don't want to be connected to this one in this direction,
     even if their connectors match.

You may play with these properties to see how they effect the result of the generator.

Setting up your new Blueprint
-------------

1. Place "WfcActor" to your level
2. Press "Blueprint/Add Script", select the name.
3. Open Blueprint editor. Change the settings (see 4-8).
   For the settings description check the "Demo" section of this Readme.
4. Set "Shadow Material" for the root object (if you want to see the shadows). You may use "shadow" material from the "Demo" content.
5. Set dimensions and other settings of WfcActor in "Generator" properties tab.
6. Add "WfcVoxel" components to the DefaultSceneRoot.
7. Set up weights, connectors and excluded neighbors.
8. Do not set any mesh to the WfcVoxel if you want it to be empty (air for example).
9. In Unreal Editor (NOT in Blueprint editor) press "Generate in Editor" to generate your level.
10. The set of new actors will be spawned each containing the mesh you've set up earlier.
11. You can clean them with button "Clean Generated". Or save "Last Seed" if you want to generate the same level later.
12. To generate the same level again - paste your saved seed into "Seed" property and press "Generate in Editor".
13. If you want to see interactive generation you have to set "Generate in Play Mode" and press play button.

Contact
-------------
If you have any Questions, Comments, Bug reports or feature requests for this plugin, or you wish to contact me you can and should email me - yv.ivan@gmail.com
