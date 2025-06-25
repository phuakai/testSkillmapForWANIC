# Collision separation  
```ghost
//no idea why it didnt work when i just had the let sprite and flags on
namespace SpriteKind {
    export const none = SpriteKind.create()
}
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    PlayerRender.setImage(img`
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        `)
})
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    PlayerRender.setImage(img`
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        8 8 . . . . . . . . . . . . . . 
        `)
})
let PlayerRender: Sprite = null
tiles.setCurrentTilemap(tilemap`level1`)
// Create visible part of player. Use animations on this
PlayerRender = sprites.create(img`
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
let PlayerCollider = sprites.create(img`
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 
    `, SpriteKind.Player)
controller.moveSprite(PlayerCollider)
// Makes things ignore PlayerRender
PlayerRender.setFlag(SpriteFlag.Ghost, true)
scene.cameraFollowSprite(PlayerCollider)
// Makes PlayerCollider invisible
PlayerCollider.setFlag(SpriteFlag.Invisible, true)
forever(function () {
    PlayerRender.setPosition(PlayerCollider.x, PlayerCollider.y)
})

```


## How collisions in makecode works

In makecode, there's default behaviours to stop sprites from walking through walls. 
It's easy to set up a level with tilemaps and walls, saving us a bit of time writing our own collision checks.

## The issue

The issue is that the default way collisions are checked is with the actual sprite itself.
This can lead to some problems when we use different sprites and animations for a player or other entity. 
When sprites change, this can cause them to clip into walls or bounce around.

## Testing the issue

This level has been set up to visualize the problem. Move the player (the rectangle) to the flower tiles, which are walls created in the tilemap editor.
Make sure you are right beside the wall. 

Now press the b button to swap your sprite, you'll now be stuck inside the wall. Swapping back will snap you back out of the wall.
If you try it with the 1 tile wall, your red sprite can actually go over the wall! 

This is an extreme example but it can cause many bugs when you start doing animations.

## Fixing the issue

A common concept in game programming is seperating the sprite from it's collider, which is what we will use to check if a sprite is colliding with a wall.
I
n makecode, we can do that by creating another sprite, let's call it PlayerCollider. For it's sprite, we should color in the entire area to make a large square.
Using the ``||sprites:set [] auto destroy <on>||`` block, the dropdown options has one to set the sprite as invisible, which we will do.
Using the same block, we can also set ghost mode on the playerRender on, this will prevent the render sprite from colliding with anything.

---

Lets change move playerRender to move the playerCollider instead, and set the camera to follow the collider as well. The last step is to 
include a forever loop, that moves the playerRender position to the playerColliders x and y, basically having it follow the collider.
#### ~ tutorialhint
```block
let PlayerRender : Sprite = null
let PlayerCollider : Sprite = null
forever(function () {
    PlayerRender.setPosition(PlayerCollider.x, PlayerCollider.y)
})
```

## Testing it out

Let's try the level again! This time you should be unable to clip through the wall or have your sprite bounce around, the sprite is constrained by the invisible square collider.

```template
namespace SpriteKind {
    export const none = SpriteKind.create()
}
controller.B.onEvent(ControllerButtonEvent.Pressed, function () {
    PlayerRender.setImage(img`
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        . . . . . . . . . . . . . . 2 2 
        `)
})
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    PlayerRender.setImage(img`
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
        `)
})
let PlayerRender: Sprite = null
tiles.setCurrentTilemap(tilemap`level`)
PlayerRender = sprites.create(img`
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    9 9 . . . . . . . . . . . . . . 
    `, SpriteKind.Player)
controller.moveSprite(PlayerRender)
scene.cameraFollowSprite(PlayerRender)


```



    
```jres
{
    "transparency16": {
        "data": "hwQQABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",
        "mimeType": "image/x-mkcd-f4",
        "tilemapTile": true
    },
    "tile1": {
        "data": "hwQQABAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA==",
        "mimeType": "image/x-mkcd-f4",
        "tilemapTile": true
    },
    "level": {
        "id": "level",
        "mimeType": "application/mkcd-tilemap",
        "data": "MTAxMDAwMTAwMDAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMjAyMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAyMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMjAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMjAyMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAyMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMjAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAyMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDIwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDEwMTAxMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAyMjAwMDAwMDAwMDAwMDAwMjIwMDAwMDAwMDAwMDAwMDIyMDAwMDAwMDAwMDAwMDAyMjAwMDAwMDAwMDAwMDAwMjIwMDAwMDAwMDAwMDAwMDAyMDAwMDAwMDAwMDAwMDAwMjAwMDAwMDAwMDAwMDAwMDIwMDAwMDAwMDAwMDAwMDAyMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMDAwMA==",
        "tileset": [
            "myTiles.transparency16",
            "myTiles.tile1",
            "sprites.castle.tileGrass2"
        ]
    },
    "*": {
        "mimeType": "image/x-mkcd-f4",
        "dataEncoding": "base64",
        "namespace": "myTiles"
    }
}
```
