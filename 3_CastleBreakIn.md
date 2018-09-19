# Castle Break-In, Part 1

After his duel with Marcus, the mysterious stranger flees. Pursued by Princess Ann, he hides in a spooky castle and seals the entrance with magic.

The magic stones block Ann's path, but a sharp impact will destroy them. Thinking quickly, Ann grabs her souvenir East Alexandria soccer ball. Using the incredible header skills she learned as captain of the royal soccer team, she attempts to make her way inside the castle's walls.

In your c9 workspace, you should see some image files for a castle, a wall, a ball sheet, a princess sheet, and some blocks.  There is also a `grass.png` file we will be using for the background.  

## Setting up the game play area

As a test of your of your JavaScript fluency, see if you can perform these tasks without looking back at previous tutorials.  Of course, feel free to copy your previous code if you get stuck!

- [ ] Import `game` and `Sprite` modules from "./sgc/sgc.js" .
- [ ] Set the background to "grass.png" using the `game` object's `setBackground()` method.

In the Stranger Hunt tutorial we created four wall objects directly from the `Sprite` class and set their properties individually in four blocks of code that looked almost identical to each other. Reimplementing repeated code over and over again isn’t ideal programming.  If there is a problem in one of the code blocks, there is often a problem in all of them, which complicates the task of trouble-shooting or modifying your code.  Better is to remove duplication by writing a component of some kind (method, class, etc.), and *reusing* it as many times as we like.  Let's use the power of the class `constructor()` method to reuse a block of code and automate this process for us.

- [ ] Define a class called `Wall` that is a child of the parent class `Sprite`.
- [ ] Define a `constructor()` method of the `Wall` class that accepts `x` , `y`, `name` and `image` arguments like this:

