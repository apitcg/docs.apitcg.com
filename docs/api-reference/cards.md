---
sidebar_position: 1
slug: /api-reference/cards
---

# Cards

## TCG Available

For now it is only available for:

- Pokémon
- One Piece 
- Dragon Ball Fusion 
- Digimon 
- Magic The Gathering
- Union arena
- Gundam
- Star Wars
- Riftbound (League Of Legends)
- Mitos y leyendas (Coming soon)

## How to use

To consume this API, you must use the following URL:

- One Piece, `https://apitcg.com/api/one-piece/cards`
- Pokémon, `https://apitcg.com/api/pokemon/cards`
- Dragon Ball Fusion, `https://apitcg.com/api/dragon-ball-fusion/cards`
- Digimon, `https://apitcg.com/api/digimon/cards`
- Magic, `https://apitcg.com/api/magic/cards`
- Union Arena, `https://apitcg.com/api/union-arena/cards`
- Gundam, `https://apitcg.com/api/gundam/cards`
- Riftbound, `https://apitcg.com/api/riftbound/cards`
- Star Wards Unlimited, `https://apitcg.com/api/star-wars-unlimited/cards`

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

## Gundam

### Example request 

```cli
https://apitcg.com/api/gundam/cards?name=strike
```

### Example response

```json
{
    "page": 1,
    "limit": 25,
    "total": 7,
    "totalPages": 1,
    "data": [
        {
            "id": "ST04-001",
            "code": "ST04-001",
            "rarity": "LR",
            "name": "Aile Strike Gundam",
            "images": {
                "small": "https://www.gundam-gcg.com/en/images/cards/card/ST04-001.webp",
                "large": "https://www.gundam-gcg.com/en/images/cards/card/ST04-001.webp"
            },
            "level": "5",
            "cost": "4",
            "color": "White",
            "cardType": "UNIT",
            "effect": "&lt;Blocker&gt; (Rest this Unit to change the attack target to it.)<br>【When Paired･Lv.4 or Higher Pilot】Choose 1 enemy Unit with 4 or less HP. Return it to its owner's hand.<br>",
            "zone": "Space / Earth",
            "trait": "(Earth Alliance)",
            "link": "[Kira Yamato]",
            "ap": "4",
            "hp": "4",
            "sourceTitle": "Mobile Suit Gundam SEED",
            "getIt": "Edition Beta",
            "set": {
                "id": "beta",
                "name": "Beta"
            }
        }
    ]
}
```

### Available properties

These are the properties you can use to filter the available cards:

- id
- code 
- name
- rarity
- color
- cardType
- effect
- zone
- trait
- link
- ap
- hp
- sourceTitle
- getIt


## Star Wars

### Example request 

```cli
https://apitcg.com/api/star-wars-unlimited/cards?name=darth
```

### Example response

