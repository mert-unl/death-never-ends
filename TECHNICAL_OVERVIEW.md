# Technical Overview

This document describes the main gameplay systems and architectural decisions behind **Death Never Ends**, a 2D roguelite RPG developed with Unity 6 and C#.

## Core Systems

### Combat System

Combat is managed by `BattleSystem`, which uses a state-based flow to handle player turns, enemy turns, victory, and defeat.

`DamageCalculator` handles hit chance, base damage, augment modifiers, critical hits, and enemy defense. Status effects and visual feedback are integrated into the combat flow.

Player actions include quick attacks, normal attacks, strong attacks, defense, and ultimate abilities.

### Augment System

Augments are a core part of character build customization. They are organized by elemental themes and five rarity tiers: Common, Uncommon, Rare, Epic, and Legendary.

Augments can modify damage, critical chance, ultimate charge, status effects, and survivability. Hook-based interactions allow augments to respond to different combat events.

### Dungeon Progression

`DungeonCreator` generates dungeon node sequences at runtime. These sequences can include combat, encounters, shops, mini-bosses, and a final boss.

`NodeFlowController` manages node progression and coordinates the transition between gameplay panels.

### Equipment and Inventory

The equipment system separates item definitions (`ItemData`) from runtime item instances (`ItemInstance`).

Players can collect equipment, equip items, compare their stats, and merge items of the same type and rarity to create higher-rarity equipment.

### Encounters

Non-combat encounters introduce risk-and-reward decisions through mini-games such as Blackjack, Rock Paper Scissors, Shadow Thief, and Jackpot.

Game rules are separated from their UI components to keep encounter logic more organized and easier to extend.

## Architecture

### Data Management

The project separates persistent player progression from run-specific state.

- `PlayerData`: Persistent progression, player stats, and inventory data.
- `RunData`: Stats, resources, augments, and inventory for the current run.
- `GameSessionData`: Temporary selections shared between scenes.
- `SaveManager`: JSON serialization and save-data management.

### Event-Driven UI

C# events, Unity Events, and callbacks connect gameplay systems with UI updates. This reduces direct dependencies between game logic and visual components.

### Visual Feedback

The project uses coroutines for attack and scene-transition sequences, alongside damage popups, screen shake, status-effect visuals, impact effects, and parallax backgrounds.

## Technology Stack

- **Engine:** Unity 6 (`6000.4.3f1`)
- **Language:** C#
- **Rendering:** Universal Render Pipeline (URP)
- **Input:** Unity Input System
- **UI:** Unity uGUI and TextMesh Pro
- **Persistence:** JSON-based save system

## Development Focus

The project is being developed independently, with an emphasis on modular gameplay systems, replayability, combat feedback, and build variety.

Future development may expand dungeon areas, enemy and boss mechanics, elemental synergies, encounters, loot, audio, and automated testing.
