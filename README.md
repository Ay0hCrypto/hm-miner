# hm-miner: Helium Miner Container

This is the codebase for the Helium miner container.

This uses the Fork and Merge Methods provided by Github to pull Ay0h's modified version of Helium's 
base image created by Helium from their [Quay repo](https://quay.io/repository/team-helium/miner?tab=tags) (built from the [GitHub source](https://github.com/helium/miner)) and make some edits to optimise it for use on various hardware including: adding sys.config edits, as well as using Nebra's open-sourced GC and Diagnostic template, for now.