```json
{
    "data": [
        {
            "id": "7661383869",
            "attributes": {
                "cardNumber": 6,
                "title": "Darth Vader",
                "subtitle": "Victor Squadron Leader",
                "cardCount": 257,
                "artist": "Borja Pindado",
                "artFrontHorizontal": true,
                "artBackHorizontal": null,
                "hasFoil": true,
                "cost": 6,
                "hp": 6,
                "power": 5,
                "text": "Action [Exhaust]: If you attacked with a non-token Vehicle unit this phase, create a TIE Fighter token.",
                "textStyled": "<p><strong>Action [</strong><img src=\"https://ffg-prod-uploads.s3.amazonaws.com/icon_exhaust_0c450d09d4.png\" alt=\"icon_exhaust.png\"><strong>]:</strong> If you attacked with a non-token <i><strong>Vehicle</strong></i> unit this phase, create a TIE Fighter token.</p>",
                "deployBox": "",
                "deployBoxStyled": "",
                "epicAction": "Epic Action: If you control 6 or more resources, choose one:\nDeploy this leader.\nDeploy this leader as an upgrade on a friendly Vehicle unit without a Pilot on it.",
                "epicActionStyled": "<p><strong>Epic Action:</strong> If you control 6 or more resources, choose one:</p><ul><li>Deploy this leader.</li><li>Deploy this leader as an upgrade on a friendly <i><strong>Vehicle</strong></i> unit without a <i><strong>Pilot</strong></i> on it.</li></ul>",
                "linkHtml": "<a href=\"/cards?cid=7661383869\" class=\"card-tooltip\" data-card=\"7661383869\">Darth Vader</a>",
                "cardId": null,
                "createdAt": "2024-11-27T04:40:29.109Z",
                "updatedAt": "2025-02-06T20:55:30.911Z",
                "publishedAt": "2025-02-06T20:53:13.262Z",
                "locale": "en",
                "hyperspace": false,
                "unique": true,
                "showcase": false,
                "cardUid": "7661383869",
                "rules": null,
                "rulesStyled": "",
                "serialCode": "04010006",
                "upgradeHp": 5,
                "upgradePower": 5,
                "artFront": {
                    "data": {
                        "id": 23991,
                        "attributes": {
                            "name": "04010006EN_Darth_Vader_Leader.png",
                            "alternativeText": null,
                            "caption": null,
                            "width": 419,
                            "height": 300,
                            "formats": {
                                "card": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//card_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                                    "hash": "card_04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                                    "mime": "image/png",
                                    "name": "card_04010006EN_Darth_Vader_Leader.png",
                                    "path": null,
                                    "size": 199.04,
                                    "width": 400,
                                    "height": 286
                                },
                                "xsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                                    "hash": "xsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                                    "mime": "image/png",
                                    "name": "xsmall_04010006EN_Darth_Vader_Leader.png",
                                    "path": null,
                                    "size": 121.67,
                                    "width": 300,
                                    "height": 215
                                },
                                "xxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                                    "hash": "xxsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                                    "mime": "image/png",
                                    "name": "xxsmall_04010006EN_Darth_Vader_Leader.png",
                                    "path": null,
                                    "size": 60.06,
                                    "width": 200,
                                    "height": 143
                                },
                                "xxxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxxsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                                    "hash": "xxxsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                                    "mime": "image/png",
                                    "name": "xxxsmall_04010006EN_Darth_Vader_Leader.png",
                                    "path": null,
                                    "size": 36.35,
                                    "width": 150,
                                    "height": 107
                                },
                                "thumbnail": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//thumbnail_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                                    "hash": "thumbnail_04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                                    "mime": "image/png",
                                    "name": "thumbnail_04010006EN_Darth_Vader_Leader.png",
                                    "path": null,
                                    "size": 88.84,
                                    "width": 250,
                                    "height": 179
                                }
                            },
                            "hash": "04010006_EN_Darth_Vader_Leader_dacc2c03a2",
                            "ext": ".png",
                            "mime": "image/png",
                            "size": 54.64,
                            "url": "https://cdn.starwarsunlimited.com//04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                            "previewUrl": null,
                            "provider": "aws-s3",
                            "provider_metadata": null,
                            "createdAt": "2024-11-26T20:03:25.716Z",
                            "updatedAt": "2024-11-26T20:03:25.716Z"
                        }
                    }
                },
                "artBack": {
                    "data": {
                        "id": 26634,
                        "attributes": {
                            "name": "04010006EN_Darth_Vader_Leader_Unit.png",
                            "alternativeText": null,
                            "caption": null,
                            "width": 300,
                            "height": 419,
                            "formats": {
                                "card": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//card_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                                    "hash": "card_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                                    "mime": "image/png",
                                    "name": "card_04010006EN_Darth_Vader_Leader_Unit.png",
                                    "path": null,
                                    "size": 195.38,
                                    "width": 286,
                                    "height": 400
                                },
                                "xsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                                    "hash": "xsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                                    "mime": "image/png",
                                    "name": "xsmall_04010006EN_Darth_Vader_Leader_Unit.png",
                                    "path": null,
                                    "size": 120.04,
                                    "width": 215,
                                    "height": 300
                                },
                                "xxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                                    "hash": "xxsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                                    "mime": "image/png",
                                    "name": "xxsmall_04010006EN_Darth_Vader_Leader_Unit.png",
                                    "path": null,
                                    "size": 59.87,
                                    "width": 143,
                                    "height": 200
                                },
                                "xxxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxxsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                                    "hash": "xxxsmall_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                                    "mime": "image/png",
                                    "name": "xxxsmall_04010006EN_Darth_Vader_Leader_Unit.png",
                                    "path": null,
                                    "size": 36.28,
                                    "width": 107,
                                    "height": 150
                                },
                                "thumbnail": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//thumbnail_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                                    "hash": "thumbnail_04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                                    "mime": "image/png",
                                    "name": "thumbnail_04010006EN_Darth_Vader_Leader_Unit.png",
                                    "path": null,
                                    "size": 88.11,
                                    "width": 179,
                                    "height": 250
                                }
                            },
                            "hash": "04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e",
                            "ext": ".png",
                            "mime": "image/png",
                            "size": 52.31,
                            "url": "https://cdn.starwarsunlimited.com//04010006_EN_Darth_Vader_Leader_Unit_fb0da8985e.png",
                            "previewUrl": null,
                            "provider": "aws-s3",
                            "provider_metadata": null,
                            "createdAt": "2024-11-26T20:52:14.666Z",
                            "updatedAt": "2024-11-26T20:52:14.666Z"
                        }
                    }
                },
                "artThumbnail": {
                    "data": {
                        "id": 21209,
                        "attributes": {
                            "name": "04010006EN_Darth_Vader_Leader_Unit_Thumbnail.png",
                            "alternativeText": null,
                            "caption": null,
                            "width": 300,
                            "height": 100,
                            "formats": {
                                "xxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxsmall_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668.png",
                                    "hash": "xxsmall_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668",
                                    "mime": "image/png",
                                    "name": "xxsmall_04010006EN_Darth_Vader_Leader_Unit_Thumbnail.png",
                                    "path": null,
                                    "size": 32.92,
                                    "width": 200,
                                    "height": 67
                                },
                                "xxxsmall": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//xxxsmall_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668.png",
                                    "hash": "xxxsmall_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668",
                                    "mime": "image/png",
                                    "name": "xxxsmall_04010006EN_Darth_Vader_Leader_Unit_Thumbnail.png",
                                    "path": null,
                                    "size": 19.37,
                                    "width": 150,
                                    "height": 50
                                },
                                "thumbnail": {
                                    "ext": ".png",
                                    "url": "https://cdn.starwarsunlimited.com//thumbnail_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668.png",
                                    "hash": "thumbnail_04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668",
                                    "mime": "image/png",
                                    "name": "thumbnail_04010006EN_Darth_Vader_Leader_Unit_Thumbnail.png",
                                    "path": null,
                                    "size": 49.05,
                                    "width": 250,
                                    "height": 83
                                }
                            },
                            "hash": "04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668",
                            "ext": ".png",
                            "mime": "image/png",
                            "size": 21.53,
                            "url": "https://cdn.starwarsunlimited.com//04010006_EN_Darth_Vader_Leader_Unit_Thumbnail_08201df668.png",
                            "previewUrl": null,
                            "provider": "aws-s3",
                            "provider_metadata": null,
                            "createdAt": "2024-11-26T19:15:57.422Z",
                            "updatedAt": "2024-11-26T19:15:57.422Z"
                        }
                    }
                },
                "localizations": {
                    "data": [
                        {
                            "id": 19505,
                            "attributes": {
                                "title": "Darth Vader",
                                "subtitle": "Capo Della Squadriglia Victor",
                                "cardNumber": 6,
                                "cardCount": 257,
                                "artist": "Borja Pindado",
                                "artFrontHorizontal": true,
                                "artBackHorizontal": null,
                                "hasFoil": true,
                                "cost": 6,
                                "hp": 6,
                                "power": 5,
                                "text": "Azione [Esaurire]: Se hai \nattaccato con unità VEICOLO non segnalino in questa fase, \ncrea 1 segnalino Caccia TIE.",
                                "textStyled": "<p><strong>Azione [</strong><img src=\"https://ffg-prod-uploads.s3.amazonaws.com/icon_exhaust_0c450d09d4.png\" alt=\"icon_exhaust.png\"><strong>]:</strong> Se hai attaccato con unità <i><strong>VEICOLO</strong></i> non segnalino in questa fase, crea 1 segnalino Caccia TIE.</p>",
                                "deployBox": "",
                                "deployBoxStyled": "",
                                "epicAction": "Azione Epica: Se controlli \n6 o più risorse, scegli 1 effetto:\nSchiera questo leader.\nSchiera questo leader come miglioria su un'unità Veicolo amica priva di carte Pilota.",
                                "epicActionStyled": "<p><strong>Azione Epica:</strong> Se controlli 6 o più risorse, scegli 1 effetto:</p><ul><li>Schiera questo leader.</li><li>Schiera questo leader come miglioria su un'unità <i><strong>Veicolo</strong></i> amica priva di carte <i><strong>Pilota</strong></i>.</li></ul>",
                                "linkHtml": "<a href=\"/cards?cid=7661383869\" class=\"card-tooltip\" data-card=\"7661383869\">Darth Vader</a>",
                                "cardId": "19501",
                                "createdAt": "2024-11-27T04:40:30.538Z",
                                "updatedAt": "2025-02-06T20:55:30.367Z",
                                "publishedAt": "2025-02-06T20:55:28.865Z",
                                "locale": "it",
                                "hyperspace": false,
                                "unique": true,
                                "showcase": false,
                                "cardUid": "7661383869",
                                "rules": null,
                                "rulesStyled": "",
                                "serialCode": "04010006",
                                "upgradeHp": 5,
                                "upgradePower": 5
                            }
                        },
                        {
                            "id": 19504,
                            "attributes": {
                                "title": "Dark Vador",
                                "subtitle": "Leader De L'Escadron Victor",
                                "cardNumber": 6,
                                "cardCount": 257,
                                "artist": "Borja Pindado",
                                "artFrontHorizontal": true,
                                "artBackHorizontal": null,
                                "hasFoil": true,
                                "cost": 6,
                                "hp": 6,
                                "power": 5,
                                "text": "Action [Inclinez] : si vous avez attaqué avec une unité VÉHICULE non-jeton à cette phase, créez un jeton Chasseur TIE.",
                                "textStyled": "<p><strong>Action [</strong><img src=\"https://ffg-prod-uploads.s3.amazonaws.com/icon_exhaust_0c450d09d4.png\" alt=\"icon_exhaust.png\"><strong>] :</strong> si vous avez attaqué avec une unité <i><strong>VÉHICULE</strong></i> non-jeton à cette phase, créez un jeton Chasseur TIE.</p>",
                                "deployBox": "",
                                "deployBoxStyled": "",
                                "epicAction": "Action Épique : si vous contrôlez au moins 6 ressources, choisissez l'un :\nDéployez ce leader.\nDéployez ce leader en tant qu'amélioration sur une unité VÉHICULE amie sans PILOTE dessus.",
                                "epicActionStyled": "<p><strong>Action Épique :</strong> si vous contrôlez au moins 6 ressources, choisissez l'un :</p><ul><li>Déployez ce leader.</li><li>Déployez ce leader en tant qu'amélioration sur une unité <i><strong>VÉHICULE</strong></i> amie sans <i><strong>PILOTE</strong></i> dessus.</li></ul>",
                                "linkHtml": "<a href=\"/cards?cid=7661383869\" class=\"card-tooltip\" data-card=\"7661383869\">Dark Vador</a>",
                                "cardId": "19501",
                                "createdAt": "2024-11-27T04:40:30.130Z",
                                "updatedAt": "2025-02-06T20:55:30.926Z",
                                "publishedAt": "2025-02-06T20:53:24.586Z",
                                "locale": "fr",
                                "hyperspace": false,
                                "unique": true,
                                "showcase": false,
                                "cardUid": "7661383869",
                                "rules": null,
                                "rulesStyled": "",
                                "serialCode": "04010006",
                                "upgradeHp": 5,
                                "upgradePower": 5
                            }
                        },
                        {
                            "id": 19503,
                            "attributes": {
                                "title": "Darth Vader",
                                "subtitle": "Jefe del Escuadrón Victor",
                                "cardNumber": 6,
                                "cardCount": 257,
                                "artist": "Borja Pindado",
                                "artFrontHorizontal": true,
                                "artBackHorizontal": null,
                                "hasFoil": true,
                                "cost": 6,
                                "hp": 6,
                                "power": 5,
                                "text": "Acción [Agota]: Si has atacado en esta fase con una unidad Vehículo que no era una ficha, crea 1 ficha de Caza TIE.",
                                "textStyled": "<p><strong>Acción [</strong><img src=\"https://ffg-prod-uploads.s3.amazonaws.com/icon_exhaust_0c450d09d4.png\" alt=\"icon_exhaust.png\"><strong>]:</strong> Si has atacado en esta fase con una unidad <i><strong>Vehículo</strong></i> que no era una ficha, crea 1 ficha de Caza TIE.</p>",
                                "deployBox": "",
                                "deployBoxStyled": "",
                                "epicAction": "Acción épica: Si controlas al menos 6 recursos, elige una opción:\nDespliega este líder.\nDespliega este líder como mejora sobre una unidad Vehículo aliada \nque no tenga un Piloto.",
                                "epicActionStyled": "<p><strong>Acción épica:</strong> Si controlas al menos 6 recursos, elige una opción:</p><ul><li>Despliega este líder.</li><li>Despliega este líder como mejora sobre una unidad <i><strong>Vehículo</strong></i> aliada que no tenga un <i><strong>Piloto</strong></i>.</li></ul>",
                                "linkHtml": "<a href=\"/cards?cid=7661383869\" class=\"card-tooltip\" data-card=\"7661383869\">Darth Vader</a>",
                                "cardId": "19501",
                                "createdAt": "2024-11-27T04:40:29.771Z",
                                "updatedAt": "2025-02-06T20:55:30.949Z",
                                "publishedAt": "2025-02-06T20:55:21.706Z",
                                "locale": "es",
                                "hyperspace": false,
                                "unique": true,
                                "showcase": false,
                                "cardUid": "7661383869",
                                "rules": null,
                                "rulesStyled": null,
                                "serialCode": "04010006",
                                "upgradeHp": 5,
                                "upgradePower": 5
                            }
                        },
                        {
                            "id": 19502,
                            "attributes": {
                                "title": "Darth Vader",
                                "subtitle": "Staffelführer der Victor-Staffel",
                                "cardNumber": 6,
                                "cardCount": 257,
                                "artist": "Borja Pindado",
                                "artFrontHorizontal": true,
                                "artBackHorizontal": null,
                                "hasFoil": true,
                                "cost": 6,
                                "hp": 6,
                                "power": 5,
                                "text": "Aktion [Erschöpfe]: Falls du in dieser Phase mit einer\nNicht-Marker-Fahrzeug-\nEinheit angegriffen hast, erschaffe 1 TIE-Jäger-Marker.",
                                "textStyled": "<p><strong>Aktion [</strong><img src=\"https://ffg-prod-uploads.s3.amazonaws.com/icon_exhaust_0c450d09d4.png\" alt=\"icon_exhaust.png\"><strong>]:</strong> Falls du in dieser Phase mit einer Nicht-Marker-<i><strong>Fahrzeug</strong></i>- Einheit angegriffen hast, erschaffe 1 TIE-Jäger-Marker.</p>",
                                "deployBox": "",
                                "deployBoxStyled": "",
                                "epicAction": "Epische Aktion: Falls du 6 oder mehr Ressourcen kontrollierst, wähle 1:\nRufe diesen Anführer.\nRufe diesen Anführer als Upgrade an eine befreundete Fahrzeug-Einheit ohne angehängten Piloten.",
                                "epicActionStyled": "<p><strong>Epische Aktion:</strong> Falls du 6 oder mehr Ressourcen kontrollierst, wähle 1:</p><ul><li>Rufe diesen Anführer.</li><li>Rufe diesen Anführer als Upgrade an eine befreundete <i><strong>Fahrzeug</strong></i>-Einheit ohne angehängten <i><strong>Piloten</strong></i>.</li></ul>",
                                "linkHtml": "<a href=\"/cards?cid=7661383869\" class=\"card-tooltip\" data-card=\"7661383869\">Darth Vader</a>",
                                "cardId": "19501",
                                "createdAt": "2024-11-27T04:40:29.444Z",
                                "updatedAt": "2025-02-06T20:55:30.963Z",
                                "publishedAt": "2025-02-06T20:53:18.191Z",
                                "locale": "de",
                                "hyperspace": false,
                                "unique": true,
                                "showcase": false,
                                "cardUid": "7661383869",
                                "rules": null,
                                "rulesStyled": "",
                                "serialCode": "04010006",
                                "upgradeHp": 5,
                                "upgradePower": 5
                            }
                        }
                    ]
                },
                "aspects": {
                    "data": [
                        {
                            "id": 7,
                            "attributes": {
                                "name": "Command",
                                "description": "<p>Accelerate your resources and call upon lots of units or strengthen the units you already have in play. This is the aspect of choice for players who want to overwhelm their opponents with a massive board presence. &nbsp;</p>",
                                "color": "#0b992d",
                                "createdAt": "2023-08-10T21:06:20.704Z",
                                "updatedAt": "2024-07-15T21:32:40.603Z",
                                "publishedAt": "2023-11-15T20:01:18.812Z",
                                "locale": "en",
                                "englishName": "Command",
                                "sortValue": 5
                            }
                        },
                        {
                            "id": 27,
                            "attributes": {
                                "name": "Villainy",
                                "description": "<p>Battle for ultimate power with cards that feature the antagonists of the <i>Star Wars</i> series. This aspect is perfect for players who want to command huge armies or wield powerful destructive effects. Similar to Heroism, this aspect is often paired with another aspect on a card.&nbsp;</p>",
                                "color": "#000000",
                                "createdAt": "2023-08-10T21:18:54.590Z",
                                "updatedAt": "2024-07-15T21:40:49.467Z",
                                "publishedAt": "2023-11-15T20:02:14.861Z",
                                "locale": "en",
                                "englishName": "Villainy",
                                "sortValue": 20
                            }
                        }
                    ]
                },
                "aspectDuplicates": {
                    "data": []
                },
                "type": {
                    "data": {
                        "id": 4,
                        "attributes": {
                            "name": "Leader",
                            "description": "",
                            "value": "Leader",
                            "createdAt": "2023-08-14T21:36:36.932Z",
                            "updatedAt": "2024-03-05T17:11:46.417Z",
                            "publishedAt": "2023-11-15T17:30:41.305Z",
                            "locale": "en",
                            "sortValue": 1
                        }
                    }
                },
                "type2": {
                    "data": {
                        "id": 6,
                        "attributes": {
                            "name": "Leader Unit",
                            "description": "",
                            "value": "Leader Unit",
                            "createdAt": "2023-08-14T21:41:58.575Z",
                            "updatedAt": "2024-03-05T17:12:16.062Z",
                            "publishedAt": "2023-11-15T17:30:47.748Z",
                            "locale": "en",
                            "sortValue": 1
                        }
                    }
                },
                "traits": {
                    "data": [
                        {
                            "id": 32,
                            "attributes": {
                                "name": "Force",
                                "description": "",
                                "createdAt": "2023-08-15T14:41:02.071Z",
                                "updatedAt": "2023-11-15T23:57:27.173Z",
                                "publishedAt": "2023-11-15T23:57:27.169Z",
                                "locale": "en"
                            }
                        },
                        {
                            "id": 7,
                            "attributes": {
                                "name": "Imperial",
                                "description": "",
                                "createdAt": "2023-08-15T14:38:15.844Z",
                                "updatedAt": "2023-11-15T23:58:36.411Z",
                                "publishedAt": "2023-11-15T23:58:36.407Z",
                                "locale": "en"
                            }
                        },
                        {
                            "id": 42,
                            "attributes": {
                                "name": "Sith",
                                "description": "",
                                "createdAt": "2023-08-15T14:42:10.071Z",
                                "updatedAt": "2023-11-16T00:00:40.840Z",
                                "publishedAt": "2023-11-16T00:00:40.835Z",
                                "locale": "en"
                            }
                        },
                        {
                            "id": 278,
                            "attributes": {
                                "name": "Pilot",
                                "description": "",
                                "createdAt": "2024-11-19T20:43:41.945Z",
                                "updatedAt": "2024-12-04T16:09:15.805Z",
                                "publishedAt": "2024-12-04T16:09:15.800Z",
                                "locale": "en"
                            }
                        }
                    ]
                },
                "arenas": {
                    "data": [
                        {
                            "id": 2,
                            "attributes": {
                                "name": "Ground",
                                "description": null,
                                "createdAt": "2023-08-15T14:32:07.804Z",
                                "updatedAt": "2023-08-18T21:37:34.527Z",
                                "publishedAt": "2023-08-18T21:37:34.522Z",
                                "locale": "en"
                            }
                        }
                    ]
                },
                "keywords": {
                    "data": []
                },
                "rarity": {
                    "data": {
                        "id": 2,
                        "attributes": {
                            "name": "Common",
                            "character": "C",
                            "color": "#964d02",
                            "createdAt": "2023-08-08T20:26:30.378Z",
                            "updatedAt": "2024-04-25T14:06:46.441Z",
                            "publishedAt": "2023-11-15T20:15:14.512Z",
                            "locale": "en",
                            "englishName": "Common",
                            "sortValue": 2
                        }
                    }
                },
                "expansion": {
                    "data": {
                        "id": 23,
                        "attributes": {
                            "name": "Jump to Lightspeed",
                            "description": "",
                            "code": "JTL",
                            "createdAt": "2024-11-19T20:22:30.978Z",
                            "updatedAt": "2024-12-04T15:54:05.681Z",
                            "publishedAt": "2024-12-04T15:53:40.404Z",
                            "locale": "en",
                            "sortValue": 4
                        }
                    }
                },
                "variantTypes": {
                    "data": [
                        {
                            "id": 46,
                            "attributes": {
                                "name": "Standard",
                                "description": null,
                                "createdAt": "2024-07-15T14:37:37.643Z",
                                "updatedAt": "2024-07-15T19:55:03.314Z",
                                "publishedAt": "2024-07-15T19:54:44.645Z",
                                "locale": "en",
                                "variantId": "01",
                                "foil": false,
                                "sortValue": 10,
                                "icon": {
                                    "data": {
                                        "id": 8701,
                                        "attributes": {
                                            "name": "icon_standard.png",
                                            "alternativeText": null,
                                            "caption": null,
                                            "width": 144,
                                            "height": 144,
                                            "formats": null,
                                            "hash": "icon_standard_7dbd7cd8e6",
                                            "ext": ".png",
                                            "mime": "image/png",
                                            "size": 0.39,
                                            "url": "https://cdn.starwarsunlimited.com//icon_standard_7dbd7cd8e6.png",
                                            "previewUrl": null,
                                            "provider": "aws-s3",
                                            "provider_metadata": null,
                                            "createdAt": "2024-07-15T14:40:17.066Z",
                                            "updatedAt": "2024-07-15T14:40:17.066Z"
                                        }
                                    }
                                }
                            }
                        }
                    ]
                },
                "variantOf": {
                    "data": null
                },
                "reprintOf": {
                    "data": null
                },
                "validationId": "7661383869"
            },
            "name": "Darth Vader",
            "code": "6/257",
            "images": {
                "small": "https://cdn.starwarsunlimited.com//card_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png",
                "large": "https://cdn.starwarsunlimited.com//xsmall_04010006_EN_Darth_Vader_Leader_dacc2c03a2.png"
            },
            "set": {
                "id": 23,
                "name": "Jump to Lightspeed"
            }
        }
    ],
    "totalCount": 10
}
```

