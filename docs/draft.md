# Project requirements
- how many images?
- image size? 700x700 480x480
- is rotation / translation in 3D?
- same filters and variations for every image?
- use Blender to convert STL do FBX in order to import to Unreal
(or use http://www.greentoken.de/onlineconv/ if data is not confidential)

Some responses
Annotation with coords + masks
Parameters
Images in a table
Scale differences
Translation
Movimeents around the screen
Rotation in any direction (2 axis)
Scale 0.8 and 1.1 where (0.8 if the screen)


# UnrealCV - Getting started
## Installing plugin

export plugin_folder=$HOME/workspace/unrealcv

### Clone the UnrealCV repository
git clone https://github.com/unrealcv/unrealcv ${plugin_folder}

### Switch to version `v0.3.10`
cd {plugin_folder}
git checkout v0.3.10

### Build the plugin binary
export UE4="/Users/Shared/Epic Games/UE_4.14/"
python build.py --UE4 ${UE4}

### Create a new project and copy plugin to the project folder
export project_folder="$HOME/Documents/Unreal Projects/MyProject"

### Copy the compiled plugin to the project folder to install it.
cp -r "${plugin_folder}"/Plugins/ "${project_folder}"/Plugins/

### For changing the scene, need to comebine UE4Editor and UnrealCV


## Snippet for connecting to the game
    from unrealcv import client
    client.connect()
    if not client.isconnected():
        print('UnrealCV server is not running. Run the game downloaded from http://unrealcv.github.io first.')
        sys.exit(-1)
    
    # Test connection
    res = client.request('vget /unrealcv/status')
    # The image resolution and port is configured in the config file.
    print(res)

## Snippet for sending a command to server
For rotation, create a string like this:
# 'vset /camera/0/rotation pitch yaw roll'
cmd = 'vset /camera/0/rotation %s' % ' '.join([str(v) for v in rot])
client.request(cmd)



### Useful commands

    # Set viewmode to object mask
    vset /viewmode [viewmode]
    (v0.2) Set ViewMode to (lit, normal, depth, object_mask)

    vget /objects
    (v0.2) Get the name of all objects

    vset /object/[obj_name]/location [x] [y] [z]
    Set object location [x, y, z]

    vset /object/[obj_name]/rotation [pitch] [yaw] [roll]
    Set object rotation [pitch, yaw, roll]

    vset /object/[obj_name]/color [r] [g] [b]
    (v0.2) Set the labeling color of an object

    vset /object/[str]/show
    (v0.3.10) Show object

    vset /object/[str]/hide
    (v0.3.10) Hide object

# Sending commands to server

