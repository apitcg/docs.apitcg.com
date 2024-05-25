---
sidebar_position: 2
slug: /api-reference
---

# API Reference

## TCG Available
For now it is only available for:

- Pokémon
- One Piece

## How to use

To consume this API, you must use the following URL:

- One Piece, https://apitcg.com/api/one-piece/cards
- Pokémon, https://apitcg.com/api/pokemon/cards

This API receives two parameters, `property` and `value`, where `property` is the property you want to filter and `value`, the value you want to search.

## One Piece

### Example request 

```cli
https://apitcg.com/api/one-piece/cards?property=name&value=luffy
```

### Example response

```json
{
    "data": [
        {
            "id": "OP01-003",
            "code": "OP01-003",
            "rarity": "L",
            "type": "LEADER",
            "name": "Monkey.D.Luffy",
            "images": {
                "small": "https://en.onepiece-cardgame.com/images/cardlist/card/OP01-003.png",
                "large": "https://en.onepiece-cardgame.com/images/cardlist/card/OP01-003.png"
            },
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
        },
        ...
    ],
    "totalCount": 39
}
```

### Available properties

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


## Pokémon

The Pokémon data source provided is from pokemontcg.io

### Example request 

```cli
https://apitcg.com/api/pokemon/cards?property=name&value=charizard
```

### Example response

```json
{
    "data": [
        {
            "id": "base1-4",
            "name": "Charizard",
            "supertype": "Pokémon",
            "subtypes": [
                "Stage 2"
            ],
            "level": "76",
            "hp": "120",
            "types": [
                "Fire"
            ],
            "evolvesFrom": "Charmeleon",
            "abilities": [
                {
                    "name": "Energy Burn",
                    "text": "As often as you like during your turn (before your attack), you may turn all Energy attached to Charizard into Fire Energy for the rest of the turn. This power can't be used if Charizard is Asleep, Confused, or Paralyzed.",
                    "type": "Pokémon Power"
                }
            ],
            "attacks": [
                {
                    "name": "Fire Spin",
                    "cost": [
                        "Fire",
                        "Fire",
                        "Fire",
                        "Fire"
                    ],
                    "convertedEnergyCost": 4,
                    "damage": "100",
                    "text": "Discard 2 Energy cards attached to Charizard in order to use this attack."
                }
            ],
            "weaknesses": [
                {
                    "type": "Water",
                    "value": "×2"
                }
            ],
            "resistances": [
                {
                    "type": "Fighting",
                    "value": "-30"
                }
            ],
            "retreatCost": [
                "Colorless",
                "Colorless",
                "Colorless"
            ],
            "convertedRetreatCost": 3,
            "number": "4",
            "artist": "Mitsuhiro Arita",
            "rarity": "Rare Holo",
            "flavorText": "Spits fire that is hot enough to melt boulders. Known to unintentionally cause forest fires.",
            "nationalPokedexNumbers": [
                6
            ],
            "legalities": {
                "unlimited": "Legal"
            },
            "images": {
                "small": "https://images.pokemontcg.io/base1/4.png",
                "large": "https://images.pokemontcg.io/base1/4_hires.png"
            }
        },
        ...
    ],
    "totalCount": 99
}
```

### Available properties

These are the properties you can use to filter the available cards:

- id
- name