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
this.y = Math.max(5, this.y);
this.y = Math.min(552, this.y);
```

This is an example of a code segment that should include comments, since it is not immediately obvious what it does. The first line picks the *greatest* of 5, or the current value of `y` and assigns it to `y`.  The second takes the *lowest* of 552, or the current value of `y` and assigns it to `y`.  In other words, if y gets to -1 as a result of being decremented in a call to `handleUpArrowKey`, the first assignment statement will set it to 0. If `y` gets to 553 as a result of being incremented in a call to `handleDownArrowKey`, the second assignment will set it back to 552.  The net effect is to keep Marcus in the display area.  Unfortunately, writing the code in this way does not make it very flexible, should you decide to change the display dimensions or sprite size.

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
spell.y = this.y;  // the position of any object created from the PlayerWizard class
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

# Wizard's Duel Part 4

## Hitting a Target

Now you will make the Stranger vulnerable to Marcus's spells. When he is hit:

1. An explosion effect will display.
2. The stranger object will be destroyed. 
3. A congratulations message will be shown.

In the previous game, we used the default behavior for collisions between the objects in the `Sprite` class; they just bounced when they collided.  Now we'd like to define some different behavior for the collision between a `spell` object and the Stranger.  The `Sprite` method we will use for this is `handleCollision(otherSprite)`.  The method is called each game loop where *this* sprite is touching another sprite.  It *overrides* the base implementation, which is that the sprites just bounce off each other.  The `otherSprite` variable holds (is set equal to) the other sprite involved in the collision.  Instead of bouncing off the `stranger` sprite, we want to create a fireball, and delete both objects involved in the collision.

![handleCollision](.\images\handleCollision.PNG)

## Create a fireball at the location where the Stranger used to be and delete the colliding sprites

First let's create a Fireball class to make some "explosion effect" objects.  Up to now, we have not passed any arguments to the `constructor()` method.  This time, we will pass the `otherSprite` object to it, so we set the position of the fireball to the position of the sprite we just hit.

- [ ] Define a `Fireball` class which is a child of the `Sprite` class and pass an object to the constructor which we will refer to as `deadSprite` within the class definition, like this:

```
class Fireball extends Sprite {
    constructor(deadSprite) {
        super();
        this.x = deadSprite.x;
        this.y = deadSprite.y;
        ...
```

- [ ] Finish the constructor definition started above by:
  * Setting the image of each object in the class to `fireballSheet.png`.
  * Setting the name of each object in the class to "A ball of fire."
  * Remove the `deadSprite` object by using `game.removeSprite(deadSprite)`;
  * Defining an animation called "explode" that uses all 16 frames of the image sheet.
  * Playing the "explode" animation.

Where is a good place to define a `handleCollision(otherSprite)` method?  We could put it in the class definition for `NonPlayerWizard`, but if we did that, we would also have to put one in the `PlayerWizard` class eventually since the stranger will no doubt be casting some spells of his own.  Better to have it one place: the `Spell` class, since spells are the things that will be colliding with both types of wizards, and maybe even other spells.

- [ ] Add and `handleCollision(otherSprite)` method to the `Spell` class definition that removes *this* spell sprite, and creates a new object in the `Fireball` class, while passing the `otherSprite` object to the constructor of the `Fireball` class like this:

```
handleCollision(otherSprite) {
     game.removeSprite(this);
     new Fireball(otherSprite);
     return false;
}
```

Here is how this works.  Let's say a `spell` object collides with the `stranger` object, triggering the `handleCollision()` method.  Now the variable `otherSprite` refers to the object  `stranger`, and `this` refers to the `spell` object that hit him.   The first line removes the `spell` object.  The second line calls the constructor of the `Fireball` class, passing it the `stranger` object.  Inside the `Fireball` constructor, any reference to `deadSprite` is a reference to the `stranger` object.  So when the `Fireball` constructor removes `deadSprite` it is actually removing the `stranger` object from the game.  If that doesn't make sense, please ask one of the instructors to explain it better; it's a tricky concept to grasp.

The last line *returns* false.  When a method returns something it means the method has a *value* that the caller can use to do stuff.  In this case, it signals the `Sprite` class that a bounce is not desired (see the `handleCollision()` documentation above). 

- [ ] Run your program and verify that you get an explosion effect when you cast a spell

One problem is we are getting a collision right away because Marcus is generating multiple spells with each press of the spacebar and they are running into each other.  Let's fix that.

- [ ] Inside the collision handler for the `Spell` class, enclose the first two lines in an `if` statement that checks to make sure that the same spell sprites don't destroy each other, like this:

```
 // Compare images so Stranger's spells don't destroy each other.
 if (this.getImage() !== otherSprite.getImage()) {
      game.removeSprite(this);
      new Fireball(otherSprite);
 }
```

Here we are using another method of the `Sprite` class called `getImage()` that returns the image used to display the sprite.

- [ ] Run the game and try to shoot the mysterious stranger (it shouldn't be too hard with Marcus's spell machine-gun).  Verify that the fireball is created at the location of the collision, and that the `stranger` sprite disappears. 

This is ok, but clearly we need to remove the fireball after the animation completes.  We will take care of that in the next step, along with a game winning (or losing) message.

### End the game with a congratulatory message

We will accomplish this by defining an `handleAnimationEnd()` method for objects in the `Fireball` class, like we did before in the `NonPlayerWizard` class.  

```
handleAnimationEnd() {
        game.removeSprite(this);
        if (!game.isActiveSprite(stranger)) {
            game.end("Congratulations!\n\nMarcus has defeated the mysterious"
            + "\nstranger in the dark cloak!");
        }
}
```

Here we are using a method of the `Game` class called `isActiveSprite()` that returns true if the argument is active in this game.  For example, `game.isActiveSprite(stranger)` will return true unless the `stranger` object has been removed from the game using `game.removeSprite()`.  Remember that the `!` operator *negates*, or gives the opposite of what comes after it, so `!game.isActiveSprite(stranger)` returns true if `stranger` has been removed.

This might also be a good time to mention the following suggestion from the [JavaScript style guide](https://www.w3schools.com/js/js_conventions.asp): 

*"For readability, avoid lines longer than 80 characters.*
*If a JavaScript statement does not fit on one line,* 
*the best place to break it,* 
*is after an operator or a comma."*

In the code snippet above, the game ending message string was longer than 80 characters, so we used the `+` operator to concatenate the two parts of the message.  This is our preferred way to break up long strings.  Notice that we don't need a continuation character for long lines of *code* that don't include strings.  JavaScript doesn't care about line endings; it uses semicolons to tell when you have completed a program statement.

You can tell how many characters are in your line by placing the cursor at the end of your line and looking at the grey text in the lower right corner of your development window.  The first set of numbers is line:character#.  (Line 5, character 73 in this example)

![LineAndChar](/images/LineAndChar.PNG)

There is also a faint white line in the c9 IDE at the 80-character point.

- [ ] Run your game and test that the correct message is displayed when you hit the `stranger` and the explosion animation ends.

____________

We are improving with every iteration but every iteration uncovers new refinements we will want to implement.

The images for Marcus's spell objects are mostly transparent, and only the central rectangle has visible contents.  We want to register a hit on the Stranger only when the center portion of the spell "projectile" collides with the Stranger, not the whole 48x48 square. One way to approximate this is by calculating the vertical offset between the sprites, and dividing by 2.  We will **nest** this if statement inside your previous `if` statement as follows.

- [ ] Inside the `Spell` class definition, replace your `handleCollision(otherSprite)` definition with this:

```
 // Compare images so Stranger's spells don't destroy each other.
 if (this.getImage() !== otherSprite.getImage()) {
      // Adjust mostly blank spell image to vertical center.
      let verticalOffset = Math.abs(this.y - otherSprite.y);
      if (verticalOffset < this.height / 2) {
          game.removeSprite(this);
          new Fireball(otherSprite);
      }
  }
  return false;
```

*Nesting* `if` statements ensures that the inner-most block (starting with `let verticalOffset...` ) will only execute if both the outer *and* inner conditions are true.  We could do this with a single `if` statement using the `&&` operator, which would make the code more compact but less computationally efficient.  Can you see why?[^*]  

Now the `spell` sprite will have to overlap the *center* of the `otherSprite` in order for it to count as a "hit."

## Targeting the Player Character

The Stranger strikes back! 

We want the Stranger to cast spells, and for Marcus to be vulnerable to that spell.  We were careful not to make our `Spell` class definition so narrow that it only applies spells cast by the `PlayerWizard` class objects, or to collisions with `NonPlayerWizard` class objects.  This is one of the advantages of object-oriented programming.  Each object is meant to "stand alone, " encapsulated with all its methods and properties, able to be used by any class.  But how will the Stranger cast his spell?  As a non-player character, there is no one pressing a key to control him. You must automate his behavior.  In the `PlayerWizard` class, we defined a `handleSpacebar()` method that created a `spell` object every time we hit the spacebar.  Let's add similar code to the `handleGameLoop()` definition in the `stranger` object.  Remember any code we place here will execute every game loop.

- [ ] Using the `PlayerWizard`'s '`handleSpacebar()` method definition as a guide, add some code to the `NonPlayerWizard`'s `handleGameLoop()` method that casts a spell ... every game loop.  You can use the same object name (`spell`) and properties -- just modify: 
  * the `name`, 
  * the `x` and `y`  coordinates as appropriate, 
  * the image argument used in the `setImage` method, 
  * the `angle` 
  * and the animation argument in the `playAnimation()` method.

- [ ] Inside the `handleAnimationEnd()` method of the `Fireball` class definition, add a second `if` statement that checks to see if the the `marcus` sprite has been removed from the game, and prints the following message at the end of the game if it has:

  ```
  "Marcus is defeated by the mysterious\nstranger in the dark cloak!\n\nBetter luck next time."
  ```


*Remember to use the `+` operator to break up the string if it makes your line longer than 80 characters.* 


- [ ] Run your game and test that the stranger sends out a continuous stream of spells towards Marcus, and that the appropriate message displays after the fireball animation, which appears in the correct places. 

Note that the spells destroy each other when they collide.  Why does this happen?

- [ ] See if you can beat the game.  It is possible, but it is too easy to reproduce once you see the trick.

## Random Behavior

At this point, the Stranger should be continuously casting spells at Marcus. Once every game loop.

Clearly that is too much.  It's better if the Stranger's spell timing is less frequent and unpredictable. Random, even.

And that's the solution. Every time step, you will generate a random number. Depending on the number, you may or may not have the Stranger cast a spell.  Let's try having the Stranger cast a spell with 1% probability.

We have already used `Math.random()` to teleport our characters to random spots in the room in the previous tutorial, so perhaps you remember that this method generates a random number between 0 and 1 (including zero but not 1).  Since all numbers are equally probable, there is a 100% probability that the number will be less than 1, a 10% probability that the number will be less than 0.1, and a 1% probability that the number will be less than 0.01.

- [ ] Inside the `NonPlayerWizard`'s  `handleGameLoop()` definition, enclose the code your wrote in the previous section within an `if` statement that tests to see if `Math.random()` is less than 0.01.  *If* it is, then cast the spell as above.  In other words, enclose the code you wrote in the previous step inside of an `if` statement like this (still inside the `handleGameLoop()` method):

```javascript
if (*** write your conditional expression here ***) {
   // Create a spell object 48 pixels to the left of this object            
   // Make it go left, give it a name and an image
   // Play the left animation
}
```

except with your code in place of, or in addition to the above comments.

- [ ] Run your game and test that the Stranger casts spells at Marcus at a suitably challenging frequency.  If 1% is not suitable, change your conditional expression to taste.

## Limiting Marcus's Spell Casting

Marcus can cast spells as quickly as the player taps (or holds) the spacebar. It is possible for Marcus to cast a solid line of spells that extend across the entire screen. That makes the game too easy to be interesting.  Let's say we want Marcus to cast a spell only once every two seconds.  In order to program this we will have to learn about scheduling events with the game engine's built-in timer.

We can get the number of seconds that have elapsed since the game started with the `game.getTime()` method.  As usual, you can check the usage and syntax for this method in the sgc documentation.  Here is how we might use it.  

First let's set our timer.

- [ ] Inside the `constructor` method of the `PlayerWizard` class, define a variable called `spellCastTime` and set it's value to zero.  We want this variable to be a property of `PlayerWizard` objects so don't forget to use the `this` keyword.  If you don't remember how to do this, review the last part of Part 1, when we declared a `speedWhenWalking` variable.

Now we define a *local* variable that stores the current time (*local* means it can't be used outside of the curly brackets where it was declared).

- [ ] Add the following to the beginning of your `handleSpacebar()` method for the `PlayerWizard` class:

```javascript
 let now = game.getTime();  // get the number of seconds since game start
```

- [ ] Encapsulate the rest of your code in the `handleSpacebar()` method (the code that generates a `spell` object and assigns values to its properties) within an `if` statement that tests to see if 2 seconds has elapsed like this: 

```javascript
 // if the current time is 2 or more seconds greater than the previous spellCastTime 
if (now - this.spellCastTime >= 2) { 
       // reset the timer 								
       this.spellCastTime = now;
       // and cast a spell 
       // insert the rest of your spell-generating code here
 }           
```

Hopefully the comments clarify how we use the `getTime` function to set and reset a timer, and how to use the difference between the timer variable and the current time to have something happen after 2 seconds has elapsed.    

What might not be so clear is why we use the `let` keyword to hold the current time, and a previously undefined object property `marcus.spellCastTime` to hold the time that the last spell was cast.  If we used `let` for the `spellCastTime` variable too, it would be redefined every time `handleSpacebar()` is triggered, which would make it worthless as a holder for the last spell cast time.  This is important to remember:

*Local variables are created when a function starts, and deleted when the function is completed.*

Instead we need a variable that will stick around even when we get ejected from the `handleSpacebar` function because the `if` condition fails (i.e. because the timer hasn't yet "gone off"). Thus, we use an object property which has *global* scope.

## Wrapping Up

We are not yet finished with the Wizard Duel tutorial.  If you are reading this you are farther along than I expected.  Please use the extra time to start on your Shooter project (below).

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

[^*]: If you place both conditions within a single `if` statement, then both the conditions will need to be tested every time the collision handler is called, which includes calculating the vertical offset.  If instead, you nest the "vertical offset" condition inside the "different sprites" condition, the vertical offset will only be calculated in those cases where the sprites are different.