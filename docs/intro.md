---
sidebar_position: 1
slug: /
---

# Introduction

Welcome to use API TCG, the purpose of this API is to expose the card data of each TCG.

## TCG Available
For now it is only available for:

- Pokémon
- One Piece

## How to use

To consume this API, you must use the following URL:

- One Piece, https://apitcg.com/api/one-piece/cards
- Pokémon, https://apitcg.com/api/pokemon/cards

This API receives two parameters, `property` and `value`, where `property` is the property you want to filter and `value`, the value you want to search.

### Example

```cli
https://apitcg.com/api/one-piece/cards?property=name&value=luffy
```

#### Response

```json
{
    "data": [
        {
            "id": "OP01-003",
            "code": "OP01-003",
            "rarity": "L",
            "type": "LEADER",
            "name": "Monkey.D.Luffy",
            "image": "https://en.onepiece-cardgame.com/images/cardlist/card/OP01-003.png",
            "cost": 4,
            "attribute": {
                "name": "Strike",
                "image": "https://en.onepiece-cardgame.com/images/cardlist/attribute/ico_type01.png"
            },
            "power": 5000,
            "counter": "-",
            "color": "Red/Green",
            "family": "Supernovas/Straw Hat Crew",
            "ability": "[Activate: Main] [Once Per Turn] ➃ (You may rest the specified number of DON!! cards in your cost area.): Set up to 1 of your {Supernovas} or {Straw Hat Crew} type Character cards with a cost of 5 or less as active. It gains +1000 power during this turn.",
            "trigger": "",
            "set": {
                "name": "-ROMANCE DAWN- [OP01]"
            },
            "notes": [
                {
                    "name": "Errata Card",
                    "url": "https://en.onepiece-cardgame.com//rules/errata_card/#errata_05"
                }
            ]
        }
        ...
    ],
    "totalCount": 39
}
```

## Available properties

These are the properties you can use to filter the available cards:

- id
- code
- rarity
- type
- name
- cost
- power
- counter
- color
- family
- ability
- trigger
- cardSet

## Questions?

If you have any questions, you can write to: iturra.sebastian@hotmail.com
