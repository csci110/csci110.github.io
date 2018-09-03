# Wizards' Duel, Part 1

First let's start with a review of what you learned in the last tutorial.  As a test of your understanding and syntax memorization, try to complete these steps without looking back at your StrangerHunt code, unless you get stuck.  The code will be very similar to what you already did in the previous game.

In `wizardsDuel.js`, write some JavaScript code to accomplish the following things:

- [ ] Import `game` and `Sprite` modules from "../sgc/sgc.js"
- [ ] Set the background to "floor.png" (*HINT: use the `game.setBackground()` method*)
- [ ] Declare a class called `PlayerWizard` that is a *child* of the `Sprite` class.
- [ ] In the constructor method of the `PlayerWizard` class:
  - [ ] Call the parent's class constructor (*HINT: Clark Kent*)
  - [ ] Set the `name` property to "Marcus the Wizard"
  - [ ] Call the `setImage()` function with "marcusSheet.png" as an argument
  - [ ] Set the `width` and `height` properties to 48
  - [ ] Set the `x` position to `this.width` and `y` to `this.y`
- [ ] Create an object of the `PlayerWizard` class called `marcus`.
- [ ] Run the game.  

You should see Marcus in the upper left hand corner of the floor, standing there quietly.  If you did all that without looking back, congratulations!  If not, please review your syntax flashcards, or make new ones to fill in the blanks.  Remember, we are learning a new *language* so we have to memorize some vocabulary and grammar rules.

______________________________

In the last tutorial, we used static image files to portray the characters.  Now we will use a series of images in *image sheets* to animate the characters in the story.  These are typically png files with 12 frames in them, three each for left, right, front and back facings of the sprite.  

- [ ] From your c9 workspace, double-click to open `marcusSheet.png`

![MarcusSheet](/images/MarcusSheet.PNG)

Notice that the frame corresponding to the single image of Marcus we used in the last tutorial is the 8th one from the left.  On either side is Marcus with one or the other foot forward.  Playing these three frames in succession gives the illusion of Marcus walking down the screen. We will use the `defineAnimation` method to tell the game engine which frames to play for each facing.  

Search for (CTRL+F) `defineAnimation` in the gameController help accessed from the README file in your tutorials folder. 

![defineAnimation](/images/defineAnimation.PNG)

Let's define an animation for the "down" direction.

- [ ]  Add this line of code to the constructor of the  `PlayerWizard` class definition:

```javascript
this.defineAnimation("down", 6, 8);
```

"The zero-based sheet index" means we start counting from 0, not 1.  So the 7th frame from the left is actually frame number 6 because the first frame is frame 0!  This is the convention for indexing arrays in JavaScript and most other programming languages.

And to display this animation for an object in the class we would use (but don't add this yet):

```javascript
this.playAnimation("down");
```



## Player Character Navigation

### The Down arrow key

Say we want the down arrow key to move Marcus toward the bottom of the screen. His front should be showing, and walking animation should be displayed.

- [ ] In the `PlayerWizard` constructor, create a custom property called `speedWhenWalking` and set its value to 100, like this:

```javascript
this.speedWhenWalking = 100;
```

This will be used to define how rapidly a `PlayerWizard` object moves up or down the screen in response to our input (100 pixels per second).  You won't find `speedWhenWalking` listed in the properties of the `Sprite` class in the sgc documentation.  `speedWhenWalking`is a *custom variable* - it is a property of any object in the `PlayerWizard` class we just created on the fly for our own sinister purposes.  

- [ ] Look for a `Sprite` method in the help file that will execute a function when the down arrow key is pressed. 

Did you find `handleDownArrowKey`?  Let's use it to move Marcus down the screen, and display the corresponding frames.  

- [ ] Add this function definition (outside of the constructor!) to the class definition for `PlayerWizard`:

```javascript
handleDownArrowKey() {
        this.playAnimation("down");
        this.speed = this.speedWhenWalking;
        this.angle = 270;
}
```

- [ ] Run the game. Test that the *down* arrow key causes Marcus to move toward the bottom of the screen, with his front showing and walking animation playing. 

- [ ] Using the previous steps as a guide, add a function definition to handle an *up* arrow key press, with the appropriate animation and direction. 

- [ ] Run the game.  Test that the up and down arrow keys cause Marcus to move toward the appropriate edge of the screen, with the correct side showing and walking animation playing.

Unfortunately, you can walk Marcus completely off of the window, and when the arrow key is released, he still moves even though his animation stops playing.  We should do something about this ... in part 2.

# Wizard's Duel, Part 2

To keep Marcus inside the display area of the window, we need to define a method that executes every game loop pass, and adjusts Marcus's `y` coordinate if it exceeds the display height (minus the sprite width, so it doesn't hang off the edge of the window) or goes negative (which would put the top of the sprite above the top of the window) .  So far we have been using `Sprite` methods like `handleMouseClick()`, `handleDownArrowKey()` etc. to define functions to be executed in response to an input from the user.  Perhaps there is another method that will execute every game loop regardless of user input.

