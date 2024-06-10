# Elemental Channels

[Elemental Channels](https://elemental.docs.rancher.com/next/channels) can be used to publish a list of container images to be used by the [Elemental Operator](https://elemental.docs.rancher.com).  

This repository automates and facilitates the creation of the officially distributed Elemental channels.  

## Goals for this repository

1. Be the single source of truth for all channel .json files that need to be published by the Elemental team
1. Automatically refresh the channels watching container registries (Daily)
1. (Optional) Publish images on GitHub, for development or testing

## Repository Watches config

The [config.yaml](./config.yaml) can be updated using the following structure:

```yaml
watches: 
    # A flavor for the Base OS being watched. Can be "" for unflavored images.
    # This will be used as a prefix to distinguish same versions of different flavors.
  - flavor: "my-flavor"
    # The resulting .json filename on the ./channels directory
    fileName: "sle-micro-5-5-my-flavor"
    # The OS human readable name
    displayName: "SLE Micro 5.5 My Flavor"
    # The repository containing the "os" type images
    osRepo: registry.suse.com/suse/sle-micro/my-flavor-5.5
    # The repository containing the "iso" type images.
    # If this is not applicable, use "N/A"
    isoRepo: registry.suse.com/suse/sle-micro-iso/my-flavor-5.5
    # How many images to limit per (minor) version.
    limit: 3
```

## Usage

It is possible, at any moment, to run the `.refresh_channels.sh` script and integrate the changes, if any.  
A GitHub [workflow](.github/workflows/refresh-channels.yaml) does it automatically every night, and optionally it can be triggered at any time.  

Manually crated `.json` files can be directly created and maintained in the `./channels` directory, for example to maintain a channel containing arbitrary images, for development or testing.  
When this is the case, be mindful of not creating collision with the automated [config.yaml](./config.yaml), otherwise files with the same name will be overwritten.  
