# miniverse_explorer

A space simulator built with Unity using real astronomical data.

Everything you see in the simulator exists in space. I am aiming to keep object sizes and distances as accurate as possible, but this is still a work in progress and mistakes may happen.

This version is currently in alpha. If you find any bugs, please report them in the Issues section.

The source code is not public yet, though I may share it later.

## Run

1. Extract the archive: `miniverse<version>`
2. Run: `space map`

## Controls

There are two modes: **Map Mode** and **Ship Mode**.

Press `V` to switch between modes.

### Map Mode (default)

- `+`: Zoom in
- `-`: Zoom out
- `Scroll`: Zoom in or out
- `Middle Mouse Button`: Rotate
- `F`: Search for an object
- `V`: Switch view

### Ship Mode

- `H`: Show or hide help
- `W`: Move forward
- `S`: Move backward
- `A`: Move left
- `D`: Move right
- `Shift`: Warp speed
- `Q`: Rotate left
- `E`: Rotate right
- `V`: Switch view

## Cache

The simulator downloads data from `https://www.darksitedb.space`, so an internet connection is required the first time you run it. Images and data are then cached locally.

This allows me to update the dataset more easily.
