---
sidebar_position: 3
slug: /api-reference/pagination
---

# API Reference

## TCG Available

For now it is only available for:

- Pokémon
- One Piece 
- Dragon Ball Fusion 
- Digimon 
- Magic The Gathering
- Union arena
- Star Wars (Coming soon)
- Mitos y leyendas (Coming soon)

## How to use

To consume this API, you must use the following URL:

- One Piece, `https://apitcg.com/api/one-piece/cards`
- Pokémon, `https://apitcg.com/api/pokemon/cards`
- Dragon Ball Fusion, `https://apitcg.com/api/dragon-ball-fusion/cards`
- Digimon, `https://apitcg.com/api/digimon/cards`
- Magic, `https://apitcg.com/api/magic/cards`
- Union Arena, `https://apitcg.com/api/union-arena/cards`

This API receives two parameters, `property` and `value`, where `property` is the property you want to filter and `value`, the value you want to search.

## One Piece

### Example request 

```cli
https://apitcg.com/api/one-piece/cards?name=luffy
```

### Example response

```json
{
    "page": 1,
    "limit": 25,
    "total": 61,
    "totalPages": 3,
    "data": [
        {
            "id": "OP03-070",
            "code": "OP03-070",
            "rarity": "R",
            "type": "CHARACTER",
            "name": "Monkey.D.Luffy",
            "images": {
                "small": "https://en.onepiece-cardgame.com/images/cardlist/card/OP03-070.png",
                "large": "https://en.onepiece-cardgame.com/images/cardlist/card/OP03-070.png"
            },
            "cost": 6,
            "attribute": {
                "name": "Strike",
                "image": "https://en.onepiece-cardgame.com/images/cardlist/attribute/ico_type01.png"
            },
            "power": 7000,
            "counter": "-",
            "color": "Purple",
            "family": "Water Seven/Straw Hat Crew",
            "ability": "[On Play] DON!! −1 (You may return the specified number of DON!! cards from your field to your DON!! deck.) You may trash 1 Character card with a cost of 5 from your hand: This Character gains [Rush] during this turn.<br>(This card can attack on the turn in which it is played.)",
            "trigger": "",
            "set": {
                "name": "-PILLARS OF STRENGTH- [OP03]"
            },
            "notes": []
        },
        ...
    ]
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

## Pokémon

### Example request 

```cli
https://apitcg.com/api/pokemon/cards?name=charizard
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

## Dragon Ball Fusion

### Example request 

```cli
https://apitcg.com/api/dragon-ball-fusion/cards?name=goku
```

### Example response

```json
{
    "data": [
         {
            "id": "FB01-001",
            "code": "FB01-001",
            "rarity": "L",
            "name": "Son Goku",
            "color": "Red",
            "images": {
                "small": "https://www.dbs-cardgame.com/fw/images/cards/card/en/FB01-001_f.webp",
                "large": "https://www.dbs-cardgame.com/fw/images/cards/card/en/FB01-001_f.webp"
            },
            "cardType": "LEADER",
            "cost": "-",
            "specifiedCost": "-",
            "power": "15000",
            "comboPower": "-",
            "features": "Saiyan/Universe 7",
            "effect": "[When Attacking] Draw 1 card.<br>[Awaken] When your life is at 4 or less, draw 1 card. Then, flip this card over.",
            "getIt": "BOOSTER PACK -AWAKENED PULSE- [FB01]",
            "set": {
                "id": "fb01",
                "name": "Awakened pulse"
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
- code
- rarity
- name
- color
- cardType
- cost
- specifiedCost
- power
- comboPower
- features
- effect
- getIt

## Digimon

### Example request 

```cli
https://apitcg.com/api/digimon/cards?name=gallantmon
```

### Example response

```json
{
    "page": 1,
    "limit": 25,
    "total": 35,
    "totalPages": 2,
    "data": [
        {
            "id": "ST7-09",
            "code": "ST7-09",
            "name": "Gallantmon",
            "level": "Lv.6",
            "colors": [
                "Red"
            ],
            "images": {
                "small": "https://world.digimoncard.com/images/cardlist/card/ST7-09.png",
                "large": "https://world.digimoncard.com/images/cardlist/card/ST7-09.png"
            },
            "cardType": "Digimon",
            "form": "Mega",
            "attribute": "Virus",
            "type": "Holy Warrior/Royal Knight",
            "dp": "11000",
            "playCost": "11",
            "digivolveCost1": "3 from Lv.5",
            "digivolveCost2": "-",
            "effect": "＜Security Attack +1＞ (This Digimon checks 1\n                            additional security card.)[When Attacking] Delete 1\n                            of your opponent's Digimon with 4000 DP or less. If\n                            no Digimon was deleted by this effect, this Digimon\n                            gets +3000 DP for the turn.",
            "inheritedEffect": "-",
            "securityEffect": "-",
            "notes": "<a href=\"/products/deck/st-7.php\">▹Starter Deck, GALLANTMON [ST-7]</a>",
            "set": {
                "id": "st7",
                "name": "Starter Deck Gallantmon"
            }
        },
        ...
    ]
}
```

### Available properties

These are the properties you can use to filter the available cards:

- id
- code
- rarity
- name

## Union Arena

### Example request 

```cli
https://apitcg.com/api/union-arena/cards?name=gon
```

### Example response

```json
{
    "data": [
        {
            "id": "HTR-1-005",
            "code": "HTR-1-005",
            "url": "https://www.unionarena-tcg.com/na/cardlist/./detail_iframe.php?card_no=UE02BT/HTR-1-005",
            "name": "Gon Freecss",
            "rarity": "C",
            "ap": "1",
            "type": "Character",
            "bp": "1500",
            "affinity": "-",
            "effect": "\n                 Draw a card, then place one card from your hand into your sideline.              ",
            "trigger": "Draw a card.",
            "images": {
                "small": "https://www.unionarena-tcg.com/na/images/cardlist/card/UE02BT_HTR-1-005.png",
                "large": "https://www.unionarena-tcg.com/na/images/cardlist/card/UE02BT_HTR-1-005.png"
            },
            "set": {
                "name": "HUNTER X HUNTER"
            },
            "needEnergy": {
                "value": "Blue-",
                "logo": "https://www.unionarena-tcg.com/na/images/cardlist/icon/need/ico_character_energy_blue0.png"
            }
        },
        ...
    ],
    "totalCount": 99
}
```

### Available properties

These are the properties you can use to filter the available cards:

- ...