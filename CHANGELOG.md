# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [v1.5.0] - 2025-07-01

### Added

-   Belt Links have been added from Potetobloke's Bangers & Mash.
-   9mm NDM has been added from Peppergrinder (#44).
-   Add loose round actors for .500 S&W Lights/Heavies (#40).
-   Added Backpack Filters for Belt Links (#42).

### Removed

-   .50 AE has been fully removed. (#44, #45).

## Changed

-   Updated Peppergrinder Menu Labels (#34).
-   Updated Spawner Menus to use Sliders instead of Option toggles (#35).
-   Fixed Slug Translations for FreeDoom & Lotan's Tomb (#38).
-   Added FD/LT-style Slug Ammobox (#41).
-   Replaced getName() with getTag() (#43).
-   .45 LC can now be multi-picked-up.

## [v1.4.0] - 2024-02-23

### Changed

-   Updated build scripts (#29).
-   Updated Serpentipede Classname for latest HDest Main (#30).

## [v1.3.4] - 2024-02-07

### Changed

-   Updated Build Scripts (#26).
-   Updated readme with proper mod title & nightly build status badge (#27).
-   Fixed build script (#28).

## [v1.3.3] - 2024-01-19

### Added

-   Added 20mm Grenade box pickup (#23).
-   Added proper credits section to Readme (#25).

### Changed

-   Update CVAR init check (#24).

## [v1.3.2] - 2023-12-14

### Changed

-   Tweaked Birdshot stats.
-   Tweaked 20mm Grenade stats (#22).

## [v1.3.1] - 2023-10-25

### Changed

-   Further refactor RemovedClasses CVAR init (#19).

## [v1.3.0] - 2023-10-20

### Changed

-   Updated Spawn Handler to properly use CheckReplacement logic.
-   Refactored .500 S&W casings (#16).
-   Refactored ammo init process (#16).
-   Corrected round mass stats for .45 ACP & .50 AM (#18).

## [v1.2.3] - 2023-07-30

### Changed

-   Fixed WAN Rocket Spawns (#15).
-   Tweaked 5mm Round sprite (#15).

## [v1.2.2] - 2023-07-04

### Changed

-   Fixed Build Scripts (#14).

## [v1.2.1] - 2023-07-04

### Changed

-   Fixed string comparisons being case-sensitive (#13).

## [v1.2.0] - 2023-07-04

### Added

-   Aded various Radtech Ammo Types (#3).
-   Added "Fumbled"/"Unspent" .50 OMG rounds, currently used by the AceCorp Wyvern (#1).
-   Updated Spawn Handler to utilize new BPSpawnPool, can now hide ammo types from spawning in backpacks (#8)!
-   Added 5mm Lose Rounds from HexaDoken's Legacy Continued (#11).

### Changed

-   Hide unused casing spawn options in Menu (#2).
-   Updated Spawn Handler (#10, #12).
-   Cleaned up Project Structure (#4).
-   Fixed .300 Savage Spawning (#5).
-   Add .300 Savage Casing & Individual Round spawning rules (#6).
-   Fixed spawning rules not accounting for all Actor classes (#7).
-   Fixed 7.62 Tokarev Casing spawn bias option in Menu (#9).

## [v1.1] - 2022-10-04

### Added

-   Added proper MENUDEF.

## [v1.0] - 2022-09-07

### Added

-   Initial Release.

[Unreleased]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.5.0...HEAD

[v1.5.0]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.4.0...v1.5.0

[v1.4.0]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.3.4...v1.4.0

[v1.3.4]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.3.3..v1.3.4

[v1.3.3]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.3.2..v1.3.3

[v1.3.2]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.3.1..v1.3.2

[v1.3.1]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.3.0..v1.3.1

[v1.3.0]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.2.3..v1.3.0

[v1.2.3]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.2.2..v1.2.3

[v1.2.2]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.2.1..v1.2.2

[v1.2.1]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.2.0..v1.2.1

[v1.2.0]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.1..v1.2.0

[v1.1]: https://github.com/HDest-Community/HDBulletLib-Recasted/compare/v1.0..v1.1

[v1.0]: https://github.com/HDest-Community/HDBulletLib-Recasted/releases/tag/v1.0
