# pgm_map_creator
Create pgm map from Gazebo world file for ROS localization

## Environment
Tested on Ubuntu 19.04, ROS Melodic, Boost 1.65

## Usage

### Add the package to your workspace
0. Create a catkin workspace
1. Clone the package to the src folder
2. `catkin_make` and source `devel/setup.bash`

### Add the map and insert the plugin
1. Add your world file to world folder
2. Add this line at the end of the world file, before `</world>` tag:
`<plugin filename="libcollision_map_creator.so" name="collision_map_creator"/>`

### Create the pgm map file

Open a terminal, run gazebo with the world file

```bash
gazebo src/pgm_map_creator/world/<world file>

```

Open another terminal, launch the request_publisher node

```bash
roslaunch pgm_map_creator request_publisher.launch \
    map_name:=<pgm_map_name> \
    save_folder:=<folder_where_to_save_map> \
    xmin:=<x_min value> \
    xmax:=<x_max value> \
    ymin:=<y_min value> \
    ymax:=<y_max value>
```

Wait for the plugin to generate map. It will be located in the map folder.

### Save map generation parameters

To save the parameters )`xmin`, `xmax`, `ymin`, `ymax`, `resolution`) to a yaml file:

```bash
roslaunch pgm_map_creator dump_map_gen_param.launch
```

## Map Properties

To use the map, the map proeprties need to be specified in a `[pgm_map_name].yaml`.
Input the following parameters in the file:

```bash
image: <pgm_map_name>.pgm
resolution: <resolution>
origin: [<xmin>, <ymin>, 0.000000]
negate: 0
occupied_thresh: 0.65
free_thresh: 0.196
```

## Acknowledgements
[Gazebo Custom Messages](http://gazebosim.org/wiki/Tutorials/1.9/custom_messages)
[Gazebo Perfect Map Generator](https://github.com/koenlek/ros_lemtomap/tree/154c782cf8feb9112bc928e33a59728ca2192489/st_gazebo_perfect_map_generator)

