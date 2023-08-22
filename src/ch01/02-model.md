# Import a model

- Setup a window, a main game loop, and other main features with `DefaultPlugins`. **`ch01/step-2-1`**

  - Bevy at its core is just a skeleton for supporting the ECS paradigm.
  - A new Bevy app contains nothing; we add ECS to it to give it meanings.
  - A plugin is a pack of ECS items that work together to handle a specific aspect of the game.
  - Window management, game loop, input management, logging, are all supported through some default plugins.

- [Download](https://github.com/bevyengine/bevy/raw/main/assets/models/animated/Fox.glb) a model to `assets/models/Fox.glb`.

  - You can design your 3D model _visually_ with software like Blender.
  - Then you can export it to formats like glTF (or its binary version `.glb`) to use it _programmatically_ in Bevy.

- Load this model in Bevy app **`ch01/step-2-2`**

  - In the code we add `setup` as a Bevy system at `Startup` phase of the game (in Bevy, systems are just functions).
  - Scene is kind of like a container: we can put one or more models, lights and cameras in it. It can be exported by 3D modeling software, and loaded by Bevy.
  - Here we load a scene with an `AssetServer`, and spawn it in `setup`:
    ```rs
    commands.spawn(SceneBundle {
        scene: asset_server.load("models/Fox.glb#Scene0"),
        ..default()
    });
    ```

- Observe it in the window with a camera

  - let's set up a default camera for now
    ```rs
    commands.spawn(Camera3dBundle::default());
    ```

- Now run the game with `cargo run`, you'll see something like this:

![](./ch01_2_2.png)

> Note about the `setup` system:
>
> - In Bevy, we define normal Rust functions to represent all kinds of behaviors in the game world. In terms of ECS, these functions are often called systems.
> - We mainly have two concerns about systems:
>   - What are they supposed to do?
>   - When are they executed?
> - Simply put, a system takes something in a game world, and do something with them.
>   - In our `setup` system (the name is arbitrary), we take access to the asset manager, and load an external model from the asset folder. Then, with mutable access to `Commands`, we generate (spawn) the loaded model and a camera in the game world.
>   - The access to what we need in Bevy (e.g. all components of a certain type, some global resources), is acquired through the typing of the function parameter. We will see a lot more examples later.
> - When you register your function as a system using `add_systems`, you are also describing when to run this system. Typical choices are `Startup` (run once at start up) and `Update` (run one time each frame).
