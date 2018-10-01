# Platform Princess, Part 1

## Import the project archive files

-[ ] From the resources tab in Sakia, locate the `platformPrincess` folder and download the download all the files there.


-[ ] In your C9 workspace, create a new folder called `4.platformPrincess` and import your files there.  You can click and drag them from an explorer window into your new folder.  You should have:

* batSheet.png, door.png, finish.png, slider.png, start.png, wall.png, water.png
* princessSheet.png, spiderSheet.png
* platformPrincess.html

- [ ] Create a new file called `platformPrincess.js`

Now let's open the `platformPrincess.js` file and start writing some code!  The instructions are deliberately sparse; please test often and make sure you have one section working without errors before moving on to the next. 

##Set the scene

First we will create a background and then add some interesting visual effects.

- [ ] Add a `backgroundImageFile` called `water.png` (if you forget how to do this, refer to your Sprite project)


- [ ] Create a `wall` object with the following properties:

* a name
* an image file (wall.png)
* set it's position to (x,y) = (0, 175)
* make it immovable
* set its collisionArea property equal to { width: 1, height: 300 }


This last property will let us use the left edge of the wall to keep Princess on the screen.  Since it is not completely obvious, you might even want to add a comment to that effect!

-[ ] Preload the image file (before the object literal).


- [ ] Add the sprite to the game (it is safest to do this just after the object literal).
- [ ] Call the `gameController.startGame()` method (it is best to do this on the last line of your code).

This might be a good place to test your game so far.

Now let's add a scrolling effect.  Consult the GameController Help file and locate a command called `scrollBackground`.  See if you can figure out how to set the arguments in this method in order to make the `water.png` image scroll horizontally at 500 pixels per second.

-[ ] *When the `wall` object is first created*, call the `gameController.scrollBackground()` method with the appropriate arguments.  Remember the way we do this is to create a self-deleting `handleGameLoop` function in the `wall` object literal.  

Run your program.  You should see a wavy water effect at the bottom of the window.  Although this game is not a "scroller", a moving background is the main feature of such games. So now you know how to make your own scroller game.

## Platforms and Sliders

Just above the water we want to create starting and finishing platforms for the Princess to stand on, and two wooden platforms that slide back and forth in between them. The start and stop blocks are instances of a class we will call `Platform ` and the sliders will be instances of a `SlidingPlatform` class which is derived from the `Platform` class.

###Platforms

-[ ] Define a class called Platform, whose constructor function accepts three arguments (`x`, `y`, and `imageFile`) and does the following for each instantiation of the class:

* sets the name to 'A platform'
* sets `x` and `y` coordinates equal to the `x` and `y` arguments passed to the constructor
* sets the `imageFile` property equal to the `imageFile` argument passed to the constructor
* makes each instance immovable
* adds each sprite to the game 

-[ ] Create an instance of the `Platform` class at opposite sides of the display.  Call the one on the left `startPlatform` and the one on the right`finishPlatform`.  Use the `start.png` file for `startPlatform` and use the `finish.png` image file for `finishPlatform`.  The position of `startPlatform` should be (0, 400), and the position of `finishPlatform` should be two `gameController.gridSize`'s away from the right edge of the display.
-[ ] Preload the `start.png` and `startPlatform` image files.

Run your game and test.  You should see two blocks on either edge of the screen near the top of the water with finished corners pointing in.

### Sliders

Now we will create a child of the `Platform` parent class to make the sliders.

-[ ] Define a class called `SlidingPlatform` which `extends` the `Platform` class.  Its constructor function should accept three arguments (x, y, and angle) and do the following for each instantiation of the class:

* call the parent class constructor function with the `super` keyword, passing it the values of x, y, and `slider.png` for `x`, `y` and `imageFile`, respectively.
* set the name to 'A sliding platform'
* set its immovable property to `false`
* set the angle equal to the angle argument passed to the constructor
* set the speed equal to the `gameController.gridSize` property.  

