---
linkTitle: Asset Cache Server Mode
title: Asset Cache Server
description: Enable a feature to reduce asset builds for a team.
weight: 200
toc: true
---

The **Asset Server Cache** mode is a feature allows an **Asset Processor** to cache product asset files to a remote folder so that other team members can skip long asset processing times.

The **Asset Server Cache** (ACS) mode lets the **Asset Processor** fetch pre-processed products of a job from an asset server cache instead of processing it locally. It does this by using a shared folder where the **Asset Processor** writes out product asset archive ZIP files for a processed source asset job. The other **Asset Processor** clients retrieve these ZIP files and unzip the product assets instead of processing the source asset from scratch.

# Setting Up Asset Processor in Asset Cache Server Mode

The **Asset Cache Server** (ACS) mode has three steps to set up the system so that developers can use it.

{{< note >}}
The next sections assume the root folder to be `T:/o3de` but it can be in any folder or it can run on Linux.
{{< /note >}}

## Create the Transfer Folder

The team will need to create a shared folder that all the team members will be able to access over the network. The tech should work between Windows and Linux, but the cacheServerAddress string only accepts a single string such as `T:/o3de/Transfer/Cache` that all the clients are meant to read from. It is possible to be clever about having an **Asset Processor** in ACS mode write to one configured `cacheServerAddress` string via Windows and having another ACS retrieve from a Samba link on Linux; but that will be a challenge the team will need to solve.

## Run the Asset Processor in ACS in Server Mode

The design of the ACS mode is to have one machine contributing to the asset cache in the remote folder and other **Asset Processor** clients running retrieve the cached product asset archive files.

To enable the **Asset Processor** in ACS mode as a server, a .setreg file needs these settings:
```
{
  "Amazon": {
    "AssetProcessor": {
      "Settings": {
        "Server": {
          "enableCacheServer": true,
          "cacheServerAddress": "T:/o3de/cache_server",
          "ACS FBX Glob": {
            "glob": "*.fbx",
            "checkServer":  true
          }
        }
      }
    }
  }
}
```

This will enable the cache server mode for an Asset Processor where it will write out the cached archive files to `T:/o3de/cache_folder` for all FBX files.

The setting key `/AssetProcessor/Settings/Server/enableCacheServer=true` enables the **Asset Processor** to run in "server mode". In this mode, the **Asset Processor** will detect source asset changes, process the product assets, archive the product files (in a ZIP archive), and save out the archive files in the remote folder.

The setting key `/AssetProcessor/Settings/Server/cacheServerAddress=<remote_shared_path>` points to a remote folder that the server and all clients can read and write from, respectively. The transfer folder should already be set up before launching the **Asset Processor**.

The setting key `/AssetProcessor/Settings/Server/ACS FBX Glob={}` object specifies that the FBX source assets as a file type to cache. There can be a number of entries specified in the settings registry where the entry’s title needs to start with the letters ACS such as "ACS Our Textures" and "ACS Audio Files". The "glob" pattern can be used to capture files by extension or some basic matching pattern. It is important to flag the entry with "checkServer" to true to enable the entry for caching.


## Run the Other Asset Processors in ACS in Client Mode

To run the **Asset Processor** in ACS mode as a client the machine will access to the remote folder, enable the cache system in client mode, and specify the asset file types to pull from the remote server.

```
{
  "Amazon": {
    "AssetProcessor": {
      "Settings": {
        "Server": {
          "enableCacheServer": false,
          "cacheServerAddress": "T:/o3de/cache_server",
          "ACS FBX Glob": {
            "glob": "*.fbx",
            "checkServer":  true
          }
        }
      }
    }
  }
}
```

The setting key `/AssetProcessor/Settings/Server/enableCacheServer=false` enables the **Asset Processor** to run in `client mode`. In this mode, the **Asset Processor** will read the source asset changes, check the remote folder for the product archives, and process the asset if it was not found. It is possible to have a hybrid of remote and local source assets since end users will add new assets locally before submitting them to a remote source asset repository.

The cache server address and ACS specifier blocks should match the **Asset Cache Server**.

## Configuring an ACS Block

The asset caching system is configured using opt-in patterns. There are many types of files that process faster than copying an archived file from a remote folder. The most common way to cache the products of a source asset file is using a "glob" wild card pattern like "*.png" and "*.wav" scan patterns. Another way is to add a regular expression to match the source assets that would take a long time to process using "pattern" such as "\/assets\/rock_[\w]*\.asset" to cache all the rock files.


```
"ACS title":
{
   "glob":  wild card pattern, // i.e. *.fbx
   "pattern": a regular expression, // i.e. [\w]*\.asset
   "checkServer": Boolean // to enable set to true
}
```

This ACS (**Asset Cache Server**) block allows users to configure the types of source assets that should be cached in the remote folder. The block is placed in the JSON path `/AssetProcessor/Settings/Server` object. The title must start with the prefix "ACS" to designate the object as a configuration block. The next part is either "glob" or "pattern" followed by the correct text for a wild card pattern or a regular expression; these are used to tag source assets that need to be cached. 

{{< note >}}
The block should only be set to "glob" or "pattern" not both.
{{< /note >}}

The "checkServer" Boolean flag is used to enable the block. The default value for "checkServer" is false, so to enable the ACS block the Boolean flag needs to be set to true.