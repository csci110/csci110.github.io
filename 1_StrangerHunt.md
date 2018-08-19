# 1. Stranger Hunt

## How to Use This Tutorial

Welcome to the first tutorial for the course! 

This document has two different kinds of content: general information and action steps.

General information appears in ordinary unlabeled text, like this sentence. This form of content will be used to introduce and explain new concepts.

For action steps, carry out the instructions and **mark the checkbox!**  This helps me figure out where you are in the process should you have questions, and hopefully will help you avoid skipping steps by accident.

Complete the assignment at the end of the tutorial and submit it electronically on Sakai using the Assignments tab.

## Overview

We will be using a framework for desktop and mobile browser games called Phaser (https://phaser.io).  In order to facilitate our interactions with this framework, Prof Mattingly created an Application Programming Interface (API) called sgc.js (stands for "Simple Game Controller") that has a set of defined functions for interfacing with Phaser.  These functions mirror similar ones on other popular game engines such as Gamemaker. 

Working from a virtual machine in the cloud (c9), we will create Javascript programs that run in a browser.  For example, our first game will be called Stranger Hunt, so we could create a Javascript file called strangerhunt.js to run it.  You will need to create a personal account for the Cloud9 virtual workspace (see below).

We provide (or you can write, if you wish) a simple hyper-text mark-up language (HTML) file that calls the phaser script, and then calls the sgc script, and finally our specific game script.

This sounds complicated, but in practice it amounts to writing some Javascript code in the C9 development environment, and viewing the output in a web browser.  The beauty of it is you can give anyone in the world with an internet connection the address of this webpage and they can use your application (i.e. play your game) on any platform with no additional software required.[^1]

## Access the virtual workspace

Cloud9 (c9) combines an online code editor with a workspace in the cloud.  It supports over 40 languages including Javascript, which we will be using.   You should have received an email inviting you to sign up for a c9 account.  If not, let me know and I will send you one.  You can sign up without an invitation, but accepting my invitation will keep you from having to give c9 a credit card number.  See Sakai Welcome section item #1 for more information.  It will walk you through cloning a workspace for this assignment.

## Cast of Characters

Our tutorials will be populated with characters from the Computational Fairy Tales kingdom created by Jeremy Kubica at [http://computationaltales.blogspot.com](http://computationaltales.blogspot.com/). You are about to meet three of them.

Princess Ann is the central character of these tales. She is the daughter of King Fredrick and heir to the throne. She was tasked by the prophets to rescue the kingdom from the coming darkness. Unfortunately, the prophecy was vague about the details of the threat facing the kingdom. Thus, she is on a quest to determine what the threat is and how to stop it.

Marcus is a powerful wizard who is constantly looking for magical solutions to everyday problems. His approach to spells can teach you a lot about computational thinking and the art of computer programming.

The third character is a mysterious stranger in a dark cloak. Are his spells powerful enough to defeat Marcus? What does he have to do with the prophecy of coming darkness?

## Game Design: Stranger Hunt

You are about to build a simple game that will introduce you to the basics of running a javascript program from Cloud9.

In this game, our three characters will move about inside a room enclosed by stone walls.

As the player, you will score points by casting a spell on the mysterious stranger. (You will cast a spell with a mouse click.)

Be careful! By accidentally casting the spell on Marcus, you will lose points.

Worse still, casting the spell on Princess Ann, who has no magical protection, will end the game.

## Creating a simple game (room with score) and running it in a browser window

First we will create a valid but do-nothing game, just a background and a score board.

- [ ] Open a web browser and go to https://c9.io
- [ ] Login and open your csci110 personal workspace.

In the left margin you will notice a "Workspace" tab that contains a directory tree of the tutorials.  

In the strangerhunt folder you should see some .png files, a .html file, and a .js file.

- [ ] Double click the StrangerHunt.js file and you will see this in the editor window

```javascript
import {game} from "./sgc/sgc.js";
game.setBackground("floor.png");
game.showScore = true;
```

Some explanations are in order.  `game` is an object created in sgc that has several *properties* and *methods*.  These concepts are well illustrated with the `car` object in this [w3schools tutorial](https://www.w3schools.com/js/js_objects.asp).  Unless you are already familiar with JavaScript objects and how to define and access them, please look at the tutorial before continuing.  

- [ ] Read the w3schools tutorial about objects (https://www.w3schools.com/js/js_objects.asp)

All we are doing with these three lines is assigning values to two `game` properties (`setBackground` and `showScore`).  From now on I will refer to these properties and methods without the `game` *identifier* (the name of the object), but remember to always include it in your code!

- [ ] Open the `strangerHunt.html` file and hit the green arrow to 'Run' it.  Alternatively, you can right click the html file without opening it, and select 'Run' from the right-click menu.  You will see a status window appear below the editor window, with a statement that looks like this:

`Starting Apache httpd, serving https://csci110-your_user_name.c9users.io/tutorials/1.strangerHunt/strangerHunt.html.`

Select the link in your c9 window (hover or left click) then choose `Open` in the menu that appears.  The first time you do this you may have to let your browser "Open the App" by clicking the red button that appears. A new window will open with the `floor.png` image and the score block.

Congratulations!  First steps are the hardest; now you know how to enter Javascript code and make your application run in any browser window. 

Next we will add some walls to the room.

## Creating a Sprite

A sprite is a graphic image that can be drawn on the screen.  Later you will create these from scratch using some tools that come with Windows (Paint and Photo Editor), or one of several powerful and free image-editing programs such as GIMP and IrfanView, but for now we will just use the images already loaded in your StrangerHunt folder (`ann.png`, `floor.png`, `marcus.png` and `stranger.png`).

We also use the word `Sprite` to define a *class* of objects in `sgc.js`.  We will talk more about this in the next section.

We are going to have to put these sprites somewhere on the screen.  In order to do that we have to know something about the coordinate system we will be using.

### Coordinate system

![CoordinatePlane](images/CoordinatePlane.gif)

In order to describe the position of an object in two dimensions with coordinate pairs (x,y), we first have to define an *origin*.  The choice is an arbitrary *convention*, but everyone has to agree.  In the case of any type of window including your browser window, the convention is to use the **upper left hand corner** as the origin.  That means we will be working in Quadrant IV in the above picture to describe the location of any object in the window.  All we care about is the *magnitude* of x and y, since we know we are always staying in quadrant IV, so for simplicity, we use positive numbers for both x and y (this is another convention). Our default room size is 800 x 600 (meaning x can vary from 0 to 800, and y can vary from 0 to 600).   Our default grid size is the height and width of our character sprites, which is 48.  These dimensions are all in 'number of *pixels*" and are stored in the `game` properties `displayWidth`, `displayHeight`, and `gridSize`.  

The *direction* is indicated with a counter-clockwise angle measurement from the positive x axis.  For example; straight up is 90 degrees, left is 180 degrees, down is 270 degrees and so on.

But what is a pixel?  

- [ ] Read [Pixels, Peacocks, and the Governor's Turtle Fountain](http://computationaltales.blogspot.com/2011/06/pixels-peacocks-and-governors-turtle.html) at [computationaltales.blogspot.com](http://computationaltales.blogspot.com/p/posts-by-topic.html).

## Classes and Objects

Please read [Marcus and the Cursed Cheese](http://computationaltales.blogspot.com/2011/08/data-validation-marcus-and-cheese.html), parts 1 - 3.

**In objected oriented programming, a class defines the type of an object. In particular, an object's class defines the data and methods for that object. Alternately, the individual objects in a program can be viewed as specific instances of a class.** (from [Part 3 of Marcus and the Cursed Cheese](http://computationaltales.blogspot.com/2011/09/classes-of-cheese-part-3-of-marcus-and.html)).

There is a lengthy explanation of how to use classes in Javascript [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes), but I often find it easier to learn something by using it, so let's jump right in by looking at the class documentation for the `Sprite` class in `sgc.js`.

- [ ] Expand the `docs` folder inside the `sgc` folder. Right-click `index.html` and select `Preview`.  You will see a table of contents for the API documentation.  Select the `Sprite` class from the list on the right-hand side. 

Notice the class definition has a list of properties and methods, which can be used by any instance of the class.  In particular, notice that there are some *properties* called `width`,` height`, `x`, `y`, and `accelerateOnBounce`; and a *method* called `setImage()`.  This keeps us from having to define these every time we want to make a new sprite that would use these properties and methods!  

Let's import this class definition into our strangerHunt.js file.

- [ ] Modify the first line of your code to add the `Sprite` class to the list of *arguments* in the `import` method like this:

```javascript
import {game, Sprite} from "./sgc/sgc.js";
```

An *argument* of a method is the value passed to that method by putting it in parentheses after the method name.  For more about arguments see https://www.w3schools.com/js/js_function_parameters.asp .

## Creating an Object from a Class definition

Javascript uses an "object-oriented" programming style. This means that the programming is organized by defining the behavior of various objects within the game.

Let's declare and initialize an object called `topWall`, by creating an instance of the `Sprite` class like this:  

- [ ] Type this in your `strangerHunt.js` file, somewhere after your `import`  method:

```javascript
let topWall = new Sprite();
topWall.width = 800;
topWall.height = 48;
topWall.setImage("horizontalWall.png");
topWall.x = 0;
topWall.y = 0;
topWall.accelerateOnBounce = false;
```

Here is a breakdown of this code snippet:

<u>Use `let` to declare a [local] variable</u>

`let` is the JavaScript keyword we should use to declare and initialize variables (there are other options but *let's* use `let` whenever we can).  `topWall` is an object with all the properties and methods of the `Sprite` class but we only set the values for those properties and methods that we actually want to use.  We assign values to each property with the  `=` operator (which means "gets" in the sense of "is assigned the value of").  

Notice how we capitalized  `topWall`?  The preferred object naming convention is to use lower case for the first word and to capitalize the second word.  There is even a term for this - camelCase.  Since we can't have spaces in variable names, it's the way JavaScript pushes words together and still keeps them legible (here is a [video](https://www.youtube.com/watch?v=NJhXiR1z7Kg) if you want more information).

<u>The `new` operator</u>

The new operator creates an instance of a class.  That is to say, it creates an object with all the properties and methods of the class.  The class is like a cookie cutter template.  Any individual cookie objects made with the cookie cutter are *instances* of the class.

<u>`width` and `height`</u>

How did we know what values to use for `width` and `height`?  In order to answer that we have to look at the image file that we are using to represent this sprite.  The fourth line in the code you just wrote says the associated image file is `horizontalWall.png`. 

- [ ] Find the `horizontalWall.png` file in your workspace directory and double-click to open it.  

At the top of the new window are some image manipulation functions and a box that says W: 800px, H:48px.  This is the width (W) and height (H) of the image in pixels (px).

<u>`accelerateOnBounce`</u>

The last property (`accelerateOnBounce`) is used by sgc's collision handler and we will see how that comes into play shortly.

<u>`x` and `y` positions</u>

The position of a sprite is determined by the sprite's origin, which (as you probably guessed) is the upper left corner of the sprite.  So the top wall was pretty easy; setting the (x, y) coordinates to (0, 0) places it in the upper left corner of the browser window.  The question is, what x and y values should we use for the other walls?

To position the left wall, we can use the height of the `topWall` sprite, which is stored in the property `topWall.height`, and put the origin of the `leftWall` sprite at the bottom of the `topWall` sprite like this:

```javascript
let leftWall = new Sprite();
leftWall.width = XX;
leftWall.height = XXX;
leftWall.setImage("verticalWall.png");
leftWall.x = 0;
leftWall.y = topWall.height;
leftWall.accelerateOnBounce = false;
```

- [ ] Replace the X's in the previous code snippet with the correct width and height values for the verticalWall.png image file.
- [ ] Using the previous steps as a guide, create two more instances of the Sprite class for the right and bottom walls.  You should use the values stored in `topWall.height`, `leftWall.height`, `topWall.width`, and `rightWall.width`  to make your code more readable, and to avoid doing any math!
- [ ] Test your game.  You should have a room with four walls. 

If you haven't already, now would be a good time to synchronize your work with gitHub.

- [ ] Expand the `github-tools` directory in your workspace, right-clik the `sync.sh` file and select `Run`.

Not too exciting yet, but we have covered a lot of advanced background material that will give you a good foundation going forward.

Next we will learn another powerful way to use classes; using "child" object classes that inherit features from their "parent" classes.

If you are reading this, you are ahead of where I expected you to be at the end of day 1.  Please use the extra time to start working on your strangerHunt project (see below).

## Wrapping up

We aren't done with the first tutorial yet, but here are some other things that need to get done soon:


- [ ] Complete the quiz for Stranger Hunt in the Tests and Quizzes tab of Sakai before Friday noon. 
- [ ] Project work minimum requirements (upload to Assignments tab of Sakai) due next Wednesday.
      - [ ] Make your own sprite using the graphics editor of your choice.  
      - [ ] Download a background image file, or create one of your own.  Resize as necessary to fit in the default window.
      - [ ] Write a JavaScript program that loads the background image and places your sprite on it.  You will need to create a new .js file to do this (right-click the 1.strangerHunt folder in the Workspace tab and select `New File`, then type `MyAwesomeGame.js` or whatever else you want to call it![^2] )

[^1]: Some restrictions apply.  For example, your session has to be active in C9, and it will time out after a certain amount of inactivity.  But the code is portable - you could use a different hosting site if you wish, though usually they require payment to keep your content available.
[^2]: Provided it is a name you wouldn't be afraid to show your mother