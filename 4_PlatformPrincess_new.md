# Platform Princess, Part 1

The instructions are deliberately sparse; please test often and make sure you have one section working without errors before moving on to the next. 

##Set the scene

- [ ] Import `game` and `Sprite` modules from "./sgc/sgc.js"

First we will create a background and then add some interesting visual effects.

- [ ] Set the background to `water.png` using the `game.setBackground()` method.

Now let's add a scrolling effect.  Consult the [sgc Help File](https://csci110.github.io/sgc/) and locate the `setBackground()` method in the `Game` class.  See if you can figure out how to set the arguments in this method in order to make the `water.png` image scroll horizontally at 500 pixels per second.


- [ ] Create a `wall` object from the `Sprite` class and give it the following properties:

* a name
* an image file (`wall.png`)
* set it's position to (x,y) = (0, 175)
* make it immovable by setting its `accelerateOnBounce` property to false.

This might be a good place to test your game so far.  You should see a wavy water effect at the bottom of the window.  Although this game is not a "scroller", a moving background is the main feature of such games. So now you know how to make your own scroller game.

## Platforms and Sliders

At the risk of stating the obvious, another feature of platform games is that the player character's motion is constrained by a series of platforms.  Just above the water we want to create starting and finishing platforms for the Princess to stand on, and two wooden platforms that slide back and forth in between them. The start and stop blocks are instances of a class we will call `Platform` and the sliders will be instances of a `Slider` class, both of which are derived from a class we will call `Support`.  So the inheritance structure looks like this.

`Sprite` class-> `Support` class -> `Platform` class and `Slider` class

 We do this because we anticipate that we will want to check under the princess for the existence of an object in either the `Platform` or `Slider` class, even though these objects behave quite differently.

###Platforms

