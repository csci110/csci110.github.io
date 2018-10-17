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
// Is there none, or is its *top* above the bottom of the princess?
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

# Platform Princess, Part 2

## Along came a spider

The first enemies are the spiders. They will move up and down the screen on their webs. If they collide with the princess, she will be knocked off balance— possibly into the water.

- [ ] Define a class called `Spider` whose constructor function accepts two arguments (x, and y) and does the following for each instance of the class:

- gives it a name
- sets its image file as `spider.png`
- sets its x and y coordinates equal to the values passed to the constructor
- sets its speed to 48
- makes it immovable (`accelerateOnBounce` = false)
- defines an animation called 'creep' which uses all three frames in the image sheet
- plays the animation continuously (see the sgc help file if you forget how to set the `playAnimation()` method to repeat)

- [ ] Create two spiders, one at (200, 225), and the other at (550, 200).

- [ ] Run your game and test that spiders appear on the wall, "creeping" in place and minding their own business.  That is about to change.

  We will program two parts to the spider's movement behavior, depending on its position relative to the main area of the princess's travel.  This is the area just above the two sliders.  It is the spider's "attack zone."  If a spider is above the attack zone, it will drop.  If a spider is below the attack zone, it will move back up.

  Let's define the top of the attack zone as anything more than one `ann` height higher than the princess (since an object there would be sitting just on top of her head) and the bottom of the attack zone as the y coordinate of the princess.

  - [ ] In the `Spider` class, define a `handleGameLoop()` method that:
    * tests to see if the spider is *above* the attack zone.  If so, move it down by setting the angle to 270. 
    * tests to see if the spider is *below* the attack zone.  If so, move it up by setting the angle to 90.

  - [ ] Run your game and test to see if the spiders move to below the level of the princess's head if they are above it and jump back up when they get to the level of her feet, whether she is standing on the starting platforms or one of the sliders. Notice what happens when the spiders collide with the princess.

### Overriding the collision handler

What happens if we just use the default "bounce" behavior for collisions between `ann` and the spiders? If one of the spiders collides with the princess by landing on her head, the spider will have a y-component to its motion that will result in the princess bouncing *down*. During first game loop in which the princess goes more than one pixel below the top of a `platform` object, the `princess.isFalling` flag will be set, and since there are no more platforms below her she will fall off the bottom of the screen. That seems ok -- if a giant spider lands on your head while you are balancing on a log in turbulent underground river, you might very well fall off the log.  However the collision box is currently too big; the actual spider image (the non-transparent part of the image) only takes up about half the width of the frame and is maybe 30 pixels high. 

- [ ] Override the `Sprite` class's collision handler by replacing it with this:

```
handleCollision(otherSprite) {
    // Spiders only care about collisons with Ann.
    if (otherSprite === ann) {
	    // Spiders must hit Ann on top of her head.
   		let horizontalOffset = this.x - otherSprite.x;
   		let verticalOffset = this.y - otherSprite.y;
   		if (Math.abs(horizontalOffset) < this.width / 2 && 
   		    Math.abs(verticalOffset) < 30) {
        		otherSprite.y = otherSprite.y + 1; // knock Ann off platform
   		}
   	}
    return false;
}
```

Remember that if the collision handler returns `false`, the colliding object will not bounce off after the collision.  This isn't a JavaScript thing; here is no way to know this without looking at the sgc documentation for `handleCollision()`.

- [ ] Run your game and verify that the princess falls off the platform if the spider lands on her head.

## Attack of the bats

The next type of enemy is a bat.   They will hover about their starting locations until they (randomly) decide to dive-bomb the princess.  

- [ ] Define a class called `Bat` that uses the `bat.png` image file, x and y coordinates passed to the constructor, doesn't bounce when hit, has a name like "A scary bat," and continuously plays an animation called "flap" that uses both frames in the image sheet.  

- [ ] Create an instance of the `Bat` class at location (200, 100) called `leftBat`.

- [ ] Create an anonymous instance of the `Bat` class at location (500, 75) called `rightBat`.

- [ ] Run your game and verify that the bats appear and flap around in place. 

