# Icarus Consumables - Minified Data Format

This document describes the structure of `icarus_consumables.min.json`, a consolidated and optimized dataset containing all food, drink, and medicine information from Icarus.

## File Overview

The minified file is a single JSON object containing four primary keys:

- `metadata`: Runtime and version information.
- `items`: The master list of consumable items.
- `recipes`: Crafting recipes indexed by the target item.
- `modifiers`: Detailed effects for food/drink buffs.
- `stat_metadata`: (Optional) Mappings for internal stat codes to readable names.

## 1. Metadata

Provides context for the generation run.

| Field | Description |
| :--- | :--- |
| `game_version` | The internal version of Icarus used (e.g., `1.2.3.12345`). |
| `build_guid` | The Steam Build ID of the processed game data. |
| `parser_version` | The version of the extraction tool used. |
| `generated_date` | Date of generation in `YYYY-MM-DD` format. |

## 2. Items (`items`)

An array of objects representing items. Descriptions and source IDs are removed for size efficiency.

| Field | Type | Description |
| :--- | :--- | :--- |
| `name` | string | Normalized internal ID (e.g., `meatstew`). |
| `display_name` | string | Localized name in English. |
| `category` | string | `Food`, `Drink`, `Animal Food`, or `Medicine`. |
| `base_stats` | object | Mapping of stat names to values (e.g., `{"Food": 50}`). |
| `tier_info` | object | Contains `total_tier` (numeric) and `best_bench`. |
| `modifiers` | array | List of buff IDs associated with this item. |
| `source_item` | string | For piece-based items (cakes), the parent item ID. |
| `is_decay_product`| boolean| True if this item is a byproduct of spoilage. |

## 3. Recipes (`recipes`)

A dictionary where keys are normalized item names and values are arrays of possible recipes.

| Field | Description |
| :--- | :--- |
| `inputs` | List of `{item: "id", count: N, is_generic: bool}`. |
| `outputs` | List of `{item: "id", count: N}`. |
| `benches` | List of localized bench names where this can be crafted. |
| `requirement` | Talent name required to unlock the recipe. |

## 4. Modifiers (`modifiers`)

Details the specific buffs granted by consuming items.

| Field | Description |
| :--- | :--- |
| `id` | The internal buff ID. |
| `lifetime` | Duration in seconds. |
| `stats` | Mapping of applied stat modifiers (e.g., `{"MaxHealth": 100}`). |
| `state_color` | Color coding for the buff (Food, Drink, Tonic). |

## 5. Stat Metadata (`stat_metadata`)

Maps internal game stat keys (e.g., `Stats_MaxHealth`) to user-friendly titles ("Max Health"). This is useful for UI display where raw keys are too technical.

---

### Tips for Consumption
- Use the `name` field in `items` to lookup entries in `recipes`.
- Modifiers in `items` point to the keys in the global `modifiers` object.
- All IDs are lowercased and stripped of non-alphanumeric characters for consistent cross-referencing.
