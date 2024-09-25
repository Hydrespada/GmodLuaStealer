![image](https://github.com/user-attachments/assets/2c1ddb2f-19e1-4714-bbef-15b0bf11960d)


# GmodLuaStealer



gluasteal retrieves client-side & shared Lua files from Garry's Mod servers that you join. gluasteal also allows you to execute your own Lua code on any server.

gluasteal is a better, cross-platform replacement for various Lua dumpers and loaders; such as scripthook.

![image](https://github.com/user-attachments/assets/ebf3178d-df89-49c6-8243-1fb4e22511fa)

# Features ?

Lua Dumper - Saves Lua files that are executed
Lua Executor - Allows you to execute your own Lua scripts
Lua Blocker - Block Lua scripts of your choosing
Concurrent IO - The Lua dumper does IO in its own thread to eliminate bottleneck and ensure maximum in-game performance
Robust protections against servers serving malicious file paths
Easily configurable

# How to use ?

Download from the Releases section.
Inject into Garry's Mod at the main menu. 
Optionally, set up your own Lua file to be loaded.
Join a server.

!
The gluasteal Directory
Logs and Lua files will be written to the gluasteal folder, in your home directory. You may create the folder if it does not already exist.

Windows: C:/Users/username/Documents/gluasteal/

Linux: /home/username/gluasteal/

macOS: /Users/username/gluasteal/

# How to inject ?

# For Windows
Use Extreme Injector, jector, GuidedHacking Injector, and many more.

# For Linux
- https://github.com/gaffe23/linux-inject

# For MacOs

Use LLDB


pid=1234
lib_path="/full/path/to/libgluasteal.dylib"

sudo lldb --attach-pid $pid --batch \
    -o "p (void*)dlopen(\"$lib_path\", 1)" \
    -o detach \
    -o quit

    # Lua Loader 
    
*Create the file 'gluasteal.lua' in the gluasteal directory; you can place your Lua code you wish to execute in here. This file is executed in a separate environment, not in _G, Though you will still able to access everything in _G.*

This file will be executed every time a Garry's Mod Lua script is about to be executed. You can return false to stop the current file (stored in gluasteal.SCRIPT) from being executed.

*Examples*
-- Stops scripts with the string 'derma' in their path from executing.
if (gluasteal.SCRIPT:match("derma")) then
	return false
end
-- Makes the code inside the if statement only execute once, before the first Lua file is loaded
-- also known as 'load before autorun'
if gluasteal.SCRIPT == "lua/includes/init.lua" then
    -- your code here
    
    -- e.g. execute the script "cc_les_mec_script.lua" in your gluasteal directory
    gluasteal.include("cc_les_mec_script.lua")
end
gluasteal variables and functions
gluasteal.SCRIPT -- The path of the Garry's Mod Lua script that is about to be executed. e.g. lua/includes/init.lua
gluasteal.SOURCE -- The source code of the script that is about to be executed. e.g. do return end
gluasteal.VERSION -- The version of gluasteal being used
gluasteal.include -- A function to execute other gluasteal Lua files, relative to the gluasteal directory. e.g. gluasteal.include("other.lua")
Note that gluasteal.SCRIPT and gluasteal.SOURCE will be an empty string in files included by gluasteal.include.

# Configuration

glua-steal can be configured through the config.toml file in the gluasteal directory.

The configuration file will automatically be created and filled with the default values if it does not exist.

Please open an issue if you would like more options to be available through the configuration file.

Example (default) configuration:

[general]

# Options for the file stealer/dumper
[stealer]
enabled = true # Enable or disable
write_mode = "truncate" # can be "truncate" or "append" - see issue #35 for info

# Options for the Lua loader
[loader]
file = "gluasteal.lua" # The file which will be run every time a Garry's Mod script is executed - relative to the glua-steal directory

# Options for the logger
[logger]
level = "info" # can be one of: trace, debug, info, warn, error, critical
