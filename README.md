# LCM to ROS

This package is to subscribe to and republish LCM messages into ROS messages.

The goal is to make a relatively generic system that requires only LCM message files
and automatically generate republishers that subscribe to an LCM topic and republish 
the same messages onto a ROS topic.

This package automatically generates ROS message files, CPP files, a launch file and
corresponding CMakeLists for catkin_make to create a set of republishers.

The user should only need to create the lcm message files.

Note that this code is currently restricted to a limited number of data types, namely
numbers, it has not been tested with strings (TODO)


## Installation:

Install LCM:
```
git clone https://github.com/lcm-proj/lcm lcm
cd lcm
./bootstrap.sh
./configure
make
sudo make install
```

Clone this repository (assuming default catkin_make workspace):
```
cd ~/catkin_ws/src
git clone git@github.com:nrjl/lcm_to_ros.git
```

Add lcm messages to the 'lcm' folder. Note that the filenames are important; the autogenerated code assumes that the name of the lcm topic is the filename for that message type. So for a file called FNAME.lcm in the lcm subdirectory, the system will subscirbe to the lcm topic FNAME, which it assumes is using message type specified in FNAME.lcm, and messages will be republished onto the ROS topic `/lcm_ros_republisher/FILENAME` Also note that the repo ignores files in the lcm directory, so that your message types will not be committed to the repo.

Run the bash script and then make:
```
cd lcm_to_ros
chmod +x lcm_to_ros_generate.sh
./lcm_to_ros_generate.sh
cd ~/catkin_ws
catkin_make
```

This will parse the lcm subdirectory for .lcm files, and for each one attempt to:
    - create the lcm C++ header in the exlcm subdirectory (using `lcm-gen`)
    - create a corresponding ROS message (in the msg subdirectory)
    - create C++ republisher code (in the autosrc subdirectory)
    - add an entry to autosrc/CMakeLists.txt
    - add an entry to launch file launch/all_republishers.launch
    
Once complete, the publishers can be run with:
`roslaunch lcm_to_ros all_republishers.launch`

This basic version assumes

