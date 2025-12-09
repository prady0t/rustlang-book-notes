

```rust

use pixi_manifest::{PrioritizedChannel, SpecType};


use rattler_conda_types::{NamedChannelOrUrl};
```

```rust

  

// Specify channel

#[arg(long)]

pub channel: Option<String>,
```

```rust

  

// Add a channel if specified

if args.channel.is_some() {

let channel = args.channel;

workspace.manifest().add_channels(

[

PrioritizedChannel::from(NamedChannelOrUrl::Name(channel.unwrap_or_default()))

.clone(),

],

&FeatureName::DEFAULT,

false,

)?;

}
```

```rust

for i in dependency_config.specs().into_iter() {

for (_, spec) in i {

let Some(channel) = spec.channel.as_ref().and_then(|c| c.name.as_ref()) else {

break;

};

  

workspace.manifest().add_channels(

[

PrioritizedChannel::from(NamedChannelOrUrl::Name(channel.clone()))

.clone(),

],

&FeatureName::DEFAULT,

false,

)?;

}

}
```