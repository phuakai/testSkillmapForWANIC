# Get to Know MakeCode Arcade 


```ghost
let mySprite: Sprite = null;
mySprite.startEffect(effects.spray)
controller.A.onEvent(ControllerButtonEvent.Pressed, function () {
    game.showLongText("The little unicorn walked into the meadow.", DialogLayout.Top)
    scene.cameraShake(4, 500)
})
scene.setBackgroundColor(9)
scene.setBackgroundImage()
mySprite.x += 0
effects.confetti.startScreenEffect()
effects.confetti.endScreenEffect()
mySprite.setPosition(70, 80)
for (let index = 0; index < 4; index++) {
    controller.moveSprite(mySprite)
    music.setVolume(20)
    music.playMelody("- - - - - - - - ", 120)
}
game.onUpdateInterval(5000, function () {
    if (game.askForString("Continue?") == "Y" || game.askForString("Continue?") == "y") {
        mySprite.say(":)")
    }
    game.splash("")
})

characterAnimations.loopFrames(
mySprite,
[img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `],
500,
characterAnimations.rule(Predicate.NotMoving)
)

characterAnimations.runFrames(
mySprite,
[img`
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    . . . . . . . . . . . . . . . . 
    `],
500,
characterAnimations.rule(Predicate.NotMoving)
)

characterAnimations.rule(predicate.NotMoving)
characterAnimations.clearCharacterState(mySprite)
characterAnimations.setCharacterState(mySprite, characterAnimations.rule(Predicate.NotMoving))
loops.forever()

