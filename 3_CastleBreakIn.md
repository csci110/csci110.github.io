# Castle Break-In, Part 1

After his duel with Marcus, the mysterious stranger flees. Pursued by Princess Ann, he hides in a spooky castle and seals the entrance with magic.

The magic stones block Ann's path, but a sharp impact will destroy them. Thinking quickly, Ann grabs her souvenir East Alexandria soccer ball. Using the incredible header skills she learned as captain of the royal soccer team, she attempts to make her way inside the castle's walls.

In your workspace, you should see some image files for a castle, a wall, a ball sheet, a princess sheet, and some blocks.  There is also a `grass.png` file we will be using for the background.  

## Setting up the game play area

As a test of your of your JavaScript fluency, see if you can perform these tasks without looking back at previous tutorials.  Of course, feel free to copy your previous code if you get stuck!

- [ ] Import `game` and `Sprite` modules from "./sgc/sgc.js" .
- [ ] Set the background to "grass.png" using the `game` object's `setBackground()` method.

In the Stranger Hunt tutorial we created four wall objects directly from the `Sprite` class and set their properties individually in four blocks of code that looked almost identical to each other. Reimplementing repeated code over and over again isn’t ideal programming.  If there is a problem in one of the code blocks, there is often a problem in all of them, which complicates the task of trouble-shooting or modifying your code.  Better is to remove duplication by writing a component of some kind (method, class, etc.), and *reusing* it as many times as we like.  Let's use the power of the class `constructor()` method to reuse a block of code and automate this process for us.

- [ ] Define a class called `Wall` that is a child of the parent class `Sprite`.
- [ ] Define a `constructor()` method of the `Wall` class that accepts `x` , `y`, `name` and `image` arguments like this:

```javascript
constructor(x, y, name, image) {
    ...
}
```

- [ ] In the constructor method of the `Wall` class:

  - [ ] Call the parent class constructor method.
  - [ ] Set this object's x and y coordinates to `x` and `y` respectively.
  - [ ] Set this object's name to `name`.
  - [ ] Set the image of this object to the `image` variable using the `setImage()` method.
  - [ ] Set this object's `accelerateOnBounce` property to `false`.

- [ ] Create an *anonymous* object in the `Wall` class, passing the coordinates (x, y) = (0, 0), "A spooky castle wall" as the name, and "castle.png" as the image to the constructor of the class like this:

```
new Wall(0, 0, "A spooky castle wall", "castle.png");
```

It is called an anonymous object because we didn't "name it" by assigning it to a variable.

- [ ] Create a [named] object called `leftWall` from the `Wall` class, using

  * 0 and 200 for `x` and `y` respectively

  * "Left side wall" for the name
  * "wall.png" for the image

(HINT: pass these values to the constructor of the class as above)

- [ ] Create an object called `rightWall` from the `Wall` class, using
  * game.displayWidth - 48 for `x` and 200 for `y`
  * "Right side wall" for the name
  * "wall.png" for the image

You should now have a grassy game play area that is enclosed by solid objects on all sides, except for the bottom.  This process you just completed, of generalizing left, right, and top walls into a `Wall` class that serves for all of them is a form of *abstraction* -- one of the 4 pillars of Object-Oriented Programming (the others are Encapsulation, Inheritance and Polymorphism).

- [ ] Run the game to check the layout.

## Adding the player character

- [ ] Define a `Princess` class that is a child of the `Sprite` class.

- [ ] Define a `constructor()` method of the `Princess` class that accepts no arguments and does the following:

  * Calls the parent class constructor
  * Sets the name to "Princess Ann"
  * Uses the "princessSheet.png" image in the `setImage()` method
  * Sets height and width to 48
  * Sets the `x` coordinate to the middle of the display area. (HINT: use `game.displayWidth`)
  * Sets the `y` coordinate to `this.height` less than the display height (HINT: use `game.displayHeight`)

  * Set a custom property called `speedWhenWalking` equal to 150.
  * Set the `accelerateOnBounce` property to `false`.
  * Define `left` and `right` animations that use frames 9-11 and 3-5 respectively.
  
