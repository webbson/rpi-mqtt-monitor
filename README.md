# Raspberry Pi MQTT Monitor

![GitHub release (latest by date)](https://img.shields.io/github/v/release/hjelev/rpi-mqtt-monitor) ![GitHub repo size](https://img.shields.io/github/repo-size/hjelev/rpi-mqtt-monitor) ![GitHub issues](https://img.shields.io/github/issues/hjelev/rpi-mqtt-monitor) ![GitHub closed issues](https://img.shields.io/github/issues-closed/hjelev/rpi-mqtt-monitor) ![GitHub language count](https://img.shields.io/github/languages/count/hjelev/rpi-mqtt-monitor) ![GitHub top language](https://img.shields.io/github/languages/top/hjelev/rpi-mqtt-monitor)

The easiest way to track your Raspberry Pi or Ubuntu computer system health and performance in Home Assistant.

- Monitor: cpu load, cpu temperature, free space, used memory, swap usage, uptime, wifi signal quality, voltage, ip address and system clock speed.

- Supports discovery messages so no manual configuration in [Home Assistant](https://www.home-assistant.io/) configuration.yaml is needed.

- You can install it with just one command from shell.

- Configurable: You can select what is monitored and how the message(s) is send (separately or as one csv message)

## Installation

### Automated

Run this command to use the automated installation:

```bash
bash <(curl -s https://raw.githubusercontent.com/hjelev/rpi-mqtt-monitor/master/remote_install.sh)
```

Raspberry Pi MQTT monitor will be intalled in the location where the installer is called, inside a folder named rpi-mqtt-monitor.

The auto-installer needs the software below and will install it if its not found:

- python (2 or 3)
- python-pip
- git
- paho-mqtt

Only python is not automatically installed, the rest of the dependancies should be handeled by the auto installation.
It will also help you configure the host and credentials for the mqtt server in config.py and create the cronjob configuration for you.

### Manual

If you don't like the automated installation here are manuall installation instructions:

1. Install pip if you don't have it:

```bash
sudo apt install python-pip
```

2. Then install this python module needed for the script:

```bash
pip3 install paho-mqtt
```

3. Install git if you don't have it:

```bash
apt install git
```

4. Clone the repository:

```bash
git clone https://github.com/hjelev/rpi-mqtt-monitor.git
```

5. Rename `src/config.py.example` to `src/config.py`

## Configuration

(only needed for manual installation)
Populate the variables for MQTT host, user, password and main topic in `src/config.py`.

You can also choose what messages are sent and what is the delay (sleep_time is only used for multiple messages) between them.
If you are sending a grouped message, and you want to delay the execution of the script you need to use the `random_delay` variable which is set to 1 by default.
This is the default configuration:

```
random_delay = randrange(1)
discovery_messages = True
sleep_time = 0.5
cpu_load = True
cpu_temp = True
used_space = True
voltage = True
sys_clock_speed = True
swap = True
memory = True
uptime = True
wifi_signal = False
wifi_signal_dbm = False
ip_addr = True
```

If `discovery_messages` is set to true, the script will send MQTT Discovery config messages which allows Home Assistant to automatically add the sensors without having to define them in configuration.

## Test Raspberry Pi MQTT Monitor

Run Raspberry Pi MQTT Monitor (you might need to update the path in the command below, depending on where you installled it)

```bash
/usr/bin/python3 /home/pi/rpi-mqtt-monitor/rpi-cpu2mqtt.py
```

Once you run Raspberry Pi MQTT monitor there will be no output if it run OK, but you should get 8 or more messages via the configured MQTT server (the messages count depends on your configuration).

## Schedule Raspberry Pi MQTT Monitor execution

Create a cron entry like this (you might need to update the path in the cron entry below, depending on where you installed it):

```
*/2 * * * * /usr/bin/python /home/pi/rpi-mqtt-monitor/rpi-cpu2mqtt.py
```

## How to update

Navigate to the folder where Rapsberry Pi MQTT Monitor is installed and pull the git repository:

```bash
git pull
```