```

### @explicitHints true

```package
arcade-character-animations
arcade-character-animations=github:microsoft/arcade-character-animations
```
## 

**⭐Welcome⭐**

In this tutorial, we will be learning how to make a ghost enemy shoot in the player characters direction.

---


```template
let mySprite = sprites.create(img`
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
let Ghost = sprites.create(img`
    ........................
    ........................
    ........................
    .........333333.........
    .......333ffff333.......
    ......33ff1111ff33......
    ......3fb111111bf3......
    ......3f11111111f3......
    ......fd11111111df......
    ......fd11111111df......
    ......fddd1111dddf......
    ......fbdbfddfbdbf......
    ......fcdcf11fcdcf......
    .......fb111111ffff.....
    ......fffcdb1bc111cf....
    ....fc111cbfbf1b1b1f....
    ....f1b1b1ffffbfbfbf....
    ....fbfbfffffff.........
    .........fffff..........
    ..........fff...........
    ........................
    ........................
    ........................
    ........................
    `, SpriteKind.Enemy)
Ghost.setPosition(80, 10)

```



## Spawning projectiles

Let's start by having a projectile spawn. While makecode comes with a set projectile block, we will not be using that as it comes with a few issues.

---

By using set sprite and set sprite location, we can have a projectile spawn on top of our ghost.
Place a ``||sprites.set Sprite||`` block and a ``||sprites.set position||`` block in the on start container. 
Give this sprite a new variable name, like "bullet" and set it's kind to projectile.

---

Use any sprite you want the ghost to shoot, and try to set it's position to the same spot as the ghost using the ghosts x and y.
#### ~ tutorialhint
```block
let Bullet : Sprite = null
let Ghost : Sprite = null
Bullet.setPosition(Ghost.x,Ghost.y))
```


## Points and Vectors

Before we make our bullets move, we must first understand **vectors**. To get the vector, or line,
from the bullet to the player, we should take the player coordinate and subtract the bullets coordinate.
A good way to remember is always subtract the destination point from the origin.

## Velocity
Now with our vector, we can use it to make things move. The term **velocity** means the speed and direction that an object is moving in.
With makecode, we can set sprites to a set velocity. This uses a ``||sprites.set sprite velocity||`` block, that has vx (velocity x) and vy (velocity y) as it's input.

Try setting the bullets velocity to (0, 50) and see what happens when you press run.

## What happens when you set the velocity to a different number?  @unplugged

![Directional Projectiles](/static/skillmap/space/vxvy.gif "Round and Round")

## Spawning on a timer

Right now, our ghost should be firing just one projectile. To have it always fire on a set interval, we can use a ``||loops.forever||`` loop
together with a ``||loops.Pause for||``. Move the blocks for setting the bullets sprite, position and velocity to this new forever block. 
Set the timer to 1000 ms. The ghost should now be firing a bullet every second.

#### ~ tutorialhint
```block
let Bullet: Sprite = null
forever(function () {
    
    Bullet = sprites.create(img`
        . . . . c c c b b b b b . . . . 
        . . c c b 4 4 4 4 4 4 b b b . . 
        . c c 4 4 4 4 4 5 4 4 4 4 b c . 
        . e 4 4 4 4 4 4 4 4 4 5 4 4 e . 
        e b 4 5 4 4 5 4 4 4 4 4 4 4 b c 
        e b 4 4 4 4 4 4 4 4 4 4 5 4 4 e 
        e b b 4 4 4 4 4 4 4 4 4 4 4 b e 
        . e b 4 4 4 4 4 5 4 4 4 4 b e . 
        8 7 e e b 4 4 4 4 4 4 b e e 6 8 
        8 7 2 e e e e e e e e e e 2 7 8 
        e 6 6 2 2 2 2 2 2 2 2 2 2 6 c e 
        e c 6 7 6 6 7 7 7 6 6 7 6 c c e 
        e b e 8 8 c c 8 8 c c c 8 e b e 
        e e b e c c e e e e e c e b e e 
        . e e b b 4 4 4 4 4 4 4 4 e e . 
        . . . c c c c c e e e e e . . . 
        `, SpriteKind.Projectile)
        
    
    Bullet.setPosition(80,10)
    Bullet.setVelocity(0,50)
    pause(1000)
})
```


## Firing at the player.

Now that we have the ghost constantly firing, we can move on to making it target the player.

Recalling how to get the vector from the ghost to the player, we can change our set velocity block to do the calculation
using a combination of math blocks. 

Replace the vx value with the ``||math:[] - []||`` block, then add the ``||sprites: [ ] x||`` blocks as the inputs like so.
Do the steps for the vy value too, your final velocity block should look like this.
```block
let Bullet: Sprite = null
let mySprite: Sprite = null
Bullet.setVelocity(mySprite.x - Bullet.x, mySprite.y - Bullet.y)
```
---
Try moving around in the level now!


## Vector normalization

You might have noticed that the closer you are to the ghost, the slower the projectile is.

This is because when we are closer, the vector from the ghost to the player is shorter, and so the speed is slower. 

To avoid this problem, we must use the concept of **normalization.** Normalizing a vector changes the vectors length, or magnitude, to 1, so we can get just the direction.

To normalize, we must first get the length of the vector. We can think of the vector as the longest side of a right angle triangle, 
with the two other sides being the x and y axis. We can then use the Pythagoras Theorem to get the length.

Then, we divide the x and y component by this length. 

In makecode, we use the ``||math:square root [ ] ||`` and ``||math: [ ] **[ ]||`` to get it's length.

## Getting the distance

To make things neater, we can use variables to keep track of our different values.
The x and y components of the vector can be split into two variables. 

```block
let Bullet: Sprite = null
let mySprite: Sprite = null
    directionX = mySprite.x - Bullet.x
    directionY = mySprite.y - Bullet.y
```

Then the length can be it's own variable, direction.

```block
 distance = Math.sqrt(directionY ** 2 + directionX ** 2)
```

## Normalizing our vector
Finally we're on our last step! Now we just have to divide distanceX and distanceY by our distance variable.

```block
    directionY = directionY / distance
    directionX = directionX / distance
```
This will change the values in the variables.

With this, we can then set the velocity of the bullet to any number we want, just multiply the direction with a speed.

```block 
let Bullet: Sprite = null
Bullet.setVelocity(directionX * speed, directionY * speed)
```

Putting it all together, our final forever loop should look something like this.
```block
let Bullet: Sprite = null
forever(function () {
    
    Bullet = sprites.create(img`
        . . . . c c c b b b b b . . . . 
        . . c c b 4 4 4 4 4 4 b b b . . 
        . c c 4 4 4 4 4 5 4 4 4 4 b c . 
        . e 4 4 4 4 4 4 4 4 4 5 4 4 e . 
        e b 4 5 4 4 5 4 4 4 4 4 4 4 b c 
        e b 4 4 4 4 4 4 4 4 4 4 5 4 4 e 
        e b b 4 4 4 4 4 4 4 4 4 4 4 b e 
        . e b 4 4 4 4 4 5 4 4 4 4 b e . 
        8 7 e e b 4 4 4 4 4 4 b e e 6 8 
        8 7 2 e e e e e e e e e e 2 7 8 
        e 6 6 2 2 2 2 2 2 2 2 2 2 6 c e 
        e c 6 7 6 6 7 7 7 6 6 7 6 c c e 
        e b e 8 8 c c 8 8 c c c 8 e b e 
        e e b e c c e e e e e c e b e e 
        . e e b b 4 4 4 4 4 4 4 4 e e . 
        . . . c c c c c e e e e e . . . 
        `, SpriteKind.Projectile)
        
    
    Bullet.setPosition(80,10)
    Bullet.setVelocity(0,50)
    pause(1000)
})

```

## Further ideas

Now with this knowledge, you can try changing the speed of the bullets, how fast they fire, or even have the ghost move around while firing!
