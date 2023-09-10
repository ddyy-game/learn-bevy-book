# Move objects

- Till now, we've only been creating objects at the beginning of the game. Our game world is still uninterestingly static.

- To make changes to **entities** in our game world, we write a **system** that mutates the **components** of our concern. `ch01/step-4-1`

  - If we want to move something around, the component that we want to modify is `Transform`, which defines the position, rotation and scale of entities.
  - To mutate a certain type of component, write a normal Rust function with following signature:
    ```rust
    fn move_objects(mut query: Query<&mut Transform>) {
        for mut transform in &mut query {
            todo!("now you can mutate transform in place");
        }
    }
    ```
  - `Query` is one of the most important utility types in Bevy for checking and modifying the game world. You wrap the component type you want to operate in it, and Bevy will fill them in whenever this system gets called, making it possible for you to iterate over them and make modifications.
  - However, there are many entities that can have the same type of component (e.g. cameras also have `Transform`). What if we only want to make changes to just a few of them?

    - The solution is, we _tag_ these entites with a custom component:

      ```rust
      #[derive(Component)]
      struct Fox;  // an empty component; we use it as a tag

      fn setup() {
          // ...
          commands
            .spawn(SceneBundle {
                scene: asset_server.load("models/Fox.glb#Scene0"),
                transform: Transform::from_scale(Vec3::splat(0.1)),
                ..default()
            })
            .insert(Fox);  // tag this entity with Fox component
      }
      ```

  - Then we can make changes only to `Transform` components of entities that also have a `Fox` component attached (note the `With` type here; this part of a query is called query filters):
    ```rust
    fn move_fox(mut query: Query<&mut Transform, With<Fox>>) {
        for mut transform in &mut query {
            // let's foward the fox a little
            transform.translation.z += 0.1;
        }
    }
    ```
  - Finally, we decide _when_ this system is run. Let's start by running this system once per frame (i.e. on `Update`):
    ```rust
    fn main() {
        App::new()
          // ...
          .add_systems(Update, move_fox)
          .run();
    }
    ```
  - Now hit `cargo run`, you will see the fox slowly moving forward along the Z axis.

> Note:
>
> [The Unofficial Bevy Cheat Book](https://bevy-cheatbook.github.io/programming/intro-data.html) has a detailed explanation of the relationship between entities and componentes.

- Let's practice the skill we learn with another example. We can see the fox eventually move out of sight. Let's change this by always pointing the camera at the fox. `ch01/step-4-2`

  - This time we need to query two different things at the same time: the fox (for obtaining its location), and the camera (we want it to point at the fox).
  - Intuitively, we may want to write the function signature as follows:
    ```rust
    fn look_at_fox(
        mut camera: Query<&mut Transform, With<Camera>>,
        fox: Query<&Transform, With<Fox>>,
    ) {
       // do something...
    }
    ```
  - We are almost there. There is still one issue with the current signature. We now have two parameters querying the same component, and one of them is mutable. It is impossible for Bevy to know if they will overlap, and mess up immutables with mutables (this violates safety rules of Rust, and can cause unexpected behaviors).
  - To avoid this, we explicitly claim that the second parameter does not overlap with the first one, by adding a `Without<Camera>` filter:
    ```rust
    fn look_at_fox(
        mut camera: Query<&mut Transform, With<Camera>>,
        fox: Query<&Transform, (With<Fox>, Without<Camera>)>,
    ) {
       // do something...
    }
    ```
  - Now we can do what we want in the function body. Here we use `get_single` and `get_single_mut`, as we are sure there is only one entity each. We also use [`let-else` statement](https://doc.rust-lang.org/rust-by-example/flow_control/let_else.html) to extract both results at the same time.
    ```rust
    fn look_at_fox(
        mut camera: Query<&mut Transform, With<Camera>>,
        fox: Query<&Transform, (With<Fox>, Without<Camera>)>,
    ) {
        let Ok(mut camera_transform) = camera.get_single_mut() else { return };
        let Ok(fox_transform) = fox.get_single() else { return };
        camera_transform.look_at(
            fox_transform.translation + Vec3::new(0.0, 5.0, 0.0),
            Vec3::Y,
        );
    }
    ```
  - Hit `cargo run` and check the result.

- At this point, you may experience an odd feeling, that the fox seems to be standing still, and it is the camera that is moving. This illusion is caused by two main reasons:

  - the lack of running animation of the fox itself
  - the lack of reference points (like ground, trees, grass, etc.).

- We will handle this in the next two sections.