This has the effect of moving the sliders 48 pixels per second in a direction defined by the `angle` property.

-[ ] Preload the `slider.png` file.


-[ ] Create two instances of the `SlidingPlatform` class so that they are a few `gridSizes` away from the start and finish platforms, and move opposite directions, like this:

```javascript
new SlidingPlatform(startPlatform.x + gameController.gridSize * 3, startPlatform.y + gameController.gridSize, 0);
new SlidingPlatform(finishPlatform.x - gameController.gridSize * 5, finishPlatform.y + gameController.gridSize, 180);
```

-[ ] Add the following class collision rule for the platforms:

```javascript
gameController.addClassCollisionRule([Platform],
    undefined, // no custom handler function; default behavior is bounce
    true // rule applies to collisions between objects of same class
);
```

The comments are there to explain the arguments of the method, but you can put the arguments on one line with no comments if you prefer.  As a test of your understanding of inheritance and polymorphism, see if you can predict which objects will be affected by this collision rule, and whether or not instances of `SlidingPlatform` will bounce off of instances of `Platform`.

Run your game and test your prediction.  You should see two wooden platforms floating just above the water and bouncing off of each other, and the start and finish blocks.  This is because instances of the `SlidingPlatform` class are *also* instances of the parent `Platform` class!

Now that the sliders are working, you will program the player character's navigation, including jumping onto the sliders.

## Player Navigation in a Platformer Game

In previous tutorials we have been handling player navigation by incrementing the value of x and y directly in response to arrow key inputs.  In this game, we will modify the `speed` and `angle` properties instead, and let the framework figure out the x and y values.  Also, to make the game a little more challenging, the player character will not stop when the direction key is released.
-[ ] Create a `princess` object with the following properties:
* a name
* an image sheet called `princessSheet.png`
* set her position to (x,y) = (`gameController.gridSize`, 300).  Yes, I know that is 100 pixels above the platform.  We are doing this on purpose to test her falling behavior later.
* set her `speed` to zero.
* create a custom variable called `movingSpeed` and set it equal to 100.

<u>Outside the object literal</u>:

-[ ] Add the `princess` sprite to the game.


-[ ] Define left and right walking animations for the `princess` object.
-[ ] Preload her image *sheet*.
-[ ] Add a collision rule for the princess and the wall, using the default bounce behavior, like this:

```javascript
gameController.addSpriteCollisionRule([princess, wall]);
```

<u>Within the `princess` object literal</u>:

-[ ] Define keyboard methods for the left and right arrow keys.  For now, these methods will do only these two things:

* Set her `angle` property to 180 or 0, respectively.  
* Set her `speed` property equal to `this.movingSpeed`.  

We will take care of the animations in a `handleGameLoop` method.

-[ ] Define a `handleGameLoop` function that tests to see if the object's angle property is equal to zero **AND** the object's speed is greater than zero.  If so, play the `right` animation.  Otherwise, if the object's angle property is equal to 180 **AND** the object's speed is greater than zero, play the`left` animation.

Run your game at test that you can move the princess left and right using the arrow keys, and that she bounces off the left edge of the wall.  

But she walks in midair!  She should fall any time that she is not on top of a platform:  either a slider or the two blocks on each edge of the moat.  To implement this, we will test to see if the princess is standing on an on object in the `Platform` class (which includes the  sliders, since they are children of the `Platform` class, as well as the start and finish blocks) during each game loop.  If she is *not*, we will increase her y position by 4 pixels every second to simulate gravity.  Later, we will want to know if the princess is falling for other reasons too, so we will find it useful to set a *flag* that tells us when this is happening.  A flag is a Boolean variable that signals a particular condition or status.  Let's define a flag for the condition of the princess falling.  

-[ ] Within the `princess` object literal, define a custom variable called `isFalling` and set its value to `false`. 

There is a sgc method called `getSpritesOverlapping()` which might help us determine whether or not a `platform` sprite is directly under the `princess` sprite.  Please read the entry in the game controller help file about this method.

