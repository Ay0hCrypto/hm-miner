# hm-miner: Helium Miner Container

This is the codebase for the Helium miner container.

This uses the Fork and Merge Methods provided by Github to pull Nebra's modified version of Helium's 
base image created by Helium from their [Quay repo](https://quay.io/repository/team-helium/miner?tab=tags) (built from the [GitHub source](https://github.com/helium/miner)) and make some edits to optimise it for use on various hardware including: adding sys.config edits beyond Nebra's initial Rocks DB edits.

## Miner config file update
Due to issues discovered during the lite hotspot transition we include NEBRA's following script, so customized sys.configs will be rewritten
when updates are pushed.
In the `start-miner.sh` script in this repo, you will see that we [update the included sys.config file](https://github.com/NebraLtd/hm-miner/blob/master/start-miner.sh#L9-L22) every time the miner container is loaded, using the below code:

```shell
wget \
    -O "/opt/miner/releases/$HELIUM_GA_RELEASE/sys.config" \
    "${OVERRIDE_CONFIG_URL}"
```
This version uses the helium snapshot.

## Miner GC setup
These edits are currently retained in the Test Version. Future Rocks-DB edits will cause this to change.
We have set 2 variables related to garbage collection to help improve the storage usage and performance of miners. 
`BLOCKCHAIN_ROCKSDB_GC_BYTES` Which is being set as an environment variable in the dockerfile
`blocks_to_protect_from_gc` Which is being set in sys.config under the blockchain block config

The values we have used for these 2 are the following: `blocks_to_protect_from_gc` is set to 4000 to give it some good buffer without removing too many blocks on GC trigger and `BLOCKCHAIN_ROCKSDB_GC_BYTES` is set to 8GB which means it will prioritze cleaning up files 

`blocks_to_protect_from_gc` is being set in hm-block-tracker [here](https://github.com/NebraLtd/hm-block-tracker/)


## Pre built containers

This repo forks docker containers for easy access:
- [hm-miner on DockerHub](https://hub.docker.com/r/nebraltd/hm-miner)
- [hm-miner on GitHub Packages](https://github.com/NebraLtd/hm-miner/pkgs/container/hm-miner)

The images are tagged using the docker long and short commit SHAs for that release with architecture `arm64` or `amd64` as a prefix. The current version deployed to miners can be found in the [helium-miner-software repo](https://github.com/Ay0hCrypto/Ay0h-HNT-Firmware/ay0h-hnt-miner-firmware/blob/production/docker-compose.yml).
