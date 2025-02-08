---
sidebar_position: 2
slug: /api-reference/card
---

# Card

## Get a specific card if you have the ID

## How to use

```cli
https://apitcg.com/api/[tcg]/cards/[id]
```

### Example request 

```cli
https://apitcg.com/api/one-piece/cards/OP06-014
```

### Example response

```json
{
    "data": {
        "id": "OP06-014",
        "code": "OP06-014",
        "rarity": "C",
        "type": "CHARACTER",
        "name": "Ratchet",
        "images": {
            "small": "https://en.onepiece-cardgame.com/images/cardlist/card/OP06-014.png?240222",
            "large": "https://en.onepiece-cardgame.com/images/cardlist/card/OP06-014.png?240222"
        },
        "cost": 1,
        "attribute": {
            "name": "Wisdom",
            "image": "https://en.onepiece-cardgame.com/images/cardlist/attribute/ico_type05.png"
        },
        "power": null,
        "counter": "1000",
        "color": "Red",
        "family": "FILM/Mecha Island",
        "ability": "[On Your Opponent's Attack] You may trash any number of {FILM} type cards from your hand. Your Leader or 1 of your Characters gains +1000 power during this battle for every card trashed.",
        "trigger": "",
        "set": {
            "name": "-WINGS OF THE CAPTAIN-[OP06]"
        },
        "notes": []
    }
}
```