- [ ] Check the sgc documentation for an appropriate method in the `Sprite` class. 

Did you find `handleGameLoop`?  The game engine is built around a "game loop."   This is a repeating series of steps that cycle endlessly as long as the game is running.  When you want something to happen continuously, you can program it in the function definition for the `handleGameLoop` method.  Any commands that you place there will be performed on every pass through the game loop.

There are many ways to confine the `y` coordinate to the display area, but one of the more elegant involves  `Math` methods called `Math.max()` and `Math.min()`.  These, and other methods in JavaScript's `Math` object are explained in detail [here](https://www.w3schools.com/js/js_math.asp).  For example, if we want `y` to stay above zero and below 552 (600 minus the sprite width) we could use the following lines in a `handleGameLoop` method definition:

```javascript
this.y = Math.max(0, this.y);
this.y = Math.min(552, this.y);
```

This is an example of a code segment that should include comments, since it is not immediately obvious what it does. The first line picks the *greatest* of 0, or the current value of `y` and assigns it to `y`.  The second takes the *lowest* of 552, or the current value of `y` and assigns it to `y`.  In other words, if y gets to -1 as a result of being decremented in a call to `handleUpArrowKey`, the first assignment statement will set it to 0. If `y` gets to 553 as a result of being incremented in a call to `handleDownArrowKey`, the second assignment will set it back to 552.  The net effect is to keep Marcus in the display area.  Unfortunately, writing the code in this way does not make it very flexible, should you decide to change the display dimensions or sprite size.

- [ ] Inside the `PlayerWizard` class definition (but outside the constructor), define a `handleGameLoop` method that adjusts y coordinates as shown above, except modify the second line to use the`game` properties of `displayHeight` and `gridSize`.  Don't forget to add comments like this to your function definition:

```javascript
// Keep Marcus in the display area 
```

We also want Marcus to stop if the arrow keys are not pressed, so let's set his speed to zero every game loop.

- [ ] Inside the `handleGameLoop()` method definition, add a statement that sets the speed to zero.

- [ ] Run your game and test that Marcus stays in the display area.

Now let's give Marcus the ability to cast spells (in the form of projectiles).  To do this we will be creating "spell" objects -- a lot of them -- that all look the same and do the same sorts of things.  

## A spell class

- [ ] Declare a class called `Spell` which is a child of the `Sprite` class.

- [ ] In the constructor method of the `Spell` class:

* Call the parent's class constructor
* Set  `speed` equal to 200
* Set `height` and `width` to 48
* Define an animation called "magic" that uses frames 0 through 7 (*HINT: use `this.defineAnimation()`*)
* Play the "magic" animation as before but this time we will use the second (optional) parameter to repeat the animation if *true*, or to play it once if *false* like this:

```
this.playAnimation("magic", true);
```

## Casting spells

We are going to use the spacebar to cast spells for Marcus.

- [ ] In the `PlayerWizard` class definition (outside the constructor), define a `handleSpaceBar()` method which does the following:
- Create an object called `spell` (lower case s) from the `Spell` class (upper case s).
- Set the `spell` object's position to the position of *this* object in the `PlayerWizard` class like this:

```
spell.x = this.x;  // this sets the position of the spell object equal to
spell.x = this.y;  // the position of any object created from the PlayerWizard class
```

* Set the `spell` object's name to "A spell cast by Marcus"
* Call the `setImage()` method using "marcusSpellSheet.png" as an argument
* Set the `spell` object's angle to zero.	

- [ ] Run the game and test that Marcus can cast spells by pressing the spacebar, and that the spell is created at Marcus' current location, and runs off the screen to the right while playing the appropriate animation.