```
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

You should now have a grassy game play area that is enclosed by solid objects on all sides, except for the bottom.  This process you just completed, of generalizing left, right, top and bottom walls into a `Wall` class that serves for all of them is a form of *abstraction* -- one of the 4 pillars of Object-Oriented Programming (the others are Encapsulation, Inheritance and Polymorphism).

- [ ] Run the game to check the layout.

## Adding the player character

- [ ] Define a `Princess` class that is a child of the `Sprite` class.

- [ ] Define a `constructor()` method of the `Princess` class that accepts no arguments and does the following:

  * Calls the parent class constructor
  * Sets the name to "Princess Ann"
  * Uses the "ann.png" image in the `setImage()` method
  * Sets height and width to 48
  * Sets the `x` coordinate to the middle of the display area. (HINT: use `game.displayWidth`)
  * Sets the `y` coordinate to `this.height` less than the display height (HINT: use `game.displayHeight`)

  * Set a custom property called `speedWhenWalking` equal to 150.
  * Set a custom property called `lives` equal to 1.
  * Set the `accelerateOnBounce` property to `false`.
  * Define `left` and `right` animations that use frames 9-11 and 3-5 respectively.

- [ ] Create an `ann` object from the `Princess` class you just defined.

- [ ] Run the game to verify that Princess Ann appears horizontally centered in the play area, with her feet touching the bottom edge of the window.

### Move her around

The Princess will move left and right using arrow keys.  Add the following to your `Princess` class definition:

- [ ] Program the navigation using Marcus's navigation from Wizards' Duel as a model. You will need to define `handleLeftArrowKey()` and `handleRightArrowKey()` methods that play the appropriate animations, set the angle to 0 or 180, and the speed of the object to `this.speedWhenWalking`.

- [ ] Define a `handleGameLoop()` method that sets the speed to zero, and uses the `Math.max` and `Math.min` functions to keep the sprite within the play area.  Keeping `ann` from going out of bounds on the left is pretty straightforward; just use the maximum of `leftWall.width` or her current position (`this.x`).  The right hand side is a little more tricky:

```
this.x = Math.min(game.displayWidth - rightWall.width - this.width, this.x);
```

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
  * Defines an animation called "spin" that uses all 12 frames, and then plays that animation.
  * Sets the `speed` property equal to 1.
  * Sets the `angle` property to `50 + Math.random() * 80`.

This will make the ball move at an angle of 500 to 130 degrees, or in a generally "upward" direction.

- [ ] Create an **anonymous** instance of the `Ball` class.  (HINT: you did this previously with the spooky castle wall)

- [ ] Run your game and test that a ball appears in the middle of the screen and spins around. 

It is moving one pixel per second at this point -- probably not fast enough for you to notice -- and floating in mid-air.  Let's add some physics to make the ball's motion appear more natural.

It is commonly assumed that an object in freefall increases its speed by a fixed amount every second until it reaches terminal velocity, at which point air resistance balances the gravitational force and the object stops accelerating.  We can roughly recreate that by incrementing the ball's speed by a fixed amount every game loop, up to a certain maximum speed, using trial and error to set the increment and maximum speed until it is fast enough to be interesting and slow enough to be playable.  A speed increment of 2, and a maximum of 200 seems to work pretty well, but feel free to experiment.

- [ ] Define a `handleGameLoop()` method for the `Ball` class that increments `this.speed` by 2 every game loop *if* the speed is less than 200.
- [ ] Run the game and test that the ball starts in a random, generally upward direction and accelerates in a somewhat realistic yet playable way.

## The header "skill game"

The default collision handler for sprites is to bounce off, like a wall (angle of incidence equals angle of reflection), but Ann's header skill allows her more precise control of the bounce direction.  We will simulate this by *overriding* the default `handleCollision()` method and adjusting the angle based on the horizontal offset between the center of the ball and the center of Ann's head.

- [ ] Inside the `Princess` class definition, define a `handleCollision()` method as follows:

```
handleCollision(otherSprite) {
        // Horizontally, Ann's image file is about one-third blank, one-third Ann, and 		   //one-third blank.
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

The last expression is a formula for calculating the ball's bounce angle. If they are perfectly aligned, the horizontal offset is zero, so the bounce angle is 90 degrees: straight up. If the difference is not zero, it will adjust the base angle of 90 to reflect the horizontal offset of the collision.

The princess should now be able to play a game of "keepy uppy" by heading the ball each time that gravity brings it to her level. The ball should bounce against the walls and castle.  If she misses, it will leave the room at the bottom.

We still need to clean up after ourselves when the ball leaves play (i.e. remove the sprite) and add some magic blocks for her to destroy with the soccer ball. It would also be nice if she got a second and third chance in case she misses a header, and to show a "you lose" message if she misses all three.  We will add the "second and third chances" in the next part. 

# Castle Break-In, Part 2

## Multiple lives

In this game, the player will have three lives. One life is lost each time the soccer ball escapes the room. When all lives are lost, the game ends.

- [ ] Inside the `constructor()` method of the `Princess` class definition, create a custom property called `lives` and set its value equal to 3.

There is a `Sprite` method called `handleFirstGameLoop()` that is called by sgc on the first game loop that this sprite is active in the game.  The baseline `Sprite` class implementation does nothing.  We will override this method in our derived class (`Princess`) so that it will create a text area for displaying the number of lives remaining.

As it happens, there is a `game` method that does this.  It is called `createTextArea(x, y)`, where x and y define the coordinates for the upper left corner of the text box.

- [ ] Inside the `Princess` class definition, define a `handleFirstGameLoop()` method with the following lines:

```
// Set up a text area to display the number of lives remaining.
this.livesDisplay = game.createTextArea(**PUT YOUR VALUE FOR X HERE**, 20);
this.updateLivesDisplay();
```

In place of **PUT YOUR VALUE OF X HERE**, pick a value that is three soccer ball widths to the *left* of the *right* edge of the display (HINT: use `game.displayWidth`).  The last line calls a custom method we have not defined yet.  We will now.

- [ ] Still inside the `Princess` class definition, define a custom method called `updateLivesDisplay()` that contains the following line of code:

```
game.writeToTextArea(this.livesDisplay, "Lives = " + this.lives);
```

The first argument of the method is the variable that refers to the text area object, and the second argument is the string of text to put there.

Perhaps you are thinking to yourself, "Why didn't we just put that line inside the `handleFirstGameLoop()` method??"  The reason is that we will want to update the lives display several times throughout the game, not just on the first pass through the game loop, which means we would have to reiterate that line somewhere else in the program.  Remember we want to *reuse* our code whenever possible to avoid repeating ourselves and complicating the task of troubleshooting and maintenance.  I know ... I am repeating myself.  Perhaps I should take my own advice and write a method to remind you to reuse your code!  `tutorial.reduceReuseRecycle() {// Reminder to reuse code instead of repeating it}`

## Dropping the ball

If the ball leaves play, we should delete the sprite to avoid memory leaks, and decrement the life counter by one.  First let's create a custom method in the `Princess` class that decrements the life counter and gives us a new ball until we run out of lives.

- [ ] In the `Princess` class definition, define a custom method called `LoseALife()` that does the following:
  - [ ] Decrements the value of `this.lives` by 1.
  - [ ] Calls `this.updateLivesDisplay()`.
  - [ ] Checks to see if `this.lives` is greater than zero.  If it is, create an new anonymous instance of the `Ball` class.  If not, end the game with the following message: `game.end('The mysterious stranger has escaped\nPrincess Ann for now!\n\nBetter luck next time.'`

- [ ] In the `Ball` class definition, define a `handleBoundaryContact()` method that:
  - [ ]  Calls `game.removeSprite(this)` to delete the ball in play
  - [ ] Calls  `ann.loseALife()` 

- [ ] Run your game and verify that the lives are being properly updated and the game doesn't end until the princess misses three headers.

We will add some blocks to the game in part 3.

If you haven't done so already, please read [Variable Initialization in Busy Kitchens](http://computationaltales.blogspot.com/2011/06/variable-initialization-in-busy.html). 