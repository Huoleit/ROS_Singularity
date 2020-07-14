## ROS - Singularity container

[gazebo_ros_demos](https://github.com/Swarm-UST/gazebo_ros_demos) is included as an example.

### Usage

### Install Singularity

Please follow this [link](https://sylabs.io/guides/3.5/user-guide/quick_start.html#quick-installation-steps) and install Singularity first.

### Get started: Pull or build from scratch

```
mkdir ~/sin
cd ~/sin
```
#### Method 1: Pull the container from library

```
singularity pull library://zfuaj/default/te_demo
```

#### Method 2: Build the container from a definition file

First, download the definition  file:
```
wget https://raw.githubusercontent.com/Swarm-UST/ROS_Singularity/master/ros-melodic.def
```

Then, build the container:

```
sudo singularity build te.sif ros-melodic.def
```

> If `git` fail to pull the repo, please pull the [repo](https://github.com/Swarm-UST/gazebo_ros_demos) manually and replace the corresponding sections of `ros-melodic.def` properly.

An example of replaced def file:

```
%setup
    mkdir -p ${SINGULARITY_ROOFS}/ws/src

    # comment git command
    # git clone -b develop_singularity https://github.com/Swarm-UST/gazebo_ros_demos.git ${SINGULARITY_ROOFS}/ws/src/sim

# uncomment file section and replace <*PATH OF GIT REPO*>
%files
    /home/gazebo_ros_demos /ws/src/sim
```

### Run the container

If you have an NVIDIA GPU, run this:

```
mkdir -p /tmp/gazebo_singularity/dconf && XDG_RUNTIME_DIR=/tmp/gazebo_singularity singularity run -p --nv te.sif 
```

Or, if you have an AMD GPU, run this:
```
mkdir -p /tmp/gazebo_singularity/dconf && XDG_RUNTIME_DIR=/tmp/gazebo_singularity singularity run -p --rocm --bind /etc/OpenCL te.sif 
```

> Caution: I haven't test the command for AMD GPU, since I do not have a AMD GPU

You can also shell into the container by runing(NVIDIA GPU):
```
mkdir -p /tmp/gazebo_singularity/dconf && XDG_RUNTIME_DIR=/tmp/gazebo_singularity singularity shell -p --nv te.sif 
```

Do not forget to source ros setup file before running the ros commands.
