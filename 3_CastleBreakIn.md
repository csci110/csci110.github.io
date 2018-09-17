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
  * game-displayWidth - 48 for `x` and 200 for `y`
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


