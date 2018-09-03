```javascript
import {game, Sprite} from "./sgc/sgc.js";

game.setBackground("floor.png");
game.showScore = true;

let topWall = new Sprite();
topWall.width = 800;
topWall.height = 48;
topWall.setImage("horizontalWall.png");
topWall.x = 0;
topWall.y = 0;
topWall.accelerateOnBounce = false;

let leftWall = new Sprite();
leftWall.width = 48;
leftWall.height = 504;
leftWall.setImage("verticalWall.png");
leftWall.x = 0;
leftWall.y = topWall.height;
leftWall.accelerateOnBounce = false;

let bottomWall = new Sprite();
bottomWall.width = 800;
bottomWall.height = 48;
bottomWall.setImage("horizontalWall.png");
bottomWall.x = 0;
bottomWall.y = topWall.height + leftWall.height;
bottomWall.accelerateOnBounce = false;

let rightWall = new Sprite();
rightWall.width = 48;
rightWall.height = 504;
rightWall.setImage("verticalWall.png");
rightWall.x = topWall.width - rightWall.width;
rightWall.y = topWall.height;
rightWall.accelerateOnBounce = false;

class Princess extends Sprite {
    constructor() {
        super();
        this.name = "Princess Ann";
        this.width = 48;
        this.height = 48;
        this.setImage("ann.png");
        this.x = leftWall.width + (Math.random() * (rightWall.x - this.width - leftWall.width));
        this.y = topWall.height + (Math.random() * (bottomWall.y - this.height - topWall.height));
        this.angle = Math.random() * 360;
        this.speed = 200; 
    }
    handleMouseClick() {
        game.end();
    }
}

let ann = new Princess();

class Wizard extends Sprite {
    constructor(){
        super();
        this.width = 48;
        this.height = 48;
        this.x = leftWall.width + (Math.random() * (rightWall.x - this.width - leftWall.width));
        this.y = topWall.height + (Math.random() * (bottomWall.y - this.height - topWall.height));
        this.angle = Math.random() * 360;
    }
    handleMouseClick(){
        game.score = game.score + this.clickScore;
        this.x = leftWall.width + (Math.random() * (rightWall.x - this.width - leftWall.width));
        this.y = topWall.height + (Math.random() * (bottomWall.y - this.height - topWall.height));
        this.speed = this.speed * 1.1;       
    }
}

let marcus = new Wizard();
marcus.speed = 100;
marcus.name = "Marcus the Wizard";
marcus.clickScore = -10;
marcus.setImage("marcus.png");

let stranger = new Wizard();
stranger.speed = 300;
stranger.name = "A mysterious stranger";
stranger.setImage("stranger.png");
stranger.clickScore = 10;
```

