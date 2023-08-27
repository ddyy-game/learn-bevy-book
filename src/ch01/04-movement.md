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
  - However, there are many entities that can have the same type of component. What if we only want to make changes to just a few of them?

    - The solution is, we _tag_ these entites with a custom component:

      ```rust
      #[derive(Component)]
      struct Fox;  // a custom component; we use it as a tag

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

  - Then we can make changes only to `Transform` components of entities that also have a `Fox` component attached:
    ```rust
    fn move_fox(
        // query more than one components with Query<(...)>
        mut query: Query<(&Fox, &mut Transform)>
    ) {
        for (_, mut transform) in &mut query {
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

- Trigger an animation when moving [TODO]
  - https://github.com/bevyengine/bevy/blob/main/examples/animation/animated_fox.rs
