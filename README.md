NintendoSpy
======

#### [Download the latest NintendoSpy release here.](https://github.com/jaburns/NintendoSpy/releases/latest)

This project provides a general solution for live-streaming your controller inputs while speedrunning, or recording inputs for tutorials on how to perform tricks.  It supports tying in to NES, SNES, Nintendo 64, and GameCube controller signals to get a live view of them, as well as any gamepad connected to your PC for use with emulators.  XBox 360 controllers are supported with a skin out of the box, but other gamepads will require creating a skin.

NintendoSpy supports custom skins using a straight-forward XML-based skin format.  You can also bind controller input combinations to trigger keypresses for hitting checkpoints on your splits.  If you create your own skins, feel free to submit them as pull requests to this repository.

## Documentation

### Wiring and hardware

The general design of NintendoSpy involves splicing the controller wire, and attaching the appropriate signal wires to an Arduino.  Then you just need to install the Arduino firmware packaged in the NintendoSpy release, and run the viewer software.  For more in-depth tutorials on how to do this, check out some of the links below.

![](https://github.com/jeremyaburns/NintendoSpy/raw/master/docs/tutorial-images/wiring-all.png)

[EvilAsh25's SNES hardware building guide](https://github.com/jaburns/NintendoSpy/blob/master/docs/guide-evilash25.md)

[Gamecube hardware tutorial](https://github.com/jaburns/NintendoSpy/blob/master/docs/tutorial-gamecube.md)

[N64 hardware tutorial](https://github.com/jaburns/NintendoSpy/blob/master/docs/tutorial-n64.md)

### Using the viewer software

Once you've unzipped the NintendoSpy release, run NintendoSpy.exe to open the controller viewer.  You'll be greeted by the input source configuration screen, which is fairly straightforward to configure.  First select the console your NintendoSpy hardware is set up to view, "PC 360", or "Generic PC Gamepad".  For the latter 2 choices, COM port is irrelevant since the device is expected to simply interface over standard USB.  If you select a Nintendo console however, you'll have to select the COM port the Arduino is communicating over.  Honestly, the easiest way to figure this out is to just try each port.  There are never many in the list, and it does no harm to pick the wrong one other than you won't see any inputs.

![](https://github.com/jeremyaburns/NintendoSpy/raw/master/docs/tutorial-images/interface.png)

Once you've selected the input source, you'll have to pick a skin.  Each skin can have multiple backgrounds, which are generally used to provide various colors for the controller itself.  After picked a skin, hit ``Go!`` and you should see the viewer screen.

### Creating your own skins

Each skin consists of a subfolder in the "skins" directory, which is expected to contain a file called ``skin.xml`` along with all the PNG image assets required by the skin.  The easiest way to create a skin for your target console is probably just to copy+paste the default skin and modify it according to your needs.  What follows is a thorough documentation of the skin.xml format for reference if you'd like to create more complex skins.

The root node of the ``skin.xml`` file must define the following 3 attributes:
```
<skin name="Default PC 360" # This is the name of the skin as it will appear in the selection list.
      author="jaburns"      # Your name or handle.
      type="pc360">         # The input type this skin is used for.
```
Valid values for the ``type`` attribute are as follows: ``nes``, ``snes``, ``n64``, ``gamecube``, ``pc360``, ``generic``. 

Each skin must define at least one ``<background>`` element.  Each background entry will be listed in the skin selector as a separate entry.  Every background for your skin must have the same dimensions.
```
<background name="Default"     # The name which will appear in the selection list.
            image="pad.png" /> # The PNG file to use for this background selection.
```
The rest of the ``skin.xml`` file defines how to render button and analog inputs.  Each type exposes a variety of buttons and analog values.  Button inputs are mapped to images at specific locations using the ``<button>`` tag, and there is a small variety of possible ways to map analog inputs.

```
<button name="up"          # Name of the source input button to map this image to.
        image="circle.png" # Image file to display when this input is pressed.
        x="101" y="71"     # Location in pixels where the top/left corner of the image should sit.
        width="16"         # width and height specification can OPTIONALLY be used to scale
        height="16" />     #   an image to a specific size.  The default size is the orignal image size.
```

### Binding controller inputs to keyboard key presses

