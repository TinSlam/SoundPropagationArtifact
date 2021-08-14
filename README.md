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

# Extending the Code
The class `SoundModelMethod` can be inherited to implement a new sound propagation model by implementing the following functions.
1. `ComputeModel`: Builds the sound model that is later used for audio computation.
2. `ComputeAudio`: Determines the output audio by choosing between stereo/mono, calling `ComputeSound` for the main audio output, etc.
3. `ComputeSound`: Uses the pre-built sound model to compute the amplitude for a certain listener.
4. `VisualizeModel`: Visualizes the computed model.

Our model's implementation can be found in the class `OurStaticMethod`, which uses the above functions to compute the model and the sound.

Implementation of the virtual sources can be found in the following files: `DiffractionSource.cs`, `ReflectionSource.cs`, `TransmissionSource.cs`
