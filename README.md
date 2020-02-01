# Python MPV JSONIPC

This implements an interface similar to `python-mpv`, but it uses the JSON IPC protocol instead of the C API. This means
you can control external instances of MPV including players like SMPlayer, and it can use MPV players that are prebuilt
instead of needing `libmpv1`. It may also be more resistant to crashes such as Segmentation Faults, but since it isn't
directly communicating with MPV via the C API the performance will be worse.

The win32_named_pipe module is from [KomodoEdit](https://github.com/Komodo/KomodoEdit) and is available
under the terms of the Mozilla Public License.

Please note that this only implements the subset of `python-mpv` that is used by `plex-mpv-shim` and
`jellyfin-mpv-shim`. Other functionality has not been implemented.

## Installation

```bash
sudo pip3 install python-mpv-jsonipc
```

## Basic usage

Create an instance of MPV. You can use an already running MPV or have the library start a
managed copy of MPV. Command arguments can be specified when initializing MPV if you are
starting a managed copy of MPV.

```python
from python_mpv_jsonrpc import MPV

# Uses MPV that is in the PATH.
mpv = MPV()

# Use MPV that is running and connected to /tmp/mpv-socket.
mpv = MPV(start_mpv=False, ipc_socket="/tmp/mpv-socket")

# Uses MPV that is found at /path/to/mpv.
mpv = MPV(mpv_location="/path/to/mpv")

# After you have an MPV, you can read and set (if applicable) properties.
mpv.volume # 100.0 by default
mpv.volume = 20 

# You can also send commands.
mpv.command_name(arg1, arg2)

# Bind to key press events with a decorator
@mpv.on_key_press("space")
def space_handler():
    pass

# You can also observe and wait for properties.
@mpv.property_observer("eof-reached")
def handle_eof(name, value):
    pass

# Or simply wait for the value to change once.
mpv.wait_for_property("duration")
```
