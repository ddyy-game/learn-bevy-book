# See through a camera

- We are now already seeing the 3D game world in the window, through a _camera_
- The mechanism of a camera
  - without a camera, the world is just a bunch of data describing what everything is and where they are
  - a camera uses such information to calculate what we should see from a certain point of view
  - specifically, given object shapes, positions, environment lightings, etc., the camera will compute colors of each pixels on the screen, as if we were seeing things directly from there; this process is called _rendering_
- Let's move our camera to a more comfortable position [TODO]
  - https://bevy-cheatbook.github.io/features/coords.html
  - https://bevy-cheatbook.github.io/features/transforms.html
- Then, we add some lights to make this scene look more natural [TODO]
  - https://github.com/bevyengine/bevy/blob/main/examples/3d/load_gltf.rs
- Also change background color to give us a more lively feel [TODO]