- [ ] Add the following to the `princess` object's `handleGameLoop` method definition:

```javascript
// Check directly below princess for platforms.
let supportingPlatforms = gameController.getSpritesOverlapping(this.x, this.y + this.height, this.width, 1, Platform);
// Is there one, and is its *top* at or below the bottom of the princess?
if (supportingPlatforms.length > 0 && supportingPlatforms[0].y >= this.y + this.height) {
    this.isFalling = false;
    } else {
         this.y = this.y + 4; // simulate gravity
         this.isFalling = true;
    }
```

Some explanations are in order:

* We are using the `getSpritesOverlapping` method to get an array of sprites that overlap a 48-pixel-wide-by-1-pixel-deep rectangle whose upper left hand corner is at the *lower* left-hand corner of the `princess` sprite (remember the sprite's origin is in the upper left, so adding the sprites height to `y` places the rectangle right at her feet). 
* `supportingPlatforms` is a temporary variable we use to hold this sprite array.  If there is at least one sprite in the array, the *length* of the array (given by `supportingPlatforms.length`) will be greater than zero.
* To access the first element in the array, we use `supportingPlatforms[0]` since we start counting at zero. If there is something in the array it will be the start or finish platform, or one of the sliders.  So  `supportingPlatforms[0].y` marks the *top* of the overlapping platform sprite.
* If this is greater than or equal to the bottom of the princess sprite, she has landed on a platform and is therefore *not* falling.
* Otherwise, she is falling, and so we increase her `y` value by 4 pixels during the game loop.

Run the game and test that the princess falls onto the start block at the beginning of the game.  When you walk off the starting block. She should:

* land safely on a sliding platform, if one is there, and
* fall into the water if a sliding platform is not there.

You probably noticed that when the princess is on a slider, you must use the arrow keys to reverse direction and keep her from walking off the end. This is intentional; to improve the game challenge, there is no way to stop the princess once she begins moving.

With careful timing, you can walk from the first slider to the second. However, there is no way to walk *up* onto the finish block. As in most platformers, the player character needs to jump.

## Jumping

It wouldn't make sense for the `princess` to be able to jump unless she were standing on something, otherwise she could fly through the air by repeatedly jumping!  Now we can put that `isFalling` flag to good use.  

- [ ] Within the `princess` object literal, define a `handleSpacebar` method that tests to see if she is **not** falling. if so, move her up the screen 1.25 times her height, like this:

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

-[ ] Within the `princess` object literal, add a `handleBoundaryContact` method that ends the game with the message: 'Princess Ann has drowned.\n\nBetter luck next time.'

## Winning the game

Let's say there is a door on the right-hand platform that leads deeper into the castle.  If Princess Ann gets there, the player wins.

-[ ] Create a `door` object with the following properties:

* a name
* an image file called `door.png` 
* x : `gameController.displayWidth` - `gameController.gridSize`
* y : `finishPlatform.y` - 2 * `gameController.gridSize`
* make it immovable

-[ ] Add the sprite to the game


- [ ] Preload the image file

- [ ] Add a collision rule for the princess and the door that ends the game with a suitable message, like this:

```javascript
// The player wins when the princess reaches the exit.
gameController.addSpriteCollisionRule([princess, door], function() {
   gameController.endGame('Congratulations!\n\nPrincess Ann can now pursue the\nstranger deeper    into the castle!');
});
```

Run your game and test the winning and losing conditions. 

Although you now have a complete game, it seems a bit simple and uninteresting. We will now add some enemies to increase the challenge.

#Platform Princess, Part 2

## Along came a spider

The first enemies are the spiders. They will move up and down the screen on their webs. If they collide with the princess, she will be knocked off balance— possibly into the water.

-[ ] Define a class called `Spider` whose constructor function accepts two arguments (x, and y) and does the following for each instance of the class:

* gives it a name
* assigns it the image sheet file `spiderSheet.png`
* sets its x and y coordinates equal to the values passed to the constructor
* makes it immovable
* adds the sprite to the game
* defines an animation called 'creep' which uses all three frames in the image sheet
* sets its collision area to:

```javascript
this.collisionArea = {
   xOffset: gameController.gridSize / 2,
   width: 1, // minimize colliding with princess from above
   yOffset: gameController.gridSize / 3,
   height: gameController.gridSize / 3
};
```

Notice the collision area is confined to a thin bar centered on the spider and only takes up a third of the grid height (to match the non-transparent part of the image).

-[ ] Preload the image sheet file for the `Spider` class.


-[ ] In the `Spider` class definition, but outside the constructor method, define a self-deleting `handleGameLoop` method that plays the `creep` animation when the game first starts, and sets it to run continuously.  See the Game Controller help file if you forget how to set a `playAnimation` to repeat.


-[ ] Create an instance of the `Spider` class called `leftSpider` at location (200, 225).


-[ ] Create an instance of the `Spider` class called `rightSpider` at location (550, 200).

Run your game and test that the `spider` objects appear on the wall, "creeping" in place and minding their own business.  That is about to change.

We will program three parts to the spider's movement behavior, depending on its position relative to the main area of the princess's travel. This is the area just above the two sliders. It is the spiders' "attack zone". If a spider is far above the attack zone (like at the start), it will drop. If a spider is within the attack zone, it will quickly bounce up and down at random. If, by random, it moves below the attack zone, it will move back up.

Let's define the top of the attack zone as anything more than one `gridSize` higher than the princess (since an object there would be sitting just on top of her head) and the bottom of the attack zone as the y coordinate of the princess.

- [ ] At the place in your code after you delete the `handleGameLoop` method that starts the spider animation, define a new `handleGameLoop` method that tests to see if the spider is *above* the attack zone.  If so, move it down by setting the object's angle to 270, and its speed to 50.  

      Otherwise (else), check to see if it is *below* the attack zone.  If so, "jiggle" it up like this:

      ```javascript
      this.y = princess.y - Math.random() * 3 * gameController.gridSize;
      ```

Run your game and test to see if the spiders move to below the level of the princess's head if they are above it and jump back up when they get to the level of her feet, whether she is standing on the starting platforms or one of the sliders.

Now for the spiders' attack.  

What happens if we just use the default "bounce" behavior for collisions between `princess` and the spiders?  If she runs into a spider while moving only in the x direction (left or right), she will bounce back only in the x direction.  That seems ok.  Maybe she can defend herself if the spider is at arm level, but she can't pass *through* the spider.  However, if one of the spiders collides with the princess by landing on her head, the spider will have a y-component to its motion that will result in the princess bouncing *down*. During first game loop in which the princess goes more than one pixel below the top of a `platform` object, the `princess.isFalling` flag will be set, and since there are no more platforms below her she will fall off the bottom of the screen. That seems ok too -- if a giant spider lands on your head while you are balancing on a log in turbulent underground river, you might very well fall off the log.  So let's use the default bounce behavior for these collisions.

-[ ] Add the `leftSpider` and `rightSpider` sprites to your existing collision rule for the `princess` object.

Run your game and verify that the princess falls off the platform if the spider lands on her head, and simply bounces off the spider if she runs into it horizontally.

## Attack of the bats

The next type of enemy is a bat.   They will hover about their starting locations until they (randomly) decide to dive-bomb the princess.  

-[ ] Using the first three steps of "Along Came a Spider" as a guide, define a class called `Bat` that uses the `batSheet.png` image file, and plays an animation called `flap` that uses both frames in the image sheet *when the game first starts.*  For the collision area, use a rectangle that takes up the middle third of the grid (approximating the non-transparent portion of the image). 


-[ ] Create an instance of the`Bat`class called `leftBat` at location (200, 100).

-[ ] Create an instance of the`Bat`class called `rightBat`at location (500, 75).

Run your game and verify that the bats appear and flap around in place. 

We will program two parts to the bats' behavior.  One where they fly about slowly in random (diagonal) directions, and one in attack (dive-bomb) mode.

Let's define the attack mode first.

-[ ] In the `Bat` class definition, but outside the constructor method, define a method called `attack()` that does two things:

* changes the speed of the bat to 300
* uses the `aimFor` method to set the angle of the bat to point to the current location of the princess, like this:

```javascript
this.aimFor(princess.x, princess.y);
```

(see `Sprite` class properties in the Game Controller documentation for more information on this method)

We want the bat to fly around randomly most of the time, but one time in 1000 game loops, the bat should fly directly toward the princess at high speed.

-[ ] At the place in your code after you delete the `handleGameLoop` method that starts the bat animation, define a new `handleGameLoop` method that tests to see if `Math.random()` is less than 0.001.  If so, call the bat's attack method.  If the bat is not attacking ... hover.  We will do more with this after we test what we have so far.  For now you can create a *stub* here by adding a comment after the the `if` statement closing bracket that says `// if bat is not attacking: hover`. 
-[ ] Add the `leftBat` and `rightBat` sprites to the existing collision rule for the `princess` object.

Run the game and see if the bats randomly dive bomb the princess and knock her off the platforms. 

## Bat hovering

Looking back at the stub in your `handleGameLoop` method, we see that we wanted to test to see if the bat is not attacking.  There are few ways we might do that.  For example, we might check its current location (x and y coordinates) and see if it is different from the starting location.  As our code is currently written, that would necessarily mean the bat had started his bombing run.  But if we start fluttering about (hovering), the position will change even though the bat is not attacking.  So we need something else.  How about speed?  If attacking, the bat will be at speed 300, and as long as we pick a different speed for hovering, we could test the bat's speed variable.  In order to make our code more readable, let's define two custom variables for these speeds.

- [ ] Inside the constructor method of the `Bat` class, define a custom variable called `normalSpeed` and set its value to 20.  While we are at it, lets set the object's `speed` to this value too.  One way to do this is with a *chained assignment* statement that looks like this:

```javascript
this.speed = this.normalSpeed = 20;
```

This works because the right-hand side of an assignment must be evaluated before the assignment can happen.  Read [this](https://en.wikipedia.org/wiki/Operator_associativity#Right-associativity_of_assignment_operators) paragraph in Wikipedia for more information about operator associativity.

-[ ] Inside the constructor method of the `Bat` class, define a custom variable called `attackSpeed` and set its value to 300.
-[ ] In the `attack` method definition, replace `this.speed = 300;` with `this.speed = this.attackSpeed;`


Now we are ready to write an `if` statement that tests to see if the bat is attacking.  But first let's talk about the hovering behavior we would like to program for the bats:

* give each bat a random diagonal direction when it is created


* every 5 seconds, increase the `angle` property of the bat either by 90 degrees or 180 degrees.  Whether it changes by 90 or 180 degrees depends on the flip of a coin:  50% of the time it does one rather than the other.

The first bullet sets the starting angle. We will use `Math.round()` on the `Math.random()` method to get a random *integer* between (and including) 0 and 3, then multiply that integer by 90 to pick out one of 4 directions.  See [mozilla](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/round) for an explanation of `Math.round()`.  This is a useful trick that we will use again.

-[ ] In the constructor function method for the `Bat` class, set the `angle` property to 

```javascript
this.angle = 45 + (Math.round(Math.random() * 3) * 90);
```

To program the second bullet, you might want to review the "spell cast timer" we used to limit Marcus's spell casting in `wizardDuel`.  The relevant block of code might have looked like this:

```javascript
spellCastTime: 0,
handleSpacebar: function() {
    let now = gameController.getTime();
    if (now - this.spellCastTime >= 2) {
        this.spellCastTime = now;
        // Cast a spell to the right
        ...
    }
}
```

Instead of `spellCastTime` use a descriptive name like `angleTimer` for example.  Your `angleTimer` code will look similar to the code snippet above, except the timer duration will be different, and the syntax is different because we are in a class definition rather than an object literal.  

-[ ] Using the custom variables you just defined, test to see if the bat is attacking.  If so, start a 5 second timer to change the bat's direction by 90 degrees 50% of the time, and 180 degrees the other 50% of the time.

You might run into a problem with your `if` statement if you used `(this.speed === this.normalSpeed)` for your conditional expression.  You can find the cause of the problem by typing `leftBat.speed` in the console window of your game.  You should have seen 20.000000000000004 whereas `leftBat.normalSpeed` returns 20.  We will learn more about why this happens next semester, but if you want a sneak preview, read this short article on [Avoiding Problems with Decimal Math in JavaScript](http://adripofjavascript.com/blog/drips/avoiding-problems-with-decimal-math-in-javascript.html).  For now you can just round `this.speed` and your conditional statement will return true when the speed is *about* 20.

## Keeping the bats in play

Lastly we need to figure out what to do about bats hovering or dive-bombing out of the display area.  Let's say if a bat reaches the bottom or sides of the screen, we just send it back to its starting point, perhaps representing the creation of a new bat to take its place.  But if it hovers to the top of the screen, we will just keep it from leaving the display area by resetting its y coordinate to zero.

First we need to keep track of the bat's starting position by creating two custom variables.

-[ ] In the constructor of the `Bat` class definition, create a custom variable called `startX` and set its value equal to the x value passed to the constructor.  You can do this by modifying your existing `this.x` assignment statement with a chained assignment statement like this if you like:

```javascript
this.x = this.startX = x;
```

-[ ] Do the same for the starting y coordinate with a variable called `startY`.

Now that we have stored the starting coordinates for each instance of the `Bat` class, we can define our `handleBoundaryContact` method.

-[ ] In the `Bat` class definition, define a `handleBoundaryContact()` method that does the following:

* if the bat's `y` coordinate is less than zero, set it equal to zero
* otherwise, reset the bat's position to its starting position, its `speed` to `this.normalSpeed`, and its `angle` to 225 degrees.

  Run your game and verify that:

* the bats hover about, changing directions every five seconds, either by 90 or 180 seconds
* the bats return to their starting positions if they leave the left or right or bottom of the display
* the bats hug the top of the display if they touch it (until another direction change happens)

##Wrapping Up

Congratulations!  You have completed the Platform Princess tutorial.  Upload your finished .js file to Sakai.  

-[ ] Read [The Town of Bool: Part 1 of Ann's encounter with the Booleans](http://computationaltales.blogspot.com/2011/05/town-of-bool.html)
-[ ] Read [The Marvelous IF-ELSE Life of the King's Turtle](http://computationaltales.blogspot.com/2011/09/marvelous-if-else-life-of-kings-turtle.html)

Use question set 6 (Platform Princess) to review and evaluate your mastery of new concepts.

-[ ] Using the png files in Sakai [Platform images folder](https://sakai.lampschools.org/portal/site/8327cf3f-758a-4ce1-bb40-5a816869e7bc/tool/40fd529a-cd2b-46fd-bed3-501a3f39e116?panel=Main#)*, create a Halloween-themed platform game similar to platformPrincess with the following minimum requirements:

* one or more sliding platforms move vertically in addition to horizontally
* the background and platforms conform to the tile set in the Platform images folder
* the object of the game is to push a crate object to the right edge of the screen (or door)

You may remove the bat and spider objects and re-use the wooden platforms if you wish.  You can resize and otherwise molest the png files with a free program such as [IrfanView](http://www.irfanview.com/main_download_engl.htm).  

-[ ] Upload your finished project js file to the Sakai assignments tab.

\* The platform graphics were provided courtesy of [Game Art 2D](http://www.gameart2d.com/freebies.html).

![platformProject](/images/platformProject.PNG)