- [ ] Create an `ann` object from the `Princess` class you just defined.

- [ ] Run the game to verify that Princess Ann appears horizontally centered in the play area, with her feet touching the bottom edge of the window.

### Move her around

The Princess will move left and right using arrow keys.  Add the following to your `Princess` class definition:

- [ ] Program the navigation using Marcus's navigation from Wizards' Duel as a model. You will need to define `handleLeftArrowKey()` and `handleRightArrowKey()` methods that play the appropriate animations, set the angle to 0 or 180, and the speed of the object to `this.speedWhenWalking`.


- [ ] Run your game and test that you can move Princess Ann back and forth across the screen.

At this point, the princess can walk through the walls and off screen. That probably shouldn't be allowed.

- [ ] Implement your fix, and test it. Be sure that it is working correctly before you move on.

## Adding the soccer ball

- [ ] Define a class called `Ball` that is a child of the `Sprite` class.
- [ ] Define a `constructor()` method of the `Ball` class that does the following:
  * Calls the parent class constructor.  Have you noticed that whenever we have a constructor method in a derived class, we *always* have to call the parent class constructor?  Great, from now on I will stop reminding you to include `super()`.
  * Sets the `x` and `y` coordinates so that it is horizontally and vertically centered in the window.
  * Sets the `height` and `width` to 48.
  * Gives it an appropriate name and image sheet.
  * Defines an animation called "spin" that uses all 12 frames, and then plays that animation repeatedly (so add `true` as in `this.playAnimation("spin", true)`).
  * Sets the `speed` property equal to 1.
  * Sets the `angle` property to `50 + Math.random() * 80`.

This will make the ball move at an angle of 50 to 130 degrees, or in a generally "upward" direction.

- [ ] Create an **anonymous** instance of the `Ball` class.  (HINT: you did this previously with the spooky castle wall)

- [ ] Run your game and test that a ball appears in the middle of the screen and spins around. 

It is moving one pixel per second at this point -- probably not fast enough for you to notice -- and floating in mid-air.  Let's add some physics to make the ball's motion appear more natural.

It is commonly assumed that an object in freefall increases its speed by a fixed amount every second until it reaches terminal velocity, at which point air resistance balances the gravitational force and the object stops accelerating.  We can roughly recreate that by incrementing the ball's speed by a fixed amount every game loop, up to a certain maximum speed, using trial and error to set the increment and maximum speed until it is fast enough to be interesting and slow enough to be playable.  A speed increment of 2, and a maximum of 200 seems to work pretty well, but feel free to experiment.

- [ ] Define a `handleGameLoop()` method for the `Ball` class that increments `this.speed` by 2 every game loop *if* the speed is less than 200.
- [ ] Run the game and test that the ball starts in a random, generally upward direction and accelerates in a somewhat realistic yet playable way.

## The header "skill game"

The default collision handler for sprites is to bounce off, like a wall (angle of incidence equals angle of reflection), but Ann's header skill allows her more precise control of the bounce direction.  We will simulate this by *overriding* the default `handleCollision()` method and adjusting the angle based on the horizontal offset between the center of the ball and the center of Ann's head.

- [ ] Inside the `Princess` class definition, define a `handleCollision()` method as follows:

```javascript
handleCollision(otherSprite) {
        // Horizontally, Ann's image file is about one-third blank, one-third Ann, and 		   // one-third blank.
        // Vertically, there is very little blank space. Ann's head is about one-fourth 		// the height.
        // The other sprite (Ball) should change angle if:
        // 1. it hits the middle horizontal third of the image, which is not blank, AND
        // 2. it hits the upper fourth, which is Ann's head.
        let horizontalOffset = this.x - otherSprite.x;
        let verticalOffset = this.y - otherSprite.y;
        if (Math.abs(horizontalOffset) < this.width / 3 
                    && verticalOffset > this.height / 4) {
            // The new angle depends on the horizontal difference between sprites.
            otherSprite.angle = 90 + 2 * horizontalOffset;
        }
        return false;
}
```

