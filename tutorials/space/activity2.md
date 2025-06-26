# My Tutorial

### @explicitHints true


## Welcome!

In this tutorial, we will be learning spawning multiple enemies and the concept of arrays.
The map, movement and player attacks have already been set up for you.

If you are stuck or don't understand any step, do raise your hand to get a TA or ask the people around you!


## Spawners

It's a good design practice to give some sort of notice that enemies are about to spawn, so the player can react accordingly.
We can do this by spawning a warning symbol, and after a certain amount of time, have that warning symbol spawn the enemey.

Lets have this spawner spawn every 500ms. 
Add a ``||game.on game update ever ___ ms||`` block. 
In this block, we can create a new enemySpawner at a random spot in the tilemap
Use the ``||sprites.set [ ]||`` block to create a new spawner of type EnemySpawn, which has been created for you.
You can use the warning symbol or anything else as the sprite for it.

---

Using the ``||scene.place [ ] on top of random []||`` block, we can have this the spawner only spawn on the floor tiles.
You might have noticed that the spawnplayer function uses the same concept.

---



```block
let spawner : Sprite = null
game.onUpdateInterval(500, function () {
   
        spawner = sprites.create(img`
            . . . . . . . 4 4 . . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . 4 5 5 5 5 4 . . . . . 
            . . . . . 4 5 f f 5 4 . . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . 4 5 5 5 5 f f 5 5 5 5 4 . . 
            . . 4 5 5 5 5 5 5 5 5 5 5 4 . . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            4 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 
            4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 
            `, SpriteKind.EnemySpawn)
        tiles.placeOnRandomTile(spawner, sprites.dungeon.darkGroundCenter)
    
})
```

## Effects

To have the enemySpawner destroy after a certain time, we can use ``||Sprites.set [ ] lifespan to ___ ||``. 
We can also use a ``||Sprites.[ ] start [ ] effect||`` to have an effect play throughout its lifespawn.
Try adding an effect for 1000ms, and also set its lifespawn to 1000ms.

---
You can try walking around the level to see if that works. It should appear, play its effect and disappear after.
#### ~ tutorialhint
```block
let spawner : Sprite = null
game.onUpdateInterval(500, function () {
   
        spawner = sprites.create(img`
            . . . . . . . 4 4 . . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . 4 5 5 5 5 4 . . . . . 
            . . . . . 4 5 f f 5 4 . . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . 4 5 5 5 5 f f 5 5 5 5 4 . . 
            . . 4 5 5 5 5 5 5 5 5 5 5 4 . . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            4 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 
            4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 
            `, SpriteKind.EnemySpawn)
        tiles.placeOnRandomTile(spawner, sprites.dungeon.darkGroundCenter)
        spawner.startEffect(effects.disintegrate, 1000)
        spawner.lifespan = 1000
})

```

## The enemy

Now with an ``||Sprites.on destroy[ ] of kind||`` block, we can have code run whenever a spawner is destroyed.
Set the block to of kind EnemySpawn, and in it, create a new sprite of type enemy.
Set its position to the sprite, which will be the spawner that is being destroyed, and have it follow the player.
You can use ``||Sprites.follow||``.
#### ~ tutorialhint
```block 
let ghost : Sprite = null
let mySprite : Sprite = null
sprites.onDestroyed(SpriteKind.EnemySpawn, function (sprite) {
    ghost = sprites.create(img`
        ........................
        ........................
        ........................
        ........................
        ..........ffff..........
        ........ff1111ff........
        .......fb111111bf.......
        .......f11111111f.......
        ......fd11111111df......
        ......fd11111111df......
        ......fddd1111dddf......
        ......fbdbfddfbdbf......
        ......fcdcf11fcdcf......
        .......fb111111bf.......
        ......fffcdb1bdffff.....
        ....fc111cbfbfc111cf....
        ....f1b1b1ffff1b1b1f....
        ....fbfbffffffbfbfbf....
        .........ffffff.........
        ...........fff..........
        ........................
        ........................
        ........................
        ........................
        `, SpriteKind.Enemy)
    ghost.follow(mySprite, 50)
    ghost.setPosition(sprite.x, sprite.y)
})

```
## Arrays

One issue you might encounter is tons of enemies spawning if you don't kill them fast enough. 
We should limit the number of enemies that can be alive to prevent this. 

**Arrays** are containers that can hold multiple variables in it. We can have an array of enemy sprites, which will have every enemy currently alive in it.
In makecode, when you spawn sprites of a certain type, they are automatically added into an array for you.

The ``||Arrays.Arrays||`` tab shows some of the blocks that can be used, it is found in the advanced dropdown..
Somewhat unintuitively, one block that is very useful is ``||Sprites.array of sprites of kind [ ]||``
We can use this to get the length of the enemy array. The length is the number of sprites currently in it.

## Limiting Spawns

Using an ``||Logic:if||`` block, we can create spawners only when the length of the enemy array is less than 5.
Add this new if condition to your game update block. The final block should look like this.
```block
let spawner : Sprite = null
game.onUpdateInterval(500, function () {
    if (sprites.allOfKind(SpriteKind.Enemy).length < 5) {
        spawner = sprites.create(img`
            . . . . . . . 4 4 . . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . 4 5 5 5 5 4 . . . . . 
            . . . . . 4 5 f f 5 4 . . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . 4 5 5 5 5 f f 5 5 5 5 4 . . 
            . . 4 5 5 5 5 5 5 5 5 5 5 4 . . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            4 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 
            4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 
            `, SpriteKind.EnemySpawn)
        tiles.placeOnRandomTile(spawner, sprites.dungeon.darkGroundCenter)
        spawner.startEffect(effects.disintegrate, 1000)
        spawner.lifespan = 1000
    }
})
```