We will program two parts to the bats' behavior.  One where they fly about slowly in random (diagonal) directions, and one in attack (dive-bomb) mode.

Let's define the attack mode first.

- [ ] Still inside the constructor of the `Bat` class, initialize a custom variable called `attackSpeed` and set it to 300.  

- [ ] In the `Bat` class definition, but outside the constructor method, define a method called `attack()` that does two things:

- changes the speed of the bat to the value of the `attackSpeed` variable.
- uses the `aimFor` method to set the angle of the bat to point to the current location of the princess, like this:

```javascript
this.aimFor(ann.x, ann.y);
```

(see `Sprite` class properties in the Game Controller documentation for more information on this method)

We want the bat to fly around randomly most of the time, but one time in 1000 game loops, the bat should fly directly toward the princess at high speed.

Unfortunately there is a spooky castle wall at y = 175.  If we use the parent's collision handler, the bats will just bounce off the wall and never hit the princess.  You know what to do: override the parent's collision handler!

- [ ] Using the `Spider` class collision handler as a guide, override the `handleCollision(otherSprite)` method so that bats only care about collisions with `ann` and increase her y coordinate by 1 when they hit. Don't worry about reducing the collision box like we did with the spiders.  Return `false` as before.  

- [ ] Define a `handleGameLoop` method that tests to see if `Math.random()` is less than 0.001.  If so, call the bat's attack method.  We will do more with this after we test what we have so far.  For now you can create a *stub* here by adding a comment after the the `if` statement closing bracket that says `// if bat is not attacking: hover`. 

- [ ] Run the game and see if the bats randomly dive bomb the princess and knock her off the platforms. 

## Bat hovering

Looking back at the stub in your `handleGameLoop` method, we see that we wanted to test to see if the bat is not attacking.  There are few ways we might do that.  For example, we might check its current location (x and y coordinates) and see if it is different from the starting location.  As our code is currently written, that would necessarily mean the bat had started his bombing run.  But if we start fluttering about (hovering), the position will change even though the bat is not attacking.  So we need something else.  How about speed?  If attacking, the bat will be at speed 300, and as long as we pick a different speed for hovering, we could test the bat's speed variable.  In order to make our code more readable, let's define two custom variables for these speeds.

- [ ] Inside the constructor method of the `Bat` class, define a custom variable called `normalSpeed` and set its value to 20.  While we are at it, lets set the object's `speed` to this value too.  One way to do this is with a *chained assignment* statement that looks like this:

```javascript
this.speed = this.normalSpeed = 20;
```

