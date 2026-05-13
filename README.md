# TowerDefenseGame-OOP
# Tower Defense Game — OOP Semester Project

**National University of Computing & Emerging Sciences**
Object Oriented Programming 
Semester Project
## What is this project?

A fully playable Tower Defense game built in C++ using the SFML graphics library.
Enemies walk along a fixed path from entry to exit. The player places defensive towers
beside the path to shoot and kill enemies before they escape. The player earns gold by
killing enemies and spends it to place towers. The game has 5 progressive waves,
each harder than the last. The player wins by surviving all 5 waves and loses when
all lives are gone.

---

## How to Compile and Run

### Requirements
- Windows 10 or 11
- Visual Studio 2019 or 2022 (with C++ Desktop Development workload installed)
- SFML 2.6.1 (download instructions below)

### Step 1 — Download SFML
1. Go to: https://www.sfml-dev.org/download.php
2. Download **SFML 2.6.1 — Visual C++ 17 — 64-bit**
3. Extract the zip and place the folder at: `C:\SFML`

### Step 2 — Configure Visual Studio
1. Open the project in Visual Studio
2. Right click the project → **Properties**
3. Under `C/C++` → General → **Additional Include Directories**, add:
   ```
   C:\SFML\include
   ```
4. Under `Linker` → General → **Additional Library Directories**, add:
   ```
   C:\SFML\lib
   ```
5. Under `Linker` → Input → **Additional Dependencies**, add:
   ```
   sfml-graphics-d.lib
   sfml-window-d.lib
   sfml-system-d.lib
   ```

### Step 3 — Copy DLL Files
Copy these files from `C:\SFML\bin` into your project's `x64\Debug\` folder:
```
sfml-graphics-d-2.dll
sfml-window-d-2.dll
sfml-system-d-2.dll
```

### Step 4 — Build and Run
- Open the solution file `TowerDefenseGame.sln` in Visual Studio
- Press **F5** to build and run
- The game window will open automatically

> **Note:** The font is loaded directly from `C:\Windows\Fonts\arial.ttf`
> which is available on all Windows systems — no extra setup needed.

---

## How to Play

| Action | Control |
|---|---|
| Place a tower | Left click on any grass tile |
| Select Cannon Tower | Press **1** key |
| Select Sniper Tower | Press **2** key |
| Select Machine Gun Tower | Press **3** key |
| Select Slow Tower | Press **4** key |
| Quit the game | Press **Escape** |

### Tower Unlock System
Towers unlock progressively as waves increase:

| Wave | Towers Available |
|---|---|
| Wave 1 | Cannon Tower only |
| Wave 2 | + Sniper Tower unlocked |
| Wave 3 | + Machine Gun Tower unlocked |
| Wave 4 | + Slow Tower unlocked |
| Wave 5 | All towers available |

### Rules
- You can place a maximum of **2 towers per wave**
- Towers are **removed between waves** — place new ones each round
- You earn **bonus gold** between waves to afford better towers
- You start with **4 lives** — lose one each time an enemy escapes
- Survive all **5 waves** to win

---

## GUI Library Used

**SFML (Simple and Fast Multimedia Library) version 2.6.1**

SFML is a free, open-source C++ multimedia library that provides simple APIs
for window creation, 2D graphics rendering, and event handling.

### How to Install SFML
1. Visit: https://www.sfml-dev.org/download.php
2. Select: `SFML 2.6.1 — Visual C++ 17 — 64-bit`
3. Download and extract to `C:\SFML`
4. Follow the Visual Studio configuration steps above

No additional package managers or installers are required.
SFML is a header + library setup — just point Visual Studio to the folders.

---

## OOP Concepts Implemented

### Inheritance
Three-level class hierarchy:
```
Entity  (abstract base)
├── Enemy  (abstract)
│   ├── BasicEnemy
│   ├── FastEnemy
│   ├── TankEnemy
│   └── FlyingEnemy
└── Tower  (abstract)
    ├── CannonTower
    ├── SniperTower
    ├── MachineGunTower
    └── SlowTower
```

### Polymorphism
All enemies stored as `Enemy*` pointers and all towers as `Tower*` pointers.
Virtual functions `update()`, `render()`, `attack()`, and `reachedExit()` are
overridden in every derived class — correct version called automatically at runtime.

### Encapsulation
All data members are `private` or `protected`. Public getters and setters
control access. Game state managed exclusively through the Game class interface.

### Abstraction
`Entity`, `Enemy`, and `Tower` are abstract classes with pure virtual functions.
They cannot be instantiated directly — only their concrete subclasses can.

---

## Enemy Types

| Enemy | Color | HP | Speed | Gold Reward | Special |
|---|---|---|---|---|---|
| BasicEnemy | Red | 100 | 1.5 | 10 | Standard enemy |
| FastEnemy | Yellow | 50 | 3.0 | 15 | Moves very fast |
| TankEnemy | Dark Red | 400 | 0.6 | 40 | Very high HP |
| FlyingEnemy | Purple | 80 | 2.0 | 25 | Flies in straight line, ignores path |

## Tower Types

| Tower | Color | Damage | Fire Rate | Range | Special |
|---|---|---|---|---|---|
| CannonTower | Blue | 40 | 1/sec | 120px | High damage |
| SniperTower | Dark Green | 80 | 0.5/sec | 220px | Very long range |
| MachineGunTower | Orange | 10 | 5/sec | 130px | Rapid fire |
| SlowTower | Cyan | 0 | 2/sec | 110px | Slows all enemies 50% for 2 seconds |

---

## Known Issues and Limitations

- No sprite images are used — all game objects are drawn using colored shapes (circles and rectangles). This is intentional for simplicity.
- Towers can be placed on top of the enemy path — the player should avoid doing this as it does not block enemies but wastes gold.
- There is no pause functionality — once the game starts it runs continuously.
- The game window is fixed at 800x600 pixels and cannot be resized.
- Sound effects and background music are not implemented.
- There is no save system or high score tracking.
- The game must be run in Debug mode (not Release) as SFML debug DLLs are linked.

---

## Project Structure

```
TowerDefenseGame/
├── Entity.h              — Abstract base class for all game objects
├── Enemy.h               — Abstract enemy base class with path movement
├── Tower.h               — Abstract tower base class with targeting logic
├── BasicEnemy.h          — Standard enemy (red circle)
├── FastEnemy.h           — Fast low-HP enemy (yellow circle)
├── TankEnemy.h           — Slow high-HP enemy (dark red circle)
├── FlyingEnemy.h         — Flying enemy that ignores the path (purple circle)
├── CannonTower.h         — High damage slow fire tower (blue square)
├── SniperTower.h         — Long range high damage tower (green square)
├── MachineGunTower.h     — Rapid fire low damage tower (orange square)
├── SlowTower.h           — Area slow effect tower (cyan square)
├── WaveManager.h         — Controls enemy wave spawning and progression
├── Game.h                — Main game class declaration
├── Game.cpp              — Main game logic, loop, rendering, input
└── main.cpp              — Entry point
```

---
**Made by:

**Name:** Muhammad Noor Shaharyar
**Roll Number:** 25L-2608
**Course:** Object Oriented Programming
**Institution:** National University of Computing & Emerging Sciences
