# Simulasi Path Planning dengan RRT*
RRT* (Rapidly-Exploring Random Tree Star) adalah algoritma berbasis sampling dan pohon untuk melakukan path planning dalam robotika.
![gazebo](https://github.com/user-attachments/assets/3feda605-df53-490f-8a7d-91ab975ca23a)
![path-pic](https://github.com/user-attachments/assets/b0b42fcd-6bce-44be-8c20-e016ada242e9)

## Environment and Dependencies
1. Ubuntu 22.04
2. ROS2 Humble
3. Gazebo Classic
4. Turtlebot3 (dengan package turtlebot3 dan turtlebot3_msgs)

## External Links
1. https://github.com/mmcza/TurtleBot-RRT-Star
2. https://emanual.robotis.com/docs/en/platform/turtlebot3/simulation

## Simulasi
### SLAM
Perintah untuk menjalankan SLAM dan membuat map:
#### Terminal 1: Launch Simulation World
```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```
#### Terminal 2: Run SLAM Node
```
$ export TURTLEBOT3_MODEL=burger
$ ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```
#### Terminal 3: Run Teleoperation Node
```
export TURTLEBOT3_MODEL=burger
ros2 run turtlebot3_teleop teleop_keyboard
```

#### Save Map
```
ros2 run nav2_map_server map_saver_cli -f ~/map
```

### Navigation
#### Ubah param burger.yaml
```
    # GridBased:
    #   plugin: 'nav2_navfn_planner/NavfnPlanner'
    #   tolerance: 0.5
    #   use_astar: false
    #   allow_unknown: true

    planner_plugin_types: ['nav2_rrtstar_planner::RRTStar'] # For Foxy and earlier
    planner_plugin_ids: ['GridBased'] # For Foxy and earlier
    plugins: ['GridBased'] # For Galactic and later
    use_sim_time: True
    GridBased:
      plugin: nav2_rrtstar_planner/RRTStar # For Galactic and later
      interpolation_resolution: 0.01
```

### Launch Simulation World
```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

### Run Navigation Node
```
export TURTLEBOT3_MODEL=burger
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=$HOME/map.yaml
```

### Estimate Initial Pose
1. Klik 2D Pose Estimate di menu RViz2
2. Klik posisi robot di map

### Set Navigation Goal
1. Klik Navigation2 Goal di menu RViz2
2. Klik posisi di map untuk set goal dan navigation akan berjalan
