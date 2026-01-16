# README for Socket CAN Configuration Files

This directory contains ROS 2 parameter configuration files for the ros2_socketcan package.

## Files

- **socket_can_default.yaml**: Default configuration with basic settings (CAN standard frame)
- **socket_can_vcan0_with_filter.yaml**: Virtual CAN interface with ID filtering enabled
- **socket_can_fd.yaml**: CAN FD (Flexible Data-rate) configuration with bus time

## Usage

### Launch with default configuration
```bash
ros2 launch ros2_socketcan socket_can_bridge.launch.xml
```

### Launch with custom configuration file
```bash
ros2 launch ros2_socketcan socket_can_bridge.launch.xml \
  config_file:=/path/to/custom_config.yaml
```

### Override specific parameters via CLI
```bash
ros2 launch ros2_socketcan socket_can_bridge.launch.xml \
  interface:=vcan0 \
  enable_frame_loopback:=true \
  'ignored_incoming_ids:=[0x9DEFB001,0x9DEFB000]' \
  ignore_incoming_ids:=true
```

## Parameters

### Interface and Hardware
- **interface**: CAN interface name (e.g., 'can0', 'vcan0')
- **enable_can_fd**: Enable CAN Flexible Data-rate mode (default: false)
- **enable_frame_loopback**: Enable frame loopback (default: false)

### Timing
- **interval_sec**: Receiver polling interval in seconds (default: 0.01)
- **timeout_sec**: Sender timeout in seconds (default: 0.01)
- **use_bus_time**: Use CAN bus time instead of system time (default: false)

### Filtering
- **filters**: CAN ID filters in candump format (default: "0:0" - accept all)
- **ignore_incoming_ids**: Enable CAN ID filtering (default: false)
- **ignored_incoming_ids**: List of CAN IDs to ignore (empty by default)
  - Format: decimal integers or hex with 0x prefix
  - Example: [2641416193, 2641416192] or [0x9DEFB001, 0x9DEFB000]

### Topics
- **from_can_bus_topic**: Published CAN frames topic (default: "from_can_bus")
- **to_can_bus_topic**: Subscribed CAN frames topic (default: "to_can_bus")

## Creating Custom Configuration

1. Copy one of the existing YAML files
2. Modify parameters as needed
3. Launch with the custom config file

Example custom configuration for diagnostics:
```yaml
/**:
  ros__parameters:
    interface: "can0"
    enable_can_fd: false
    filters: "123:FFF"  # Only receive CAN ID 0x123
    use_bus_time: true
    ignore_incoming_ids: false
```