### Available properties

These are the properties you can use to filter the available cards:

- id
- name


## Riftbound

### Example request 

```cli
https://apitcg.com/api/riftbound/cards
```

### Example response

```json
{
    "page": 1,
    "limit": 25,
    "total": 384,
    "totalPages": 16,
    "data": [
        {
            "id": "origins-proving-grounds-001/024",
            "number": "001/024",
            "code": "001/024",
            "name": "Annie - Fiery",
            "images": {
                "small": "https://tcgplayer-cdn.tcgplayer.com/product/653136_400w.jpg",
                "large": "https://tcgplayer-cdn.tcgplayer.com/product/653136_in_1000x1000.jpg"
            },
            "set": {
                "id": "origins-proving-grounds",
                "name": "Origins: Proving Grounds",
                "releaseDate": "2025-10-31"
            },
            "tcgplayer": {
                "id": 653136,
                "url": "https://www.tcgplayer.com/product/653136/riftbound-league-of-legends-trading-card-game-origins-proving-grounds-annie-fiery"
            },
            "cleanName": "Annie Fiery",
            "rarity": "Epic",
            "cardType": "Champion Unit",
            "domain": "Fury",
            "energyCost": "5",
            "powerCost": "1",
            "might": "4",
            "description": "Your spells and abilities deal 1 Bonus Damage.<br>\r\n<em>(Each instance of damage they deal is increased by 1.)</em>",
            "flavorText": "<em>\"I never play with matches.\"</em>",
            "presaleInfo": {
                "isPresale": true,
                "releasedOn": "2025-10-31T00:00:00",
                "note": "This product is a Presale item and will ship in October 2025. Orders containing Presale items will ship once all items are available. Release date and product details are subject to change as updated by the publisher."
            },
            "modifiedOn": "2025-09-09T18:03:03.603"
        }
    ]
}
```

### Available properties

These are the properties you can use to filter the available cards:

- id
- code 
- name