# Gmod Addon Loader aka gLoader

Forget about manually including each file on your addon, this module does all of this for you, *and more*, with a single function call.

## Features

- Automatic file including with support for specific realms based on the file/folder suffix:
    - Using the `_sv` suffix will prevent loading on the client-side.
    - Using the `_cl` suffix will prevent loading on the server-side.
    - Using the `_sh` suffix will force loading for both realms.
    - If none of the above, it'll assume the file or folder is shared.
    - With folders, this behavior will affect every single inside of it.
    - Files and folders are loaded alphabethically.
    - Within a scope, files will be loaded before the folders.

- Automatic namespace creation based on directory names:
    - Every folder will create a table within the global addon table using the exact same structure.
    - These tables will be created with a PascalCase format (i.e. `foldername` will create `Foldername`).
    - In order to have more than one word, you can use underscores in the folder name (i.e. `folder_name` will create `FolderName`).
    - Folders called `core` will be loaded before the rest *without* creating a new namespace table.
    - Since the namespace structure is created *before* the files are loaded, you can safely reference them without having to worry about `nil` indexing.
    - If a namespace table is not used, it'll instantly be removed. No need to worry about garbage within your global addon table.

- Automatic addon specific hook creation:
    - When your addon is done loading, the hook `addonName_OnAddonLoad` will be called.
    - Your addon's global table will be provided as the only argument for this hook.

- Automatic addon reload console command creation:
    - When your addon is loaded, the console command `addonName_reload` will be created.


## How to Use

1. Place the `gloader.lua` file on your addon's folder following the same structure provided by this repository (`yourAddon/lua/includes/module/gloader.lua`).
2. Create a file inside `yourAddon/lua/autorun` specific for your addon to load.
3. Inside of this file, call `require("gloader")` to initialize the gLoader module. Right after that, call `gloader.Load(addonName, addonFolder)`.
4. Your addon is now loaded and ready to use, enjoy!


## The Load function

This module comes with a single global function:

```lua
gloader.Load(addonName :: string, addonFolder :: string)
```
- `addonName` will define the name of the addon used by the console loading progress prints, reload console command and global addon table creation.
- `addonFolder` will define the folder inside `lua/` where all the files and folders will be loaded from.


## Credits

- [Stooberton aka "Steve"](https://github.com/Stooberton) for the original idea, originally implemented in [ACF-3](https://github.com/Stooberton/ACF-3).