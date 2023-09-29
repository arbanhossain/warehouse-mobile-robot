Find the Laptop and Pi IP by typing `ifconfig` in respective terminal windows.

SSH into Raspberry Pi from Laptop: `ssh bot@$ip_address$`. (Open 4-5 SSH terminals)

Make sure communication between Laptop ROS and Pi ROS is working.

Add the following to your `.bashrc`:

For Pi:

```bash
export ROS_DOMAIN_ID=1
export ROS_MASTER_URI=https://localhost:11311/
export ROS_HOSTNAME=$RaspberryPiIPaddr$
export ROS_IP=$RaspberryPiIPaddr$
```

For Laptop:

```bash
export ROS_DOMAIN_ID=1
export ROS_MASTER_URI=https://$RaspberryPiIPaddr$:11311/
export ROS_HOSTNAME=$LaptopIPaddr$
export ROS_IP=$LaptopIPaddr$
```

If ROS nodes from one machine do not show up on the other, execute:

```bash
ros2 daemon stop
ros2 daemon start
```

on both machines. If the issue persists, restart both machines.

### On Pi

Give permission to ports:

- `sudo chmod 777 /dev/i2c-*`
- `sudo chmod 777 /dev/gpiomem`
- `sudo chmod 777 /dev/ttyUSB0`
- `sudo chmod 777 /dev/ttyACM*`

Launch ROS packages and nodes:

- Robot system: `ros2 launch basic_mobile_robot basic_mobile_bot_v3.launch.py use_simulator:=False use_sim_time:=False`
- Lidar: `ros2 launch rplidar_ros rplidar_a1_launch.py scan_mode:=Standard frame_id:=base_link`
- Map: `ros2 launch slam_toolbox online_async_launch use_sim_time:=False`
- Controller-ROS interface: `ros2 launch serial_stuff motor_pub /dev/ttyACM0`
- LFR-ROS interface: `ros2 run gui_pack gui`

---

**Switch to LFR mode**: `ros2 topic pub --once /gui std_msgs/msg/String "{data: 'lfr'}"` (Execute again after table is picked up.)

**Put down Car Jack**: `ros2 topic pub --once /gui std_msgs/msg/String "{data: 'car'}"` (Execute again after car jack is down.)

### On Laptop

- Controller: `rqt_robot_steering`
