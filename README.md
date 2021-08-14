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
1. Sound Model Method: The sound model method to use.
2. Max Volume: A number between 0 and 1 to scale the output audio.
3. Amplitude: The amplitude of the source.
4. Decaying Factor: `pdf` from the paper.
5. Transfer Function Method: Currently can choose between two different sound propagation function method (equation 2 in paper). Can be easily extended for more options.
6. Diffraction: Toggles sound diffraction.
7. Decaying Diffraction: A variation of diffraction in which sound loses more energy the more it bends. (Might be buggy)
8. Diffraction Static Caching: Caching information for higher-order diffraction sources as an optimization method. (Might be buggy)
9. Diffraction Greedy Search: An optimization for diffraction. Discussed in paper.
10. Diffraction Remove Redundant Corner: An optimization for diffraction. Discussed in paper.
11. Diffraction Map Partitioning: An optimization for model computation. A spatial partitioning method.
12. Transmission: Toggles sound transmission.
13. Transmission Ray Count: The number of transmission rays to shoot.
14. Reflection: Toggles sound reflection.
15. Reflection Ray Count: The number of reflection rays to shoot.
16. Reflection Bounce Limit: The maximum number of bounces for a reflection ray.
17. Reflection Minimum Range Limit: The minimum amplitude required for a ray in order to bounce.
18. Reflection Distance Delay Factor: The distance factor in computing sound delay in reflection. Corresponds to `rdf` from paper.
19. Audio Clip: The audio soundclip to play.
20. Output Channels: Can choose between stereo and mono.
21. Volume Based Stereo: The stereo sound would be based on both sound direction and sound volume.
22. Audio Start Delay: An offset for the soundclip to play.
23. Reflection Audio Pool Count: The number of audio sources to reserve for outputing reflection.
24. Reflection Maximum Audio Delay: The maximum delay that can be produced by reflection.
25. Visualize: Toggles visualization.
26. Visualize Source Position: Visualizes the sound source location.
27. Visualize Beacon: Provides a better visualization of the sound source location in 3D mode.
28. Visualize Range Circle: Visualizes the range of the sound source.
29. Visualize Primary Source: Visualizes the visibility polygon.
30. Visualize Diffraction: Visualizes the regions coverd by diffraction.
31. Visualize Transmission: Visualizes the regions covered by transmission.
32. Visualize All Sound Directions: Visualizes all diffraction/transmission sources toward the listener. (Used for stereo)
33. Visualize Weighted Sound Directions: Visualizes the final sound direction. (Used for stereo)
34. Update Each Frame: The sound model is only computed when a parameter is changed. This option would force it to compute on every frame.
35. Use Global Transmission Ratio: Can be enabled to ignore all surface material of the geometry and use a global value as the transmission ratio.
36. Global Transmission Ratio: The transmission ratio of each edge in the geometry, if the above option is enabled.
37. Use Global Reflection Ratio: Can be enabled to ignore all surface material of the geometry and use a global value as the reflection ratio.
38. Global Reflection Ratio: The reflection ratio of each edge in the geometry, if the above option is enabled.
39. Point, Line, Circle, Sound Mesh Pool Sizes: A pool of resources to optimize the visualization. If the pool is exhausted, visualization may become slow. Should be specified before run-time.

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