- [ ] Define a class called `Support`, derived from the `Sprite` class, whose constructor function accepts three arguments (`x`, `y`, and `image`) and does the following for each instantiation of the class:
  - calls the parent class constructor (which doesn't accept any arguments)
  - sets this object's `x` and `y` coordinates equal to the `x` and `y` arguments passed to the constructor
  - passes the `image` variable to this object's  `setImage()` method

- [ ] Define a class called `Platform`, derived from the `Support` class, whose constructor function accepts the same three arguments (`x`, `y`, and `image`) and does the following for each instantiation of the class:

  - calls the parent class constructor (which accepts `x`, `y` and `image` arguments)

  - gives it a timeless and classy name like "A platform"
  - sets the `accelerateOnBounce` property to `false`.

As a test of your inheritance knowledge, see if you can answer this question to your satisfaction:  How come we didn't have to set the `x` and `y` coordinates, or call the `setImage()` function for objects in the `Platform` class?

- [ ] Create two instances of the `Platform` class at opposite sides of the display.  

  Call the one on the left `startPlatform` and the one on the right`finishPlatform`.  

  Use the `start.png` file for `startPlatform` and use the `finish.png` image file for `finishPlatform`.

  The position of `startPlatform` should be (0, 400), and the position of `finishPlatform` should be two `ann` widths away from the right edge of the display, at the same level. You can use `game.displayWidth - 48 * 2` for `x` if you wish, since we haven't created the Princess yet.

Run your game and test.  You should see two blocks on either edge of the screen near the top of the water with finished corners pointing in.

### Sliders

Now we will create a second child of the `Support` parent class to make the sliders.

- [ ] Define a class called `Slider` which is derived from the `Support` class.  Its constructor function should accept three arguments (`x`, `y`, and` angle`) and do the following for each instantiation of the class:

* call the parent class constructor function with the `super` keyword as usual, passing it the values of `x`, `y`, and `"slider.png"` for `x`, `y` and `image`, respectively.
* set the name to 'A sliding support'
* set the `angle` equal to the `angle` argument passed to the constructor
* set the speed equal to 48.  

This has the effect of moving the sliders 48 pixels per second in a direction defined by the `angle` property.

Notice that we are using the default value of `accelerateOnBounce` for objects in the class (true), which means they will bounce when there is a collision.

Did you also notice that we use `x`, `y` and *`angle`* arguments in the constructor of the child class, but we only passed two of those to the parent class constructor?  When we called the parent class constructor, we sent it the fixed value of `"slider.png"` in place of `image`, which the parent class needs for its constructor method.  Please make sure you understand this before you move on.  It seems like a subtle and small thing but it demonstrates the powerful flexibility of the inheritance system, and it might help you understand how the cascading `super()` calls work with derived classes.  It is not simple.

- [ ] Create two anonymous instances of the `Slider` class so that they are a few `ann` widths away from the start and finish platforms, and move opposite directions, like this:

```javascript
new Slider(startPlatform.x + 48 * 3, startPlatform.y + 48, 0);
new Slider(finishPlatform.x - 48 * 5, finishPlatform.y + 48, 180);
```

- [ ] Run your game and test.  You should see two wooden platforms floating just above the water and bouncing off of each other, and the start and finish blocks.  

Now that the sliders are working, you will program the player character's navigation, including jumping onto the sliders.

## Player Navigation in a Platformer Game

- [ ] Define a `Princess` class, derived from the `Sprite` class, whose constructor does the following for each instance of the class:

* Uses "ann.png" as the argument for the `setImage()` method
* set the position to (x,y) = (48, 300).  Yes, I know that is 100 pixels above the platform.  We are doing this on purpose to test her falling behavior later.
* set her `speed` to zero.
* create a custom variable called `speedWhenWalking` and set it equal to 125.
* define left and right animations, using frames 9-11, and 3-5 respectively

- [ ] Still inside the class definition, but outside the constructor, define keyboard methods for the left and right arrow keys (`handleLeftArrowKey()` and `handleRightArrowKey()`).  For now, these methods will do only these two things:

* Set the `angle` property to 180 or 0, as appropriate.  
* Set the `speed` property equal to `this.speedWhenWalking`.  

We will take care of the animations in a `handleGameLoop()` method.

- [ ] Still inside the `Princess` class definition, define a `handleGameLoop()` method that tests to see if the object's angle property is equal to zero **AND** the object's speed is greater than zero.  If so, play the `right` animation.  If the object's angle property is equal to 180 **AND** the object's speed is greater than zero, play the`left` animation.
- [ ] Also within the `HandleGameLoop()` method, use the Math.max() method to keep the x coordinate 5 pixels from the edge of the screen like this:

```javascript
this.x = Math.max(5, this.x);
```

Normally we would add a statement to set the speed to zero (when the arrow keys are not pressed) but we will make the game a little more challenging by not allowing her to stop.  Maybe she's frightened by something we will add to the game in the next section.

- [ ] Create an object called `ann` in the `Princess` class.

Run your game at test that you can move the princess left and right using the arrow keys, and that she can't move off the left edge of the screen.  

But she walks in midair!  She should fall any time that she is not on top of a platform:  either a slider or the two blocks on each edge of the moat.  To implement this, we will test to see if the princess is standing on an object in the `Support` class during each game loop (which includes the start and finish platforms and the sliders, since they are all children of the `Support` class).  If she is *not*, we will increase her y position by 4 pixels every second to simulate gravity.  Later, we will want to know if the princess is falling for other reasons too, so we will find it useful to set a *flag* that tells us when this is happening.  A flag is a Boolean variable that signals a particular condition or status.  Let's define a flag for the condition of the princess falling.  

- [ ] Within the `Princess` class constructor, define a custom variable called `isFalling` and set its value to `false`. 

There is a sgc method called `getSpritesOverlapping()` which might help us determine whether or not a `platform` sprite is directly under the `princess` sprite.  Please read the entry in the game controller help file about this method.

- [ ] Add the following to the `princess` object's `handleGameLoop` method definition:

```javascript
this.isFalling = false;  // assume she is not falling unless proven otherwise
// Check directly below princess for supports
let supports = game.getSpritesOverlapping(this.x, this.y + this.height, this.width, 1, Support);
// Is there none, or is its *top* at or below the bottom of the princess?
if (supports.length === 0 || supports[0].y < this.y + this.height) {
     this.isFalling = true;		// she is falling so ...
     this.y = this.y + 4; 		// simulate gravity
}
```

Some explanations are in order:

* We are using the `getSpritesOverlapping` method to get an array of sprites that overlap a 48-pixel-wide-by-1-pixel-deep rectangle whose upper left hand corner is at the *lower* left-hand corner of the `princess` sprite (remember the sprite's origin is in the upper left, so adding the sprites height to `y` places the rectangle right at her feet). 
* `supports` is a local variable we use to hold this sprite array.  If there is at least one sprite in the array, the *length* of the array (given by `supports.length`) will be greater than zero.
* To access the first element in the array, we use `supports[0]` since we start counting at zero. If there is something in the array it will be the start or finish platform, or one of the sliders.  So  `supports[0].y` marks the *top* of the overlapping platform sprite.
* If this is greater than or equal to the bottom of the princess sprite, she has landed on a platform and is therefore *not* falling.
* Otherwise, she is falling, and so we increase her `y` value by 4 pixels during the game loop.

Run the game and test that the princess falls onto the start block at the beginning of the game.  When you walk off the starting block. She should:

* land safely on a sliding platform, if one is there, and
* fall into the water if a sliding platform is not there.

You probably noticed that when the princess is on a slider, you must use the arrow keys to reverse direction and keep her from walking off the end. This is intentional; to improve the game challenge, there is no way to stop the princess once she begins moving.

With careful timing, you can walk from the first slider to the second. However, there is no way to walk *up* onto the finish block. As in most platformers, the player character needs to jump.

## Jumping

It wouldn't make sense for the `princess` to be able to jump unless she were standing on something, otherwise she could fly through the air by repeatedly jumping!  Now we can put that `isFalling` flag to good use.  

- [ ] Within the `Princess` class definition, define a `handleSpacebar()` method that tests to see if she is **not** falling. if so, move her up the screen 1.25 times her height, like this:

```javascript
if (!this.isFalling) {
    this.y = this.y - 1.25 * this.height; // jump
}
```

​Notice we are using the `!` (sometimes called the "bang" operator) for **NOT**.   In other words, it flips the value `true` to `false`, and vice-versa.  This is another way of saying `if (this.isFalling === false)`.  When the space bar is pressed, nothing will happen if the princess is falling. But if she isn't falling, she jumps up by 48*1.25 pixels, and then begins falling.

Run the game and test that the princess can:

* jump and land on the starting block,
* jump and land on sliders, and
* jump up onto the finish block.

## Losing the game

Apparently swimming is not among Princess Ann's many talents, so if she falls into the water, she sinks to the bottom and drowns.

- [ ] Within the `Princess` class definition, add a `handleBoundaryContact` method that ends the game with the message: 'Princess Ann has drowned.\n\nBetter luck next time.'

## Winning the game

Let's say there is a door on the right-hand platform that leads deeper into the castle.  If Princess Ann gets there, the player wins.

- [ ] Define a `Door` class derived from the `Sprite` class.  Define a constructor method that does the following:

* uses "door.png" as the argument for the `setImage()` method
* sets the x coordinate to  `game.displayWidth` - 48;
* sets the y coordinate to `finishPlatform.y` - 2 * 48;
* make it immovable (`accelerateOnBounce` = `false`)

- [ ] Add a collision handler `handleCollision(otherSprite)` that checks to see if `otherSprite` is equal to the `ann` object and ends the game with the message: 'Congratulations!\n\nPrincess Ann can now pursue the\nstranger deeper into the castle!'
- [ ] Create an object in the `Door` class called `exit`.  Set its name to "The exit door" or something similar.

Run your game and test the winning and losing conditions. 

Although you now have a complete game, it seems a bit simple and uninteresting. We will now add some enemies to increase the challenge.



##Wrapping Up

We're not quite done the tutorial yet.  If you are reading this, you are farther ahead than I expected.  Please use the extra time to start on the project below.  

-[ ] Read [The Town of Bool: Part 1 of Ann's encounter with the Booleans](http://computationaltales.blogspot.com/2011/05/town-of-bool.html)
-[ ] Read [The Marvelous IF-ELSE Life of the King's Turtle](http://computationaltales.blogspot.com/2011/09/marvelous-if-else-life-of-kings-turtle.html)

Use question set 5 (Platform Princess) to review and evaluate your mastery of new concepts.

-[ ] Using the png files in Sakai [Platform images folder](https://sakai.lampschools.org/portal/site/8327cf3f-758a-4ce1-bb40-5a816869e7bc/tool/40fd529a-cd2b-46fd-bed3-501a3f39e116?panel=Main#)*, create a Halloween-themed platform game similar to platformPrincess with the following minimum requirements:

* one or more sliding platforms move vertically in addition to horizontally
* the background and platforms conform to the tile set in the Platform images folder
* the object of the game is to push a crate object to the right edge of the screen (or door)

You may remove the bat and spider objects and re-use the wooden platforms if you wish.  You can resize and otherwise molest the png files with a free program such as [IrfanView](http://www.irfanview.com/main_download_engl.htm).  

-[ ] Upload your finished project js file to GitHub.

\* The platform graphics were provided courtesy of [Game Art 2D](http://www.gameart2d.com/freebies.html).

![platformProject](/images/platformProject.PNG)