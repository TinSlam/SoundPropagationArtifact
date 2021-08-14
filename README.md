# Efficient 2D Sound Propagation in Video Games
Artifacts for the paper "Efficient 2D Sound Propagation in Video Games."

The Unity package can be downloaded to use and test the model. <br>
The full source code can be found [here](https://github.com/TinSlam/SoundPropagation). <br>
The demo software showcases the model in multiple minigames. The program is best to be used with headphones.

# Code Instructions
1. Create or open a Unity 3D project.
2. Download and import the SoundPropagation Unity package. <br>
  I. Choose Assets > Import Package > Custom Package. <br>
  II. Locate the downloaded package "SoundPropagation.unitypackage." <br>
  III. Click import. <br>
3. Add a new layer named "SoundModels." <br>
  I. Choose Layers > Edit Layers... <br>
  II. Pick an empty layer and name it "SoundModels" <br>
4. You can now run any of the demo scenes, which include the ones used in the paper.
5. In 2D mode, the position of the cursor is the listener. In 3D mode, the position of the 3D character is the listener.

# Creating a New Scene
1. Add the "SoundPropagation" prefab to your scene.
2. Define the sound geometry. <br>
  I. You can load the geometry from any maps on https://movingai.com/benchmarks/grids.html by setting the following values in the inspector for the GeometryData game object. <br> <br>
      Load Map From File => True <br>
      File Name => Directory of the ".map" file downloaded. The path is relative to Assets/Resources/Maps. <br> <br>
  II. You can create custom geometries by adding "SoundObstacle" prefabs as children of the GeometryData game object. <br>
3. Sound sources can be added using the "SoundSource" prefab.

# Unity Inspector
The inspector can be used to control the model and the geometry.

`SoundPropagation > Manager` Game Object
1. Render Mode 3D: Allows the user to switch between 2D and 3D cameras. Can be toggled at run-time.
2. Visualize Geometry: Determines whether the sound geometry should be visualized. Can be toggled at run-time.
3. Create 3D Colliders: When active, prevents the player from passing through the sound geometry in 3D mode.
4. Active Geometry Index: The current sound geometry to use. Can be changed at run-time, if `Geometry Simplification Thresholds` created simplified geometries.
5. Geometry Simplification Thresholds: If this list is empty, the geometry is not simplified. For any value inside the list, a simplified version of the geometry will also be created. The list should be specified before run-time.
6. Map Partition Size: A parameter for the spatial partitioning of the geometry. Should be specified before run-time.
7. Mouse Follow 2D: Determines whether the sound source should follow the cursor in 2D mode. Can be toggled at run-time.
8. Camera Translation Speed: Determines the speed of camera movement using WASD keys. Can be changed at run-time.
9. Point, Line, and Circle Pool Sizes: A pool of resources to optimize the visualization. If the pool is exhausted, visualization may become slow. Should be specified before run-time.
10. Debug Sound Source: The sound source that is affected by keyboard shortcuts found in the UI. It can only be changed at run-time.

`SoundPropagation > GeometryData` Game Object
1. Load Map From File: Determines whether the map should be loaded from a file from https://movingai.com/benchmarks/grids.html, or it should be loaded as a custom maps from children game objects.
2. File Name: If loading from a map, this would be the directory of the ".map" file downloaded. The path is relative to Assets/Resources/Maps.

`SoundPropagation > GeometryData > SoundObstacle` Game Object
1. Add Noise: If true, adds a small noise to the points from the line renderer.
2. Materials: A list of surface materials corresponding to each edge of the obstacle. Material at index `i` would correspond to the edge from point `i` to point `i + 1` in the line renderer.
3. Default Material: If the above list is empty, all edges of the obstacle would have this material.

A material has the following two properties: Transmission Ratio and Reflection Ratio, which determine the portion of sound that goes through the edge, and the portion of sound that is reflected by the edge. Both are a value between 0 and 1.

`SoundSource > Properties` Game Object
1. Sound Model Method:
2. Max Volume:
3. Amplitude:
4. Decaying Factor:
5. Transfer Function Method:
6. Diffraction:
7. Decaying Diffraction:
8. Diffraction Static Caching:
9. Diffraction Greedy Search:
10. Diffraction Remove Redundant Corner:
11. Diffraction Map Partitioning:
12. Transmission:
13. Transmission Ray Count:
14. Reflection:
15. Reflection Ray Count:
16. Reflection Bounce Limit:
17. Reflection Minimum Range Limit:
18. Reflection Distance Delay Factor:
19. Audio Clip:
20. Output Channels:
21. Volume Based Stereo:
22. Audio Start Delay:
23. Reflection Audio Pool Count:
24. Reflection Maximum Audio Delay:
25. Visualize:
26. Visualize Source Position:
27. Visualize Beacon:
28. Visualize Range Circle:
29. Visualize Primary Source:
30. Visualize Diffraction:
31. Visualize Transmission:
32. Visualize All Sound Directions:
33. Visualize Weighted Sound Directions:
34. Update Each Frame:
35. Use Global Transmission Ratio:
36. Global Transmission Ratio:
37. Use Global Reflection Ratio:
38. Global Reflection Ratio:
39. Point, Line, Circle, Sound Mesh Pool Sizes: 

# Extending the Code
The class `SoundModelMethod` can be inherited to implement a new sound propagation model by implementing the following functions.
1. `ComputeModel`: Builds the sound model that is later used for audio computation.
2. `ComputeAudio`: Determines the output audio by choosing between stereo/mono, calling `ComputeSound` for the main audio output, etc.
3. `ComputeSound`: Uses the pre-built sound model to compute the amplitude for a certain listener.
4. `VisualizeModel`: Visualizes the computed model.

Our model's implementation can be found in the class `OurStaticMethod`, which uses the above functions to compute the model and the sound.

Implementation of the virtual sources can be found in the following files:<br>
`DiffractionSource.cs`<br>
`ReflectionSource.cs`<br>
`TransmissionSource.cs`<br>

The primary source, which runs the algorithms discussed in the paper and creates the virtual sources can be found in `PrimarySoundSource.cs`.
