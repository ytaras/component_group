# component_group

[![Crates.io](https://img.shields.io/crates/v/component_group.svg)](https://crates.io/crates/component_group)
[![Docs.rs](https://docs.rs/component_group/badge.svg)](https://docs.rs/component_group)
[![Build Status](https://travis-ci.com/sunjay/component_group.svg?token=i5M6iNHVbWshsp6jHWxw&branch=master)](https://travis-ci.com/sunjay/component_group)
[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/sunjay)

This crate defines the `ComponentGroup` trait. This trait is used to make
managing a group of [`specs::Component`] instances easier. This is useful for
when you have several components that are often created, read, and updated
together. You can use this trait to easily move an entire group of components
between instances of [`specs::World`].

Instead of having you keep duplicate code in sync across your application, this
trait groups all of that logic in one place so you can focus and minimize the
changes you need to make every time you add a component to the group.

The `ComponentGroup` trait can be automatically derived if each component in the
group implements the `Clone` trait. This removes any of the boilerplate you may
have needed to write in order to implement the trait yourself.

See [**the documentation**][docs] for more details about the motivations for
creating this trait and how to use it.

```rust
// Don't forget to add the component_group crate to your Cargo.toml file!
use component_group::ComponentGroup;

use specs::{World, Component, VecStorage, HashMapStorage};
use specs_derive::Component;

// These components are just for demonstration purposes. You should swap them
// out for your own. Components need to be Clone to use the automatic derive.

#[derive(Debug, Clone, Component)]
#[storage(VecStorage)]
pub struct Position {x: i32, y: i32}

#[derive(Debug, Clone, Component)]
#[storage(VecStorage)]
pub struct Velocity {x: i32, y: i32}

#[derive(Debug, Clone, Component)]
#[storage(VecStorage)]
pub struct Health(u32);

#[derive(Debug, Clone, Component)]
#[storage(HashMapStorage)]
pub struct Animation {frame: usize}

// This is all of the code you need to write to define the group and its operations!
#[derive(ComponentGroup)]
struct PlayerComponents {
    position: Position,
    velocity: Velocity,
    health: Health,
    // This optional component is allowed to not be present
    animation: Option<Animation>,
}

// Now you can easily add all of these components to an entity, load them all
// from the world, or even update them all at once!
```

[`specs::Component`]: https://docs.rs/specs/*/specs/trait.Component.html
[`specs::World`]: https://docs.rs/specs/*/specs/world/struct.World.html
[docs]: https://docs.rs/component_group
