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
4. You can now run any of the demo scenes.

# Creating a New Scene
1. Add the "SoundPropagation" prefab to your scene.
2. Define the sound geometry. <br>
  I. You can load the geometry from any maps on https://movingai.com/benchmarks/grids.html by setting the following values in the inspector for the GeometryData game object. <br> <br>
      Load Map From File => True <br>
      File Name => Directory of the ".map" file downloaded. The path is relative to Assets/Resources/Maps. <br> <br>
  II. You can create custom geometries by adding "SoundObstacle" prefabs as children of the GeometryData game object. <br>
3. Sound sources can be added using the "SoundSource" prefab.