The last expression is a formula for calculating the ball's bounce angle. If the ball and Ann's head are perfectly aligned, the horizontal offset is zero, so the bounce angle is 90 degrees: straight up. If the difference is not zero, it will adjust the base angle of 90 to reflect the horizontal offset of the collision.

The princess should now be able to play a game of "keepy uppy" by heading the ball each time that gravity brings it to her level. The ball should bounce against the walls and castle.  If she misses, it will leave the room at the bottom.

We still need to clean up after ourselves when the ball leaves play (i.e. remove the sprite) and add some magic blocks for her to destroy with the soccer ball. It would also be nice if she got a second and third chance in case she misses a header, and to show a "you lose" message if she misses all three.  We will add the "second and third chances" in the next part. 

# Castle Break-In, Part 2

## Multiple lives

In this game, the player will have three lives. One life is lost each time the soccer ball escapes the room. When all lives are lost, the game ends.

- [ ] Inside the `constructor()` method of the `Princess` class definition, create a custom property called `lives` and set its value equal to 3.

There is a `Sprite` method called `handleFirstGameLoop()` that is called by sgc on the first game loop that this sprite is active in the game.  The baseline `Sprite` class implementation does nothing.  We will override this method in our derived class (`Princess`) so that it will create a text area for displaying the number of lives remaining.

As it happens, there is a `game` method that does this.  It is called `createTextArea(x, y)`, where x and y define the coordinates for the upper left corner of the text box.

- [ ] Inside the `Princess` class definition, define a `handleFirstGameLoop()` method with the following lines:

```javascript
// Set up a text area to display the number of lives remaining.
this.livesDisplay = game.createTextArea(**PUT YOUR VALUE FOR X HERE**, 20);
this.updateLivesDisplay();
```

In place of **PUT YOUR VALUE OF X HERE**, pick a value that is three soccer ball widths to the *left* of the *right* edge of the display (HINT: use `game.displayWidth`).  The last line calls a custom method we have not defined yet.  We will now.

- [ ] Still inside the `Princess` class definition, define a custom method called `updateLivesDisplay()` that contains the following line of code:

```javascript
game.writeToTextArea(this.livesDisplay, "Lives = " + this.lives);
```

The first argument of the method is the variable that refers to the text area object, and the second argument is the string of text to put there.

Perhaps you are thinking to yourself, "Why didn't we just put that line inside the `handleFirstGameLoop()` method??"  The reason is that we will want to update the lives display several times throughout the game, not just on the first pass through the game loop, which means we would have to reiterate that line somewhere else in the program.  Remember we want to *reuse* our code whenever possible to avoid repeating ourselves and complicating the task of troubleshooting and maintenance.  I know ... I am repeating myself.  Perhaps I should take my own advice and write a method to remind you to reuse your code!  `tutorial.reduceReuseRecycle() {// Reminder to reuse code instead of repeating it}`

## Dropping the ball

If the ball leaves play, we should delete the sprite to avoid memory leaks, and decrement the life counter by one.  First let's create a custom method in the `Princess` class that decrements the life counter and gives us a new ball until we run out of lives.

- [ ] In the `Princess` class definition, define a custom method called `loseALife()` that does the following:
  - [ ] Decrements the value of `this.lives` by 1.
  - [ ] Calls `this.updateLivesDisplay()`.
  - [ ] Checks to see if `this.lives` is greater than zero.  If it is, create an new anonymous instance of the `Ball` class.  If not, end the game with the following message: `game.end('The mysterious stranger has escaped\nPrincess Ann for now!\n\nBetter luck next time.'`

- [ ] In the `Ball` class definition, define a `handleBoundaryContact()` method that:
  - [ ]  Calls `game.removeSprite(this)` to delete the ball in play
  - [ ] Calls  `ann.loseALife()` 

- [ ] Run your game and verify that the lives are being properly updated and the game doesn't end until the princess misses three headers.

We will add some blocks to the game in part 3.

If you haven't done so already, please read [Variable Initialization in Busy Kitchens](http://computationaltales.blogspot.com/2011/06/variable-initialization-in-busy.html). 