Trying it now, there shouldn't be more than 5 enemies at once.

## Visualizing how arrays work

Using arrays, we can do other interesting things with them. 

```customts 

namespace SpriteKind 
{

//% isKind
    export const EnemySpawn = SpriteKind.create()
}

```

```template
function SpawnPlayer () {
    mySprite = sprites.create(img`
        . . . . . . f f f f . . . . . . 
        . . . . f f f 2 2 f f f . . . . 
        . . . f f f 2 2 2 2 f f f . . . 
        . . f f f e e e e e e f f f . . 
        . . f f e 2 2 2 2 2 2 e e f . . 
        . . f e 2 f f f f f f 2 e f . . 
        . . f f f f e e e e f f f f . . 
        . f f e f b f 4 4 f b f e f f . 
        . f e e 4 1 f d d f 1 4 e e f . 
        . . f e e d d d d d d e e f . . 
        . . . f e e 4 4 4 4 e e f . . . 
        . . e 4 f 2 2 2 2 2 2 f 4 e . . 
        . . 4 d f 2 2 2 2 2 2 f d 4 . . 
        . . 4 4 f 4 4 5 5 4 4 f 4 4 . . 
        . . . . . f f f f f f . . . . . 
        . . . . . f f . . f f . . . . . 
        `, SpriteKind.Player)
    controller.moveSprite(mySprite)
    tiles.placeOnRandomTile(mySprite, sprites.dungeon.collectibleInsignia)
    scene.cameraFollowSprite(mySprite)
}
sprites.onOverlap(SpriteKind.Enemy, SpriteKind.Projectile, function (sprite, otherSprite) {
    sprites.destroy(sprite)
})
let spawnIndicator: Sprite = null
let Player_AOE: Sprite = null
let mySprite: Sprite = null
let ghost: Sprite = null
tiles.setCurrentTilemap(tilemap`level1`)
SpawnPlayer()
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    Player_AOE = sprites.create(img`
        ................................
        ..........22222222222...........
        ........222222222222222.........
        ......2222222222222222222.......
        .....222222222222222222222......
        ....22222222222222222222222.....
        ...2222222222222222222222222....
        ..222222222222222222222222222...
        ..222222222222222222222222222...
        .22222222222222222222222222222..
        .22222222222222222222222222222..
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        2222222222222222222222222222222.
        .22222222222222222222222222222..
        .22222222222222222222222222222..
        ..222222222222222222222222222...
        ..222222222222222222222222222...
        ...2222222222222222222222222....
        ....22222222222222222222222.....
        .....222222222222222222222......
        ......2222222222222222222.......
        ........222222222222222.........
        ..........22222222222...........
        `, SpriteKind.Projectile)
    Player_AOE.setPosition(mySprite.x, mySprite.y)
    Player_AOE.lifespan = 300
    Player_AOE.z = -1
    Player_AOE.setFlag(SpriteFlag.Invisible, false)
})

```

```ghost
list.length()
game.onUpdateInterval(500, function () {
    if (sprites.allOfKind(SpriteKind.Enemy).length < 5) {
        spawnIndicator = sprites.create(img`
            . . . . . . . 4 4 . . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . . 4 5 5 4 . . . . . . 
            . . . . . 4 5 5 5 5 4 . . . . . 
            . . . . . 4 5 f f 5 4 . . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . . 4 5 5 f f 5 5 4 . . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . . 4 5 5 5 f f 5 5 5 4 . . . 
            . . 4 5 5 5 5 f f 5 5 5 5 4 . . 
            . . 4 5 5 5 5 5 5 5 5 5 5 4 . . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            . 4 5 5 5 5 5 f f 5 5 5 5 5 4 . 
            4 5 5 5 5 5 5 5 5 5 5 5 5 5 5 4 
            4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 
            `, SpriteKind.EnemySpawn)
        tiles.placeOnRandomTile(spawnIndicator, sprites.dungeon.darkGroundCenter)
        spawnIndicator.startEffect(effects.disintegrate, 1000)
        spawnIndicator.lifespan = 1000
    }
})
let list = [1, 2, 3]
list[0]
list.push(1)

```



```jres
{
    "transparency16": {
        "data": "hwQQABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",
        "mimeType": "image/x-mkcd-f4",
        "tilemapTile": true
    },
    "level1": {
        "id": "level1",
        "mimeType": "application/mkcd-tilemap",
        "data": "MTAxMDAwMTAwMDAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMjAyMDIwMTAxMDIwMTAxMDEwMTAyMDEwMTAxMDEwMTAxMDEwMTAxMDEwMjAxMDEwMTAxMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAyMDEwMTAxMDEwMjAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMTAxMDEwMTAyMDEwMTAxMDEwMzAxMDEwMTAxMDEwMjAxMDEwMTAxMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAyMDEwMTAxMDEwMjAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMTAxMDEwMTAyMDEwMTAxMDEwMjAyMDIwMjAxMDEwMjAxMDEwMTAxMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDIyMjIwMDAyMDAyMDAwMDAwMDAwMDAwMjAwMjAwMDAwMDAwMDAwMDIwMDIwMDAwMDAwMDAwMDAyMDAyMDAwMDAwMDAwMDAwMjAwMjAwMDAwMDAwMDAwMDIwMDIwMDAwMDAwMDAwMDAyMDAyMDAwMDAyMjIyMDAwMjAwMjAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMA==",
        "tileset": [
            "myTiles.transparency16",
            "sprites.dungeon.darkGroundCenter",
            "sprites.dungeon.floorLight0",
            "sprites.dungeon.collectibleInsignia"
        ],
        "displayName": "level1"
    },
    "*": {
        "mimeType": "image/x-mkcd-f4",
        "dataEncoding": "base64",
        "namespace": "myTiles"
    }
}
```
