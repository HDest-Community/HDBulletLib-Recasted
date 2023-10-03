# Notes

This must be loaded after Hideous Destructor and before all addons.

## Contents

This library is limited only to the following:

- Bullets
- Loose Ammo (+ Boxes)
- Spawning

Magazines are out of this library's scope. On its own, the library only provides a framework. However, due to how HD generates items for ammo boxes and backpacks, options have been provided to remove those items. By default, no ammo from this mod will show up in backpacks and ammo boxes.

It's better to opt-in than it is to opt-out in this case. Check out the menu.

Currently Includes:

- 12 Gauge Slugs, .500 S&W Lights and Heavies, .420 Frei and .069 Bore by the Peppergrinder team
- .50 OMG by Bogus
- .45 ACP and 4-Gauge Buckshot by Popguy
- 10mm Auto, .45 LC, Golden .45 LC, Less-Lethal Shells, Explosive Slugs and Flare Shells by Swampyrad
- .56 Caliber Musket Balls by Cozi and Ted
- 4-Gauge Saboted Slugs by Khan
- 5mm Bumblebee Rimfire, 6mm Flechettes and .50 Action-Mega by Potetobloke.
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