# Castle Break-In, Part 3

Next, you will create the magic stone blocks that the stranger has placed in Princess Ann's way.  We will make several different types, but they will be more alike than different.  This could be an ideal time to use inheritance!  Let's start by making the parent `Block` class (which is itself a child of the `Sprite` class).

- [ ] Define a class called `Block` derived from the `Sprite` class.  
- [ ] Define a constructor method of this class that accepts two arguments (x and y) and does the following upon creation of each instance of the class:
- Sets its x and y properties equal to the values in the passed arguments (i.e. `this.x = x;` etc.)
- Gives it a name.
- Gives it the `block1.png` image file
- Sets the `accelerateOnBounce` property to `false`.

(Did you remember to do the thing I said I wasn't going to remind you to do anymore?)

- [ ] Outside the class definition, create an anonymous instance of `Block` located at (x,y) = (400, 200).

- [ ] Run your game and make sure you have a block in front of the castle gate.  It doesn't do anything ... yet.

## `For` loops

Let's say we want to create many of these blocks to make the game more interesting.  We could write a series of `new Block()` statements but there is a more efficient and elegant way: with a loop!  This is probably tied for **the** most powerful programming technique you will learn (along with conditional statements)

- [ ] Read [Loops and Making Horseshoes](http://computationaltales.blogspot.com/2011/03/loops-and-making-horseshoe.html)

- [ ] Read [W3Schools article on `for` syntax in javascript]()

Now let's use a `for` loop to automate the process of adding a row of blocks.

- [ ] Replace the line you just wrote that creates *one* anonymous block with the following `for` loop that creates multiple blocks:

```javascript
for (let i = 0; i < 5; i++) {
    new Block(200 + i * 48, 200);
}
```

The `i++` in statement 3 is a short-hand way of writing

```javascript
i = i + 1;     // increment the value of i by 1
```

The `new Block()` command is executed 5 times:

- One the first run through the loop, 		i = 0 and the `x` position of the block is 200 + 0(48) = 200
- One the second run through the loop,    i = 1 and the `x` position of the block is 200 + 1(48) = 248
- One the third run through the loop,        i = 2 and the `x` position of the block is 200 + 2(48) = 296
- One the fourth run through the loop,     i = 3 and the `x` position of the block is 200 + 3(48) = 344
- One the fifth run through the loop,         i = 4 and the `x` position of the block is 200 + 4(48) = 392
- One the sixth attempt,                               i = 5 and conditional in statement 2 fails so the loop terminates

- [ ] Run your game and test to see if you have a row of 5 blocks running in front of the castle gate.  

## Static variables for the win

The object of our game will be to eliminate all the blocks of a certain kind, not just the ones in front of the gate.  In order to know if we have eliminated all of them, we have to count how many we have made, and subtract one from this number every time a block is smashed.  To keep track of this we will need a so-called **static** variable associated with the `Block` *class*, not just a specific instance of that class.  To implement static variables in JavaScript we use the class name as the object identifier instead of the keyword `this` which refers to a specific instance of the class.  For example:  

- [ ] Outside all of the class definitions, create a variable called `Block.blocksToDestroy` and set its value to zero.  **Make sure you do this before the `for` loop that generates all the blocks!**

- [ ] Add this line to the `constructor()` method of the `Block` class

```
Block.blocksToDestroy = Block.blocksToDestroy + 1;
```

Now every instance of the class will modify this variable when the object is created.

Next we need to define a collision handling method for the `Block` class.

- [ ] In the `Block` class definition, define a new method called `handleCollision()` that does the following:

- Deletes `this` sprite using the `game.removeSprite()` method.  (*HINT: pass `this` to the method by putting it in the argument list*)
- Decrements the value of `Block.blocksToDestroy` by 1.
- Checks to see if `Block.blocksToDestroy` is less than or equal to zero.  If it is, end the game with the following message:  (*HINT: pass this message to the `game.end()` method*)

```javascript
'Congratulations!\n\nPrincess Ann can continue her pursuit\nof the mysterious stranger!'
```

* Return `true`. Make sure this happens whether or not `Block.blocksToDestroy` is less than or equal to zero! (i.e. put it outside the `if` block)  The method should return `true `if a bounce is desired, and it is - we want the ball to bounce off the block.  There is no way you would know this without looking at the [sgc documentation](https://csci110.github.io/sgc/).

- [ ] Run the game and test that you get the congratulatory message after destroying all the blocks.  (you may wish to decrease the number of blocks for testing purposes).  You should have the beginning of a "breakout" style game, where the ball destroys the blocks that it hits, but also bounces from the impact.

## Using derived classes (children) to add more blocks

Before you put very many blocks into the room, you will add some variety. There will be two more kinds of block.  They will be *mostly* like the first block but different in minor ways (like the houses in the 3 little pigs example).  An elegant way to implement this is with *children* of the parent class.  We've had some practice with this already, since all of our classes have been children of the `Sprite` class.  We will use the derived block classes, which are themselves derived from the `Sprite` class, to review the concepts of inheritance, overriding, and polymorphism.

Somewhere *after* you declare the `Block.blocksToDestroy` static variable:

- [ ] Define an `ExtraLifeBlock` class which are derived from the `Block` class.
- [ ] Create an instance of the `ExtraLifeBlock` class at (x,y) = (200, 250).  
- [ ] Define another child of the `Block` class called `ExtraBallBlock` which is just like the first one.
- [ ] Create an instance of the `ExtraBallBlock` class at (x,y) = (300, 250). 
- [ ] Run your game and observe what happens when the ball strikes an instance of `ExtraLifeBlock` or `ExtraBallBlock`.

Did you notice that they behave exactly like the parent `Block` instances -- and have all the properties and methods of `Block` like the image file  and the collision handler?  And we did this without repeating any code!  

## Inheriting behavior

Even though there is no programming for `ExtraLifeBlock`and `ExtraBallBlock`, the associated objects behave just like `Block` objects.

Why? A child (`ExtraLifeBlock` or `ExtraBallBlock`) "inherits" behavior from its parent (`Block`). The destroy-when-hit behavior is defined in the parent, but the child inherits this and behaves in the same way. (In programming, children inherit from parents, but parents never inherit from children.)

Also because of the parent/child relationship, an `ExtraLifeBlock` instance is a special kind of `Block` instance. And an `ExtraBallBlock` instance is a special kind of `Block` instance. Our class collision handler  defines behavior that causes the block to be removed when the ball hits it. This will also apply when the ball collides with instances of `ExtraLifeBlock` or `ExtraBallBlock`, because these are a kind of `Block`.

Parents help you to follow an important programming guideline: "Don't repeat yourself." Using inheritance, you do not have to define the same actions in all three types of blocks. You also don't need to do more collision rule programming for each type of block object.

## *Overriding* the parent's behavior

When a child matures, some of his or her behavior is different from the parents'.

The same is true with object programming. There is no sense in creating a child object that behaves exactly like the parent.

Right now, all three types of block have identical behavior and appearance. Let's start by defining some new properties and behavior for `ExtraLifeBlock`. The ball should still bounce against it. However, it will not be destroyed; it will remain in place. Each time the ball strikes it, the player earns an additional life.  And to distinguish it from the parent block, let's give it a different image file.  But we need to be careful here.  The parent class constructor accepts two arguments (`x` and `y`).  This means we will need to have these arguments in the constructor of the child class too, and pass those to the parent class constructor by putting them in the argument list for the `super()` method, like this:

```javascript
class ExtraLifeBlock extends Block {
    constructor(x, y) {
        super(x, y);
        ...
    }
}    
```

- [ ] In the constructor definition for the `ExtraLifeBlock` class:
  - [ ] Set the image file to `'block2.png'`.  
  - [ ] Decrement the value of `Block.blocksToDestroy` by 1.  We do this because the blocks are indestructible, so we don't want them to count against us to meet the win condition.

Each `ExtraLifeBlock` instance is already inheriting a collision handler called `handleCollision()` from its parent class, `Block`. By re-defining that event in the child, we will "override" the parent behavior.

- [ ] In the class definition for `ExtraLifeBlock`, define a new `handleCollision()` method that calls `ann.addALife()` and returns `true`.   

Note that this *overrides* (replaces) the parent class `handleCollision()` method for each instance of `ExtraLifeBlock`.  We haven't yet defined `ann.addALife()`.  We will do that now.

- [ ] In the `Princess` class definition, define an `addALife()` method that does the following:

- Increments the value of `this.lives` by 1
- Calls `this.updateLivesDisplay()`

- [ ] Run your game and test that the `ExtraLifeBlock` instance behaves as expected.

The instance of `ExtraLifeBlock` acts almost exactly like an instance of the parent `Block` class, but it used a different collision handler defined in the child object. This is an example of *polymorphism*.

"Polymorphism" comes from Greek roots that mean "having many forms". Here, this means that it's possible for a block to be an instance of `ExtraLifeBlock` and also to be an instance of `Block`, the parent.

**Polymorphism**:

*A feature of object-oriented programming that treats instances differently depending on their most specific object. In particular, allowing child objects to define their own specific behavior for some operations, while continuing to inherit parent behavior for other operations.*

Now for something new: expanding our use of the `super()` method.  It's not just for constructors!

## *Adding* to the parent's behavior

Suppose you want `ExtraBallBlock` to behave differently, too. It will still do everything the parent does: the ball should bounce against it, and the block should be destroyed. But *in addition*, a second soccer ball will be created, to add interest to the game play.

- [ ] In the constructor definition for the `ExtraBallBlock` class, set the image file to `'block3.png'`.

- [ ] Override the  `handleCollision()` method of the `ExtraBallBlock` class like this:

```javascript
handleCollision() {
   super.handleCollision(); // call function in superclass
   new Ball(); // extend superclass behavior
   return true;
}
```

The first command, `super.handleCollision();`,  calls the same event in the parent class. If you stopped here, you would get exactly the same behavior as you had before overriding the parent's behavior. But the goal is to *add* to that behavior, so we add some additional code to the function definition; in this case, creating a new instance of the `Ball` class.

- [ ] Run the game and test for the expected behavior. When the ball strikes an instance of `ExtraBallBlock`, the block should be destroyed and an additional soccer ball should appear.

### Removing the penalty for losing the 'extra' balls

There is a small problem with this programming. Each time a soccer ball leaves the room, the player loses a life. The extra soccer balls are intended as a fun and useful "bonus" only. As long as the player keeps one or more balls in the air, there should be no penalty.

You will now modify things so that the player loses a life only when all soccer balls have left the room.

- [ ] Outside the class definitions, but *before* you create an object in the `Ball` class, initialize a new *static* variable called `Ball.ballsInPlay` and set its value to zero.

- [ ] In the constructor method definition of the  `Ball` class, add a line to the  to increment the value of `Ball.ballsInPlay` by 1.

- [ ] In the class definition for `Ball`, modify the `handleBoundaryContact()` method definition to decrement the value of `Ball.ballsInPlay` by 1, and then only call the `ann.loseALife()` method *if* `Ball.ballsInPlay` is equal to zero.

- [ ] Run the game to verify that everything is working correctly.

  ## Wrapping up

  One final point: this game's level of interest and challenge depends not only on the programming, but also on the arrangement of blocks in the room. Experiment until you find a good balance.  Challenge yourself to use `for` loops to create rows or columns of blocks. 

  If you find it too difficult to catch the balls when they are first created, consider modifying the speed of the Princess.

- [ ] When you are satisfied with the playability of your game, submit your assignment on Sakai by linking the url of your repl.

- [ ] Use Question Set 4 to review and evaluate your mastery of new concepts. 

  ## Break-In Project

- [ ] Create a copy of your `.js` and `.html` files.  Modify the copied version of your Castle Break-In game to add two or more classes derived from the `Block` class.  Have each derived class provide a unique benefit, power-up or penalty to the player.  

- [ ] Submit your project by linking the url of your repl on Sakai.