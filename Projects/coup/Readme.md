<div align="center">

# 👹 COUP _ YOUR HONOR, THAT'S CAP

`SYSTEM_STATUS: ACTIVE` • `VERIFICATION: DISABLED` • `BUILD: v0.1-DEV`

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge\&logo=python\&logoColor=white)
![Build](https://img.shields.io/badge/Build-v0.1--DEV-FF0033?style=for-the-badge)
![Dependencies](https://img.shields.io/badge/Dependencies-Zero-00C853?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-58A6FF?style=for-the-badge)

**A command-line bluffing game built from basic Python logic — because apparently `if/else` was enough to start a heist.**

[Mission Brief](#01--mission_brief) • [The Crew](#02--the_crew) • [Game Engine](#03--game_engine) • [Roadmap](#07--roadmap)

---

</div>

## `01 // MISSION_BRIEF`

```console
[SYSTEM] Establishing connection...
[SYSTEM] 3 players detected.
[SYSTEM] 6 influence cards loaded.
[SYSTEM] Coin reserves initialized.
[SYSTEM] Truth verification: DISABLED.
[SYSTEM] Operation COUP is now LIVE.
```

> **OBJECTIVE:** Take control.

Every player has:

* hidden influence
* limited coins
* a set of possible actions
* permission to lie

The problem?

Someone at the table is eventually going to call your bluff.

```text
                         ┌─────────────────┐
                         │   TRUST ME BRO  │
                         └────────┬────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    ▼                           ▼
                 TELL TRUTH                   BLUFF
                    │                           │
                    ▼                           ▼
                 SURVIVE                  GET EXPOSED
```

The entire operation is controlled through **basic Python variables, lists, input, conditionals, and loops.**

---

## `02 // THE_CREW`

```text
╔══════════════════════════════════════════════════════════════╗
║ PLAYER       COINS       INFLUENCE       CARDS              ║
╠══════════════════════════════════════════════════════════════╣
║ ALEX         $02         ██              DUKE               ║
║                                           AMBASSADOR          ║
║                                                              ║
║ BLAIR        $03         ██              CAPTAIN             ║
║                                           CONTESSA            ║
║                                                              ║
║ CHARLIE      $03         ██              ASSASSIN            ║
║                                           DUKE               ║
╚══════════════════════════════════════════════════════════════╝
```

### `PLAYER_A // ALEX`

```text
COINS:      02
INFLUENCE:  ██
CARDS:      [ DUKE ] [ AMBASSADOR ]
```

### `PLAYER_B // BLAIR`

```text
COINS:      03
INFLUENCE:  ██
CARDS:      [ CAPTAIN ] [ CONTESSA ]
```

### `PLAYER_C // CHARLIE`

```text
COINS:      03
INFLUENCE:  ██
CARDS:      [ ASSASSIN ] [ DUKE ]
```

```console
[WARNING] Not every claim can be trusted.
```

---

## `03 // GAME_ENGINE`

The current version is deliberately procedural.

No game engine.

No external libraries.

No complicated architecture.

Just Python controlling the state of the operation.

```text
VARIABLES
   │
   ├── coins
   ├── influence
   └── turn
        │
        ▼
LISTS
   │
   └── player cards
        │
        ▼
INPUT
   │
   └── player decisions
        │
        ▼
IF / ELIF / ELSE
   │
   └── determine outcomes
        │
        ▼
WHILE LOOP
   │
   └── rotate turns
        │
        ▼
GAME STATE
```

The interesting part isn't the amount of code.

It's how simple pieces of Python combine to create **rules, decisions, and consequences.**

---

## `04 // MECHANICS_BREAKDOWN`

### `ACTION_PROTOCOL`

The player chooses an action from the terminal.

```python
act = int(input("Enter your choice of action: "))

if act == 1:
    # Income
    pass

elif act == 2:
    # Foreign Aid
    pass

elif act == 3:
    # Duke
    pass

elif act == 4:
    # Captain
    pass

elif act == 5:
    # Assassin
    pass

else:
    print("Invalid action")
```

```text
╔══════════════════════════════════════╗
║             ACTION MENU              ║
╠══════════════════════════════════════╣
║                                      ║
║  [1] INCOME          +1 COIN         ║
║  [2] FOREIGN AID     +2 COINS        ║
║  [3] DUKE            +3 COINS        ║
║  [4] CAPTAIN         STEAL 2         ║
║  [5] ASSASSIN        PAY 3           ║
║                                      ║
╚══════════════════════════════════════╝
```

---

### `DUKE_PROTOCOL // CLAIM`

A player can claim to hold the Duke.

The program then checks their cards.

```python
claim = "Duke"

if "Duke" in Alex_Cards:
    print("Alex actually has Duke!")
    blair_i -= 1

else:
    print("Alex was bluffing!")
    alex_i -= 1
```

The logic becomes:

```text
                 PLAYER CLAIMS DUKE
                         │
                         ▼
                 CHALLENGE?
                    /       \
                  YES        NO
                   │          │
                   ▼          ▼
               CHECK CARD   ALLOW
                /     \        │
              TRUE    FALSE    ▼
               │        │     +3 COINS
               ▼        ▼
         CHALLENGER   CLAIMANT
          -1 INFL.     -1 INFL.
```

A psychological bluff becomes a simple membership check:

```python
"Duke" in Alex_Cards
```

That's one of the things this project is teaching me:

> **Basic syntax can become surprisingly powerful when you combine it with logic.**

---

### `CAPTAIN_PROTOCOL`

Captain allows the current player to steal two coins from another player.

```python
steal_count = int(input("Choose your target: "))

if steal_count == i:
    print("You can't steal from yourself!")

elif steal_count == 1:
    alex_coins -= 2
    if alex_coins < 0:
        alex_coins = 0

elif steal_count == 2:
    blair_coins -= 2
    if blair_coins < 0:
        blair_coins = 0

elif steal_count == 3:
    charlie_coins -= 2
    if charlie_coins < 0:
        charlie_coins = 0

else:
    print("Invalid target")
```

```text
INPUT
  │
  ▼
TARGET
  │
  ├── SELF? ───────→ DENY
  │
  ├── ALEX? ───────→ STEAL
  │
  ├── BLAIR? ──────→ STEAL
  │
  ├── CHARLIE? ────→ STEAL
  │
  └── INVALID? ────→ ERROR
```

---

### `ASSASSIN_PROTOCOL`

The Assassin currently checks whether the player can afford the action.

```python
if alex_coins >= 3:
    alex_coins -= 3
    print("Alex paid 3 coins")

else:
    print("Alex doesn't have enough coins!")
```

```text
             ASSASSIN REQUEST
                    │
                    ▼
             COINS >= 3 ?
                /      \
              YES       NO
               │         │
               ▼         ▼
             PAY 3      DENY
```

The actual assassination and target logic is part of the roadmap.

---

### `TURN_PROTOCOL`

The current game uses a simple integer to track whose turn it is.

```python
i = 1

while i < 4:

    # Current player's action happens here

    i += 1

    if i == 4:
        i = 1
```

```text
       ┌─────────┐
       │  ALEX   │
       └────┬────┘
            ▼
       ┌─────────┐
       │  BLAIR  │
       └────┬────┘
            ▼
       ┌─────────┐
       │ CHARLIE │
       └────┬────┘
            │
            └──────────────┐
                           ▼
                         ALEX
```

The current implementation keeps rotating until the program is stopped manually.

---

## `05 // SYSTEM_CAPABILITIES`

```text
╔══════════════════════════════════════════════════════════╗
║                  SYSTEM CAPABILITIES                     ║
╠══════════════════════════════════════════════════════════╣
║                                                          ║
║  [✓] Character Selection                                ║
║  [✓] Three-Player System                                ║
║  [✓] Hidden Card Storage                                ║
║  [✓] Coin Tracking                                      ║
║  [✓] Influence Tracking                                 ║
║  [✓] Turn Rotation                                      ║
║  [✓] Income                                             ║
║  [✓] Foreign Aid                                        ║
║  [✓] Duke Claim                                         ║
║  [✓] Duke Challenge                                     ║
║  [✓] Captain Steal                                      ║
║  [✓] Assassin Payment                                   ║
║                                                          ║
║  [ ] Randomized Deck                                    ║
║  [ ] Card Reveal                                        ║
║  [ ] Card Replacement                                   ║
║  [ ] Blocking                                            ║
║  [ ] Influence Elimination                              ║
║  [ ] Coup                                                ║
║  [ ] Winner Detection                                   ║
║  [ ] Proper Game Ending                                 ║
║                                                          ║
╚══════════════════════════════════════════════════════════╝
```

---

## `06 // KNOWN_BUGS`

| Component   | Current Issue                                                         | Priority |
| ----------- | --------------------------------------------------------------------- | -------- |
| `GAME_LOOP` | Turns currently rotate indefinitely. No win-state break condition.    | `HIGH`   |
| `CARDS`     | Cards are hardcoded instead of randomly distributed.                  | `MEDIUM` |
| `ASSASSIN`  | Payment works, but target/elimination logic is not implemented.       | `HIGH`   |
| `CAPTAIN`   | Stealing works, but challenge/block mechanics are not implemented.    | `MEDIUM` |
| `INFLUENCE` | Influence decreases, but there is no complete elimination system yet. | `HIGH`   |
| `WIN_STATE` | There is currently no actual winner detection.                        | `HIGH`   |

```console
[!] These aren't hidden problems.
[!] They're the next problems to solve.
```

---

## `07 // ROADMAP`

```text
CURRENT BUILD
     │
     ▼
┌───────────────┐
│    v0.1 DEV   │
│  BASIC LOGIC  │
└───────┬───────┘
        │
        ▼
┌────────────────┐
│ RANDOMIZATION  │
│ & CARD SYSTEM  │
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ BLOCKS &       │
│ COUNTER-ACTIONS│
└───────┬────────┘
        │
        ▼
┌────────────────┐
│ ELIMINATION    │
│ & WIN DETECTION│
└───────┬────────┘
        │
        ▼
┌───────────────┐
│    v1.0       │
│ FULL GAME     │
└───────────────┘
```

### `TARGET_UPGRADES`

```text
[01] Random card distribution
[02] Proper card management
[03] Card reveal & replacement
[04] Blocking mechanics
[05] Counter-actions
[06] Influence elimination
[07] Coup action
[08] Forced Coup rule
[09] Winner detection
[10] Proper game termination
```

The goal isn't just to make the game bigger.

It's to make the **logic underneath it harder.**

---

## `08 // REPOSITORY_STRUCTURE`

```text
learning-py-from-scratch/
│
├── README.md
│
└── projects/
    │
    └── coup/
        │
        └── coup_game.py
```

### `PROJECT_STATUS`

```text
LANGUAGE        → Python
VERSION         → 3.10+
TYPE            → Command-Line Game
DEPENDENCIES    → None
ARCHITECTURE    → Procedural
STATUS          → Under Development
BUILD           → v0.1
```

---

## `09 // PHILOSOPHY`

```text
╔══════════════════════════════════════════════════════╗
║                                                      ║
║           LEARNING PYTHON FROM SCRATCH               ║
║                                                      ║
║       LEARN → BUILD → BREAK → DEBUG → UNDERSTAND     ║
║                         ↺                            ║
║                                                      ║
╚══════════════════════════════════════════════════════╝
```

COUP is part of a larger learning journey.

The idea isn't to start with advanced Python and build something huge immediately.

It's to start with the basics and keep pushing them.

```text
IF / ELSE
    ↓
LOOPS
    ↓
LISTS
    ↓
INPUT
    ↓
STATE
    ↓
DECISIONS
    ↓
GAME LOGIC
    ↓
HARDER PROBLEMS
```

### No blind vibe coding.

AI can explain a concept.

AI can help debug a broken path.

AI can point out a logical flaw.

But the goal is to understand **why the code works**, not just make the code work.

---

## `10 // FINAL_TRANSMISSION`

```console
[SYSTEM] Operation status: ACTIVE

[SYSTEM] Cards: HIDDEN
[SYSTEM] Money: LIMITED
[SYSTEM] Trust: LOW
[SYSTEM] Bluff detection: ENABLED
[SYSTEM] Logic engine: PYTHON

> Someone is lying.

> Someone is watching.

> Someone is about to challenge.

> Choose wisely.
```

```text
             🔴 CLAIM
                │
                ▼
             🔵 CHALLENGE
                │
                ▼
             🔴 VERIFY
                │
                ▼
             🔵 CONSEQUENCE
                │
                ▼
             🔴 SURVIVE
```

### `CLAIM.` `CHALLENGE.` `SURVIVE.`

```text
╔══════════════════════════════════════════════════════════════╗
║                                                              ║
║                    OPERATION COUP                            ║
║                                                              ║
║                    BUILD: v0.1-DEV                           ║
║                    STATUS: ACTIVE                            ║
║                                                              ║
║                     TRUST NO ONE.                            ║
║                                                              ║
╚══════════════════════════════════════════════════════════════╝
```

`> connection terminated.`