This works because the right-hand side of an assignment must be evaluated before the assignment can happen.  Read [this](https://en.wikipedia.org/wiki/Operator_associativity#Right-associativity_of_assignment_operators) paragraph in Wikipedia for more information about operator associativity.

Now we are ready to write an `if` statement that tests to see if the bat is attacking.  But first let's talk about the hovering behavior we would like to program for the bats:

- give each bat a random diagonal direction when it is created

- every 3 seconds, increase the `angle` property of the bat either by 90 degrees or 180 degrees.  Whether it changes by 90 or 180 degrees depends on the flip of a coin:  50% of the time it does one rather than the other.

The first bullet sets the starting angle. We will use `Math.round()` on the `Math.random()` method to get a random *integer* between (and including) 0 and 3, then multiply that integer by 90 to pick out one of 4 directions.  See [mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round) for an explanation of `Math.round()`.  This is a useful trick that we will use again.

- [ ] In the constructor function method for the `Bat` class, set the `angle` property to 

```javascript
this.angle = 45 + Math.round(Math.random() * 3) * 90;
```

To program the second bullet, you might want to review the "spell cast timer" we used to limit Marcus's spell casting in `wizardDuel`.  The relevant block of code might have looked like this:

```javascript
handleSpacebar() {
    let now = game.getTime();
    if (now - this.spellCastTime >= 2) {
        this.spellCastTime = now;
        // cast a spell
    }
}
```

Instead of `spellCastTime` use a descriptive name like `angleTimer` for example.  Your `angleTimer` code will look similar to the code snippet above, except the timer duration will be different.

- [ ] Inside the `Bat` class `constructor()` method, define a custom variable called `angleTimer` and set it to zero.

- [ ] Inside the `Bat` class `handleGameLoop()` method, and using the `normalSpeed` variable, test to see if the bat is hovering.  If so, start a 5 second timer to change the bat's direction by 90 degrees 50% of the time, and 180 degrees the other 50% of the time.  Challenge yourself to see if you can do this using the rounding trick we used to set the original angle, instead of writing a second `if` statement.

You might run into a problem with your first `if` statement if you used `(this.speed === this.normalSpeed)` for your conditional expression.  You can find the cause of the problem by adding `console.log(this.speed)` inside the `handleGameLoop()` method.  You should have seen 20.000000000000004 whereas `leftBat.normalSpeed` returns 20.  We will learn more about why this happens next semester, but if you want a sneak preview, read this short article on [Avoiding Problems with Decimal Math in JavaScript](http://adripofjavascript.com/blog/drips/avoiding-problems-with-decimal-math-in-javascript.html).  For now you can just round `this.speed` and your conditional statement will return true when the speed is *about* 20.

## Keeping the bats in play

Lastly we need to figure out what to do about bats hovering or dive-bombing out of the display area.  Let's say if a bat reaches the bottom or sides of the screen, we just send it back to its starting point, perhaps representing the creation of a new bat to take its place.  But if it hovers to the top of the screen, we will just keep it from leaving the display area by resetting its y coordinate to zero.

First we need to keep track of the bat's starting position by creating two custom variables.

- [ ] In the constructor of the `Bat` class definition, create a custom variable called `startX` and set its value equal to the x value passed to the constructor.  You can do this by modifying your existing `this.x` assignment statement with a chained assignment statement like this if you like:

```javascript
this.x = this.startX = x;
```

- [ ] Do the same for the starting y coordinate with a variable called `startY`.

Now that we have stored the starting coordinates for each instance of the `Bat` class, we can define our `handleBoundaryContact` method.

- [ ] In the `Bat` class definition, define a `handleBoundaryContact()` method that does the following:

- if the bat's `y` coordinate is less than zero, set it equal to zero
- if the bat's `y` coordinate is greater than `game.displayHeight`, reset the bat's position to its starting position, its `speed` to `this.normalSpeed`, and its `angle` to 225 degrees.

- [ ] Run your game and verify that:

- the bats hover about, changing directions every three seconds, either by 90 or 180 seconds
- the bats return to their starting positions if they leave the left or right or bottom of the display
- the bats hug the top of the display if they touch it (until another direction change happens)

##Wrapping Up

This completes the platform princess tutorial. 

- [ ] Read [The Town of Bool: Part 1 of Ann's encounter with the Booleans](http://computationaltales.blogspot.com/2011/05/town-of-bool.html)
- [ ] Read [The Marvelous IF-ELSE Life of the King's Turtle](http://computationaltales.blogspot.com/2011/09/marvelous-if-else-life-of-kings-turtle.html)

Use question set 5 (Platform Princess) to review and evaluate your mastery of new concepts.

- [ ] Using the png files in Sakai [Platform images folder](https://sakai.lampschools.org/portal/site/8327cf3f-758a-4ce1-bb40-5a816869e7bc/tool/40fd529a-cd2b-46fd-bed3-501a3f39e116?panel=Main#)*, create a Halloween-themed platform game similar to platformPrincess with the following minimum requirements:

* one or more sliding platforms move vertically in addition to horizontally
* the background and platforms conform to the tile set in the Platform images folder
* the object of the game is to push a crate object to the right edge of the screen (or door)

You may remove the bat and spider objects and re-use the wooden platforms if you wish.  You can resize and otherwise molest the png files with a free program such as [IrfanView](http://www.irfanview.com/main_download_engl.htm).  

- [ ] Synchronize your finished project js file with GitHub.

\* The platform graphics were provided courtesy of [Game Art 2D](http://www.gameart2d.com/freebies.html).

![platformProject](/images/platformProject.PNG)