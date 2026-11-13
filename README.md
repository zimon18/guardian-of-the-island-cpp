# Guardian of the Island

A console RPG built as a team project for an introductory C/C++ programming course. Explore an island, collect items, survive turn-based encounters and face its guardian.

| | |
| --- | --- |
| **Type** | Academic team project, four contributors |
| **Language** | C++17, CMake, Python 3 for CLI tests |
| **My documented part** | `Item` hierarchy and `MenuSystem` |
| **Platforms** | Windows and Ubuntu, built and tested in GitHub Actions |
| **Team repository** | [kacper-lebida/guardian-of-the-island-cpp](https://github.com/kacper-lebida/guardian-of-the-island-cpp) |

## My contribution

The team credits in the original project README identify my part as the **item hierarchy** and the **menu system**. The supplied project archive had no Git history, so this attribution follows the team's own documentation.

### Item hierarchy (`src/Item.h`, `src/Item.cpp`)

- An abstract `Item` base class with a name, a value and pure virtual `use()`, `getDescription()`, `getType()` and `getPower()`.
- Three concrete item types: `WeaponItem` raises attack, `HealingItem` restores HP and `ArmorItem` adds bonus HP.
- `getType()` and `getPower()` give every item a type tag and a numeric power. The save system uses them to write items to a text file and to rebuild the correct subclass when a game is loaded.
- The inventory stores items through base-class pointers, so it can use any item without knowing its concrete type. A new item type only needs a new subclass.

### Menu system (`src/MenuSystem.h`, `src/MenuSystem.cpp`)

- Displays the exploration menu: **E** explore, **I** inventory, **U** use item, **S** save, **Q** quit.
- Dispatches each choice to the matching `Game` method and reports invalid options.
- In the current version, input is read one line at a time, case is ignored and longer input is rejected, so typing `explore` does not trigger an action by accident. End of input is treated as quit. The end-of-input and input-recovery handling were tightened during portfolio preparation, described below.

## Gameplay

- 20 possible locations and three exploration outcomes: nothing, loot or combat.
- Six enemy types, with one to five enemies per combat location.
- Weapons increase attack; healing and armor items add HP. All items are consumed when used.
- Randomized attacks and probabilistic escape/item-use actions during combat.
- A final boss after ten completed locations.
- Manual saving to `save.txt` and automatic loading at startup.

## Structure

| Module | Responsibility | Credited to |
| --- | --- | --- |
| `Game` | Main loop, exploration, encounters, loot and boss progression | Kacper Lebida, Wiktor Kaczmarczyk, Tomasz Kraska |
| `Character` | Character statistics and inventory ownership | Tomasz Kraska |
| `Inventory` | Item storage, use and serialization | Kacper Lebida |
| `Item` | Abstract interface and weapon/healing/armor implementations | **Szymon Baran** |
| `SaveSystem` | Player and progress persistence | Kacper Lebida |
| `MenuSystem` | Exploration menu and command dispatch | **Szymon Baran** |
| `CombatSystem` | An unused placeholder; actual combat is implemented in `Game.cpp` | — |

## Build and run

Use a C++17 compiler. From the repository root with CMake:

```sh
cmake -S . -B build -DCMAKE_BUILD_TYPE=Debug
cmake --build build --config Debug
ctest --test-dir build -C Debug --output-on-failure
```

Run `./build/guardian` on Linux or `./build/Debug/guardian.exe` with a Visual Studio generator on Windows. Single-configuration Windows generators place the executable at `./build/guardian.exe`.

Alternatively, compile directly with GCC or Clang:

```sh
g++ -std=c++17 -Wall -Wextra -Wpedantic src/*.cpp -o guardian
./guardian
```

On Windows with MinGW, use `-o guardian.exe`. The save file is written to the working directory. Python 3 is required for the optional CLI smoke tests.

## Review status

The portfolio version fixes boss progression, invalid damage ranges, malformed save handling, input recovery and restart state. These fixes, the regression tests and the CI workflow were prepared by Kacper Lebida in the team repository; this repository carries the same commits.

CMake configuration, build and CTest run on both Windows and Ubuntu in this repository's [GitHub Actions workflow](../../actions/workflows/build.yml).

See [portfolio preparation and validation](docs/portfolio-preparation.md) for changes, test coverage and remaining limitations. The original coursework and the later improvements are documented separately.

## Team credits

Credits reproduced from the supplied project README:

| Contributor | Documented area |
| --- | --- |
| Kacper Lebida | Inventory, SaveSystem, game loop/state (`Game.h`) |
| Wiktor Kaczmarczyk | Turn-based combat (`Game.cpp`) |
| Szymon Baran | Item hierarchy and MenuSystem |
| Tomasz Kraska | Character, victory condition and loot |

No license was included in the supplied archive. This repository does not assign a new license to the team's code.
