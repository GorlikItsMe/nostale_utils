# NosTale utils

## Installation:
```shell script
pip install pywin32 psutil
```

## Example 1:
Inject packet logger into all Nostale instances, hide packetlogger and rename Nostale client to this format 
`Nostale CHAR_ID: {character_name} PL_PORT: {pl_port}`
![alt text](https://raw.githubusercontent.com/romansvozil/nostale_packet_logger_utils/master/images/example_1.PNG "Example 1")

```python
from utils import setup_all_clients, read_current_name
from asyncio import run

pid_port_pairs = run(setup_all_clients())

for pid, port in pid_port_pairs:
    print(f"{read_current_name(pid)}: \t{port}")

# Output: 
# InGameName: 	55154
```

## Example 2:
Creating PacketLoggerWrapper instance and waiting on map change
```python
from utils import setup_all_clients, PacketLoggerWrapper
from asyncio import run

async def wait_for_map_change():
    ports = await setup_all_clients()
    packet_logger = PacketLoggerWrapper(ports[0][1])
    packet_logger.serve()
    while True:
        print("Waiting for map change.")
        if await packet_logger.wait_for_packet(lambda _packet: _packet[1] == "c_map"):
            print("Map have been changed.")

run(wait_for_map_change())
```

## Example 3:
If you want to hide packetlogger windows, you can do it like this.
```python
from utils import hide_window, get_packet_logger_windows 

for window in get_packet_logger_windows():
    hide_window(window)
```

## Example 4:
Read Nostale client names without having to inject packetlogger
```python
from utils import get_nostale_windows, read_current_name

pid_name_pairs = [(window["pid"], read_current_name(window["pid"])) for window in get_nostale_windows()]
```

# Links:
- [PacketLogger](https://www.elitepvpers.com/forum/nostale-hacks-bots-cheats-exploits/4297215-release-packetlogger.html)
- [Injector](https://github.com/numaru/injector/blob/master/injector.pya)