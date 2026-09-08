# Beneath the stars

A space simulator built with Unity using real astronomical data.

Everything you see in the simulator exists in space. I am aiming to keep object sizes and distances as accurate as possible, but this is still a work in progress and mistakes may happen.

This version is currently in alpha. If you find any bugs, please report them in the Issues section.

## Run

1. Extract the archive: `beneath_the_starsv<version>.zip`
2. Run: `space map`

# Getting Started with Beneath the Stars

*Beneath the Stars* is a space simulator with two ways to explore:

- **Free Roam** lets you move between map and ship views and explore at your own pace.
- **Scenario** plays scripted `.bts` cinematics such as the bundled `demo` tour.

This guide gets the demo running and then uses it to introduce scenario authoring.

## Run the demo

To run a compiled build, launch it once and follow the same **Play > Scenario > demo** menu path.

## Useful controls

Press **H** in either gameplay view to show the in-game control reference. These are the default bindings and can be changed under **Settings > Controls**.

### Map view

| Action | Default input |
| --- | --- |
| Rotate view | Hold middle mouse button and move the mouse |
| Step rotation | Arrow keys |
| Zoom | Mouse wheel, `+`, or `-` |
| Search for an object | `F` |
| Show an object's description | Left-click |
| Move to an object | Double left-click |
| Switch to ship view | `V` |

### Ship view

| Action | Default input |
| --- | --- |
| Increase/decrease throttle | `W` / `S` |
| Strafe left/right | `A` / `D` |
| Move up/down | Space / Left Ctrl |
| Steer | Mouse or arrow keys |
| Roll left/right | `Q` / `E` |
| Boost | Left Shift |
| Look around without steering | Hold right mouse button |
| Switch to map view | `V` |

Press **Escape** to open the gameplay menu. During a Scenario, this also pauses the scripted timeline. Press the backtick key (`` ` ``) to open the Runtime Debug Console; enter `Scenario` in its Filter field to see only scenario validation and playback messages.

## What the demo teaches

The bundled `demo.bts` is a complete example of the scripting system:

1. `@start` loads `SolarSystem` directly and places the ship at an explicit coordinate.
2. `show-ship`, `show-ship-ui`, and `ship-sound` prepare a clean cinematic view.
3. `show-image` and `waitForSpace=true` create an interactive title card.
4. `start-recording`, `play-music`, and `play-sound` add capture and audio.
5. `parallel` synchronizes movement, rotation, text, and sound.
6. `lock` keeps the ship in a moving target's reference frame while it approaches or orbits that target; `unlock` releases it.
7. `show-video` and `show-orbits` add presentation elements during the Solar System sequence.
8. `teleport` moves the tour through `MilkyWay`, `Sagittarius A Star`, and `UniverseMap`.
9. `stop-recording` safely finalizes the MP4, and `wait-for-space` holds the closing frame.
10. When the queue ends, Scenario mode returns to the main menu automatically.

## Create your first scenario

Create a UTF-8 text file named `my-first-tour.bts` with this content:

```text
@start map="SolarSystem" x=-31.78577 y=461 z=-1364

show-ship visible=false
show-ship-ui visible=false
ship-sound enabled=false

lock target="Earth"
parallel {
    look-at target="Earth" duration=2
    go-to-object target="Earth" speed=200 stopDistance=20 acceleration=40 deceleration=60
    write-text {
        text="Welcome to my first space tour." duration=3
        text="Movement and text are running together." duration=4
    }
}

rotate-around-object target="Earth" duration=8 degreesPerSecond=10 faceTarget=true axis=y
unlock

show-ship-ui visible=true
ship-sound enabled=true
wait-for-space
```

Lines normally run in order. Commands inside `parallel` start together, and the next line waits until every command in that block has finished. Braces must be on their own lines, parallel blocks cannot be nested, and names or text containing spaces must be quoted.

Do not add `teleport map="Menu"` at the end; the game adds the return to the main menu for you.

## Install and test a scenario

Run the game once so it creates the writable content folders. Place `my-first-tour.bts` directly in the `Scenarios` folder for your operating system:

| Operating system | Scenario folder |
| --- | --- |
| Windows | `%USERPROFILE%\AppData\LocalLow\DarksiteDb\beneath the starts\Scenarios` |
| macOS | `~/Library/Application Support/DarksiteDb/beneath the starts/Scenarios` |
| Linux | `${XDG_CONFIG_HOME:-$HOME/.config}/unity3d/DarksiteDb/beneath the starts/Scenarios` |

Then open **Play > Scenario** and select **my-first-tour**. If the Scenario list was already open, select **Back** and reopen it to refresh the entries.

The game also creates sibling `Music`, `Images`, and `Videos` folders. Put custom files directly in the appropriate folder, without subfolders:

| Content | Supported formats | Example command |
| --- | --- | --- |
| Music and sound effects | `.ogg`, `.mp3`, `.wav` | `play-music file="track.ogg" volume=0.7 loop=true fade=1` |
| Images | `.png`, `.jpg`, `.jpeg` | `show-image path="title.png" duration=5` |
| Videos | `.mp4`, `.m4v`, `.mov`, `.webm`, `.ogv` | `show-video path="intro.mp4"` |

Recording output is written beneath the sibling `Recordings` folder. A command such as `start-recording file="captures/my-tour" type=mp4` creates any requested subfolder and preserves an existing recording by adding a numeric suffix.

For content shipped with a build, developers should instead use:

- `Assets/StreamingAssets/Scenarios`
- `Assets/StreamingAssets/Music`
- `Assets/StreamingAssets/Images`
- `Assets/StreamingAssets/Videos`

A writable user file with the same name as a bundled file takes precedence.

## Common problems

- **The scenario is not listed:** confirm the file ends in `.bts`, is directly inside the writable `Scenarios` folder, and refresh the Scenario list.
- **The scenario does not start:** open the Runtime Debug Console, filter by `Scenario`, and check the reported filename and line number.
- **A target cannot be found:** use a valid object name from the active scene. Lookup is case-insensitive, but names containing spaces must be quoted.
- **Music, images, or video cannot be found:** include the file extension, use only a top-level filename, and place the file in the matching writable folder.
- **A scene cannot be loaded:** use an enabled scene name: `UniverseMap`, `MilkyWay`, `SolarSystem`, or `"Sagittarius A Star"`. `Menu` cannot be used with `@start`.
- **Parallel movement looks unstable:** avoid running unrelated transform-changing commands together. The supported cinematic pairing is `go-to-object` with `rotate-around-object` using the same locked target.

## Some screenshots

<img width="1353" height="906" alt="Screenshot 2026-05-21 235707" src="https://github.com/user-attachments/assets/227557e9-b121-49fc-b0a2-b600f23b3f5b" />
<img width="1368" height="936" alt="Screenshot 2026-05-21 235752" src="https://github.com/user-attachments/assets/3e88e0ee-bf95-4bdb-9249-e20e33bc3b70" />
<img width="2326" height="1908" alt="Screenshot 2026-05-05 193830" src="https://github.com/user-attachments/assets/31015403-ec94-4689-848a-6f0e2600429d" />
<img width="2359" height="1719" alt="Screenshot 2026-05-05 194033" src="https://github.com/user-attachments/assets/511437ee-640a-4558-9617-ee26bb81dec0" />







