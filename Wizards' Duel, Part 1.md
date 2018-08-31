# Wizards' Duel, Part 1

First let's start with a review of what you learned in the last tutorial.  As a test of your understanding and syntax memorization, try to complete these steps without looking back at your StrangerHunt code, unless you get stuck.  The code will be very similar to what you already did in the previous game.

In `wizardsDuel.js`, write some JavaScript code to accomplish the following things:

- [ ] Import `game` and `Sprite` modules from "../sgc/sgc.js"
- [ ] Set the background to "floor.png" (use the `game.setBackground()` method)
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

![MarcusSheet](C:/Users/Renaud/Google%20Drive/DnE/Courses/CSCI-110/Archive/csci110-retooled/docs/images/MarcusSheet.PNG)

Notice that the frame corresponding to the single image of Marcus we used in the last tutorial is the 8th one from the left.  On either side is Marcus with one or the other foot forward.  Playing these three frames in succession gives the illusion of Marcus walking down the screen. We will use the `defineAnimation` method to tell the game engine which frames to play for each facing.  

Search for (CTRL+F) `defineAnimation` in the gameController help accessed from the README file in your tutorials folder. 

![defineAnimation](C:/Users/Renaud/Google%20Drive/DnE/Courses/CSCI-110/Archive/csci110-retooled/docs/images/defineAnimation.PNG)

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

## Wrapping Up

Use Question Set 2 to review and evaluate your mastery of new concepts.

We are not yet done with this game.  If you are reading this you are farther along than I expected.  Please use the extra time to do the reading and to complete your project described below.

- [ ] Read [Functions and Sailing](http://computationaltales.blogspot.com/2011/04/functions-and-sailing.html)

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