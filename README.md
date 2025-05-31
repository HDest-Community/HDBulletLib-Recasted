# HDBulletLib: Recasted

[![Nightly Build](https://github.com/HDest-Community/HDBulletLib-Recasted/actions/workflows/nightly.yml/badge.svg)](https://github.com/HDest-Community/HDBulletLib-Recasted/actions/workflows/nightly.yml)

## Notes

This must be loaded after Hideous Destructor and before all addons.

## Contents

This library is limited only to the following:

- Bullets
- Loose Ammo (+ Boxes)
- Spawning

Magazines are out of this library's scope. On its own, the library only provides a framework. However, due to how HD generates items for ammo boxes and backpacks, options have been provided to remove those items. By default, no ammo from this mod will show up in backpacks and ammo boxes.

It's better to opt-in than it is to opt-out in this case. Check out the menu.

Currently Includes:

- .50 AE by Accensus
- 12 Gauge Slugs, .500 S&W Lights and Heavies, .451 Frei, .066 Bore, and 9mm NDM by the Peppergrinder team
- .50 OMG by Bogus
- .45 ACP and 4-Gauge Buckshot by Popguy
- 10mm Auto, .45 LC, Golden .45 LC, Less-Lethal Shells, Explosive Slugs and Flare Shells by Swampyrad
- .56 Caliber Musket Balls by Cozi and Ted
- 4-Gauge Saboted Slugs by Khan
- 5mm Bumblebee Rimfire, 6mm Flechettes, .50 Action-Mega and Belt Links by Potetobloke.
- 20mm Grenades, Thunder Rockets and Tortoise Rockets by Wanzer.
- .30-06 by HexaDoken.

## Addition Requirements

To likely have your round be added to HDBulletlib-Recasted, you should preferably follow these guidelines:

- Keep the sprites in the general art style of vanilla Hideous Destructor.
- Have your round be free of startup errors and potential crashes.
- Check if it works with Purge Useless Ammo completely.
- Preferably have at least one gun either planned to be released or to be released with the round for it.
- Proper language, menu and event handler support based on HDBulletLib-Recasted's features.

If your round follows all of those guidelines, feel free to make a pull request from y our own fork/branch and it'll likely get added!

## Future to-dos

- Migrate all of the menu strings into a LANGUAGE lump instead.
- Migrate all of the pickup messages and tags to a LANGUAGE lump instead.
- Add more community created munitions.
- Add presets for spawns to allow easier setup for new users.

## Credits

_Unfortunately most of the assets in this library were added before being maintained by the community without proper credits.  If you see something that is uncredited and know who should be attributed to making it, reach out or create an issue so it can be fixed!_

- 20mm Grenade Box Pickup: Zrrion, recolored by Wanzer.
