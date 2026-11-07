# K1C Lubrication Routine

A G-code file and Klipper macro for the Creality K1C that run the motion system through its full travel after you lube it. Grease the rods and lead screws, run this, and the motors spread it out evenly.

There are two ways to use it:

- `K1C_Lubrication_Routine.gcode`: send it to the printer like any print. No config changes needed.
- `lubricate_axes.cfg`: a `LUBRICATE_AXES` macro you add to your Klipper config, with adjustable cycle count and speed.

## What it does

Homes the printer (the macro skips this if it's already homed), then runs two cycles of:

- Full-width X sweeps at three Y positions
- Full-length Y sweeps at three X positions
- Corner-to-corner diagonals
- XY moves with small Z changes mixed in
- Long Z travel up and down

All moves stay inside the K1C's travel limits (X 0–225, Y 2–223, Z 15–245). XY moves run at 200 mm/s and Z-only moves at 15 mm/s. It finishes centered at Z100. No heaters are used.

## Before you run it

1. Lubricate the rods and lead screws per Creality's maintenance guide.
2. Clear the build plate and make sure the nozzle is cool.
3. Stay with it. If anything binds or grinds, stop it.

## Option 1: G-code file

Send `K1C_Lubrication_Routine.gcode` to the printer however you normally send prints, and start it like a print.

## Option 2: Klipper macro

You'll need access to your printer's config files, which on the K1C usually means root.

1. Paste the contents of `lubricate_axes.cfg` into `gcode_macro.cfg` or `printer.cfg`. Or copy the file next to `printer.cfg` and add `[include lubricate_axes.cfg]`.
2. Restart Klipper with `FIRMWARE_RESTART`.
3. Run it from the console:

```
LUBRICATE_AXES
LUBRICATE_AXES CYCLES=3
LUBRICATE_AXES CYCLES=2 SPEED=9000
```

`CYCLES` defaults to 2. `SPEED` is the XY feedrate in mm/min, since it's used as the F value, so the default of 12000 is 200 mm/s. Z-only moves stay at 15 mm/s regardless.

The macro prints progress with `RESPOND`, which needs a `[respond]` section in your config. If you don't have one, add it or the cycle messages won't show.

## Notes

Written for the K1C. Other printers, including others in the K1 family, may have different travel limits. Check yours before running it.

## License

MIT
