# lib_sign
**lib_sign** is a simple library allowing you to write on the Sign GUI/Screen.

**Current Version:** 1.0.1

**Updated On:** January 11th, 2026

**Supported Minecraft Versions**: 1.21.5, 1.21.7, 1.21.8 (1.21.10 and 1.21.11 at a later date)

---
## Table of Contents
- [Requirements](#requirements)
- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [API](#api)
---

## Requirements
- One of the following Minecraft versions:
	- 1.21.5
	- 1.21.7
	- 1.21.8
- One of the following Mod Loaders:
	- Fabric
 	- NeoForge
- Minescript 5.0 (i.e. Pyjinn support)
- Install mappings (run `\install_mappings` once in-game)

## Features
### Implemented
- Write in signs without external automation libraries;
- Per-character, randomized delays;
- Optional automatic line wrapping (currently very basic).

### Planned
- Per-word line wrapping;
- Deleting already-written text.

## Installation
- Download `lib_sign.pyj` and `lib_sign.py (optional)`.
- Place the file(s) in your `minescript` directory.

## Quick Start
### With `lib_sign.py` (Recommended):
```python
import time
from minescript import echo
from lib_sign import Sign

min_delay = 0.05
max_delay = 0.3
autowrap = False
close_screen_when_done = False

if not Sign.write("line 1\nline 2\nline 3\nline 4", min_delay, max_delay, autowrap, close_screen_when_done):
	echo("Do something if writing didn't start")
	return
while Sign.is_writing(): time.sleep(0.1)
echo("Finished writing!")
Sign.close_screen()
```

### Without `lib_sign.py`:
```python
import time
from minescript import echo
from java import import_pyjinn_script
lib_sign = import_pyjinn_script("lib_sign.pyj")
Sign = lib_sign.get("Sign")()

min_delay = 0.05
max_delay = 0.3
autowrap = False
close_screen_when_done = False

if not Sign.write("line 1\nline 2\nline 3\nline 4", min_delay, max_delay, autowrap, close_screen_when_done):
	echo("Do something if writing didn't start")
	return
while Sign.is_writing(): time.sleep(0.1)
echo("Finished writing!")
Sign.close_screen()
```

## API
### `Sign`
This is **lib_sign**'s main (and only) class.


### `Sign.write()`
This method allows you to write text in a sign screen.
Keep in mind it expects a sign screen **with no text** to already be open.
#### Arguments
- `text: str`
	- Text to be writen.
	- If it exceeds the max length of a Sign line, that line will get cut off - use `"\n"` to avoid this.
	- Alternatively, use at the `autowrap` argument.
-  `min_delay: float`
	- The minimum delay between keystrokes/characters.
	- Default Value: `0.05`
- `max_delay: float`
	- The maximum delay between keystrokes/characters.
	- Default Value: `0.1`
- `autowrap: bool`
	- If True, will attempt to autowrap the text.
	- Could still cause the text to be cut off if it is too long to fit in 4 lines.
	- Default Value: `False`
	- **Note**: With the current implementation, **words may be split across lines**.
		- Example: `"supercalifragilisticexpialidocious"` gets wrapped as `["supercalifragilisti", "cexpialidocious"]`
- `close_screen_when_done: bool`
	- If True, will close the screen once writing has finished.
	- Equivalent to calling `Sign.close_screen()` after the writing is finished.
	- Default Value: `False`
#### Returns
`success: bool` - `True` if writing started successfully, `False` otherwise (i.e. if no sign screen was found)


### `Sign.is_writing()`
This method allows you to know if there is a writing process ongoing.
Mainly used to block script execution until writing has finished.
#### Arguments
None.
#### Returns
`is_writing: bool` - `True` if an ongoing writing process exists, `False` otherwise.


### `Sign.close_screen()`
This method allows you to close the current sign screen.
#### Arguments
- `screen: Screen | None`
	- The sign screen to close.
	- If none is provided, the currently opened screen will be obtained from `mc`.
#### Returns
- `success: bool` - `True` if sign screen closed successfully, `False` otherwise (i.e. no sign screen found).