I know it's not perfect.  We will fix some of the problems you see very soon.

## A word about style

You might have noticed that we are using **camelCase** for identifier names (objects, variables and functions), and **PascalCase** for classes.  PascalCase is considered a subset of camelCase, actually (it is "upper case" camelCase, so to speak). This follows generally accepted style guidelines for coding, which makes it easier to read and maintain code that someone else has written.  For more about JavaScript style and coding conventions, refer to the [JS Style Guide](https://www.w3schools.com/js/js_conventions.asp).

## Deleting game objects to conserve resources

When the `spell` objects leave the screen, you have no further use for them. But they do not simply cease to exist. The game engine is still tracking their position and keeping track of all their associated data.

This is a waste of system resources, and this kind of issue can lead to poor game performance. It is time to clean up after yourself!

- [ ] Add the following method to the class definition for `Spell` *outside* of the constructor definition.  

```javascript
handleBoundaryContact() {
    // Delete spell when it leaves display area
    game.removeSprite(this);
}
```

The`handleBoundaryContact()` method will get called every time `x `or `y`  of the associated object are outside the bounds of the display area. In this case, your programming will cause the `spell` objects to be deleted from the game when they leave the room.

## Graphical fine tuning

Now you will improve the visuals of Marcus's spell casting.  Marcus will face toward the right edge of the screen when a spell is cast, so he is looking in the direction that it goes, and the spell sprite will be created in front of him, rather than on top of him as it is now.

- [ ] Inside the constructor of the `PlayerWizard` class, define an animation called 'right' that uses frames 3 through 5 of `marcusSheet.png`.  

- [ ] Inside the `PlayerWizard`  `handleSpacebar` function definition, add a line to play Marcus's "right" animation.
- [ ] Modify the `x` position of the `spell` object so that it starts a distance `this.width` to the right of Marcus.
- [ ] Run your game and verify that Marcus turns to the right and fires a spell in front of him when the space bar is pressed.


## Wrapping Up

Use Question Set 3 to review and evaluate your mastery of new concepts.

# Wizard's Duel Part 3

## A Non-Player Character (NPC)

Marcus eventually gets tired at shooting his spells into thin air, so we should give him a target that can shoot back.

- [ ] Define a `NonPlayerWizard` class that is child of the `Sprite` class.

- [ ] In the constructor method for this class:
- Call the parent class constructor
- Set the name to "The mysterious stranger"
- Call the `setImage()` function with "strangerSheet.png" as the argument
- Set the `width` and `height` to 48
- Set the `x` coordinate to `game.displayWidth - 2 * this.width` to put the NPC near the right edge of the display
- Set the `y` coordinate to `this.height` to put the NPC near the top of the screen
- Set the `angle` to 270
- Set the `speed` to 150
- Define "up," "down," and "left" animations 

![strangerSheet](/images/strangerSheet.png)

	Note: "up" and "down" can use the same frames that we used for the `marcus` object (0 through 2 for 				"up," and 6 through 8 for "down"), but can you tell what frames we should use for "left"?

* Play the "down" animation

- [ ] Create an object of the `NonPlayerWizard` class called `stranger`.

- [ ] Run your game and verify that the Stranger appears at the upper right hand corner of the screen and drifts down and off the screen with no walking animation.

To add a walking animation for the Stranger, and to keep him from walking off the screen, we will have to define a `handleGameLoop` function for him, similar to what we did for Marcus when we wanted to keep Marcus in the display area.  Unfortunately if we just follow the same recipe with `Math.max()` and `Math.min()` functions, the Stranger will just wander to the bottom of the screen and stay there, making him a sitting duck for Marcus's wizardry.  To make things more challenging, we will have the Stranger change directions *if* he gets to the edge of the display area.  In order to do this we will use one of the most powerful weapons in the programmer's arsenal:  **conditional actions**.

## Conditional Actions

- [ ] Read [Learning IF-ELSE the Hard Way](http://computationaltales.blogspot.com/2011/05/learning-if-else-hard-way.html)

Here is the syntax and usage for `if` statements in JavaScript:

```javascript
if (condition) {
    block of code to be executed if condition is true
} 
```

- [ ] Define a `handleGameLoop()` method for the  `NonPlayerWizard` class that contains this code:

```javascript
if (this.y <= 0) {
      // Upward motion has reached top, so turn down
      this.y = 0;
      this.angle = 270;
      this.playAnimation("down");
}
if (this.y >= game.displayHeight - this.height) {
     // Downward motion has reached bottom, so turn up
     this.y = game.displayHeight - this.height;
     this.angle = 90;
     this.playAnimation("up");
}
```

`if` the Stranger is going off the top of the screen, then set his y position back to zero, and change his direction to 270 degrees, and play the 'down' animation.

if the Stranger is going off the bottom of the screen, then set his position back to where the whole sprite image is showing, and change his angle to 90 degrees, and play the 'up' animation.

## Comparison Operators

Comparison operators are used within conditional statements to determine equality or difference between variables or values.  We used the greater `>=` and less than `<+` operators in the previous example to see if the Stranger was out of bounds.  We need a few more.

- [ ] Please read [The Incident at the North Gate (or Why = Is Not ==)](http://computationaltales.blogspot.com/2011/06/incident-at-north-gate-or-why-is-not.html).   I recommend saying "*is equal*" to yourself when you see `==` or `===` and "gets" when you see `=` .
- [ ] Refer to [this](https://www.w3schools.com/js/js_comparisons.asp) article on operators and pay particular attention to the difference between `=`, ` ==`, and `===`.  We will be using `===` rather than `==` whenever possible.  Also now might be a good time to learn the symbols for the logical operators AND (`&&`), OR (`||`) and NOT(`!`). 

- [ ] Run the game and verify that the Stranger changes direction and (temporarily) changes his facing and associated animation when he reaches the edge of the display.

Why temporarily?  Because the condition that triggers play animation only happens once each time the Stranger reaches an edge!  We need to add a statement that keeps the animation going at other times.  

## Animation Endings

There is a `Sprite` method called `handleAnimationEnd()` that triggers whenever an animation defined for the associated sprite completes all of its frames.  We will use this in conjunction with some conditional statements to play the correct animation for the mysterious stranger.

- [ ] Add a `handleAnimationEnd()` method definition to the `NonPlayerWizard` class that contains the following code:

```javascript
if (condition1) {
    this.playAnimation("up");
}
if (condition2) {
    this.playAnimation("down");
}
```

- [ ] Replace "condition1" in the code above with a conditional statement that tests to see if `this.angle` *is equal* to 90, and replace "condition2" with a conditional statement that tests to see if `this.angle` *is equal* to 270.  *HINT:  Use the `===` operator to compare `this.angle` to 90 or 270.*

- [ ] Run your game again and verify that the Stranger animates correctly in both directions.

## Wrapping Up

We are not yet finished with the Wizard Duel tutorial (one part remains).  If you are reading this you are farther along than I expected.  Please use the extra time to start on your Shooter project (below).

- [ ] Read [Functions and Sailing](http://computationaltales.blogspot.com/2011/04/functions-and-sailing.html)

- [ ] Use question set 4 to review and evaluate your mastery of new concepts.

# Shooter project

- [ ] Create or download a "shooter" sprite and background.  This can be a spaceship, a person, a dripping faucet, evil cucumber, or whatever.  *You can even re-use the sprite and background from your first project.*
- [ ] Create or download one or more target sprites.  This can be a person, a soccer goal, an asteroid, space alien, computer science teacher, etc.
- [ ] Create or download projectile sprites.  They can be dots, bananas, rockets, etc. or you can re-use the sprites from Wizard's Duel if you wish.
- [ ] Create a shooter game as similar as practical to Wizard's Duel but different in one or more of the following ways:

- move horizontally and shoot vertically
- target and/or shooter move(s) in two dimensions
- collision handler does something besides blowing up such as increasing speed, multiplying, increasing shooting speed, increasing a score, or whatever.

- [ ] Consider uploading your own, or any open-source sprites you find to the "Sprites to Share" folder of the Resources tab in Sakai. 

- [ ] **Site your sources for pixel art in the comments of your code, or explicitly state which sprites are your original work.**  I expect you will be making *your own* unique modifications to your `wizardsDuel.js` script to complete this project.

  Grading is based on level of effort in any of the above categories.  For example if you wish to focus on sprite creation instead of programming complexity, please see [this tutorial](http://makegames.tumblr.com/post/42648699708/pixel-art-tutorial) on pixel art.
```

```