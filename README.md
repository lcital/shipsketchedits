# shipsketchedits
//THIS JS FILE IS FOR SKETCHING THE SHIP IN REGARDS TO ASTEROIDS.JS, SHIP.JS, AND 
//LASERS.JS

var ship; //Ship Class 
var asteroidsArray = []; //Array for Asteroids
var lasersArray = []; //Array for Lasers
var count = ; //counter for time 
var score = 0; //counter for score


function setup() 
{
  createCanvas(600, 600);
  ship = new Ship();
  //Pushes 5 Asteroid Objects onto the asteroidsArray
  for (var i = 0; i < 5; i++) 
  {
    asteroidsArray.push(new Asteroid());
  }
}

function draw() 
{
  background(0);

  //for each object in the array, the for loop
  //checks if the ship hits each asteroid,
  //use console.log to check for confirmation in "inspect element"
  for (var i = 0; i < asteroidsArray.length; i++) 
  {
    if (ship.hits(asteroidsArray[i]))
    {
      console.log('hit asteroid');

      //TO DO: increment a counter and score



    }
    //displays each asteroid after they are hit or not
    //updates their image, (break apart) or if they hit the
    //edge, they flow through the other side
    asteroidsArray[i].render();
    asteroidsArray[i].update();
    asteroidsArray[i].edges();
  }

  //this for loop goes through each laser point 
  //and updates their display on the screen (if they hit the edge)
  for (var i = lasersArray.length - 1; i >= 0; i--) 
  {
    lasersArray[i].render();
    lasersArray[i].update();
    //if the laser hit the edge, then they are taken off the array
    //with splice, we can take off objects at their index call
    //we use the for loop backwards to check each object from the
    //last index to the first
    if (lasersArray[i].offscreen()) 
    {
      lasersArray.splice(i, 1);
    } 
    else 
    {
      //also going backwards in the for loop to avoid double checking
      for (var j = asteroidsArray.length - 1; j >= 0; j--) 
      {
        if (lasersArray[i].hits(asteroidsArray[j])) 
        {
          //checking each index in the laserArray if each laser hits
          //the asteroidsArray objects
          if (asteroidsArray[j].r > 10) 
          //if the radius of each asteroid in the array is greater than 10 pixels,
          //then the asteroids will breakup
          {
            var newAsteroids = asteroidsArray[j].breakup();
            //after breaking up one asteroids, there will be two asteroids
            //so we need to add them (p5 = "concat") to the asteroidsArray
            asteroidsArray = asteroidsArray.concat(newAsteroids);
          }
          //if the asteroids are smaller than 10 pixels and they are hit, 
          //then we need to take them off the asteroidArray
          //also, if the laser hits a small asteroid, then we 
          //take the laser off the laserArray
          asteroidsArray.splice(j, 1);
          lasersArray.splice(i, 1);
          break;
        }
      }
    }
  }

  //check in the "inspect element" if the laserArray is updating
  console.log(lasersArray.length);

  //display the ship's position, rotation, and updating its movement
  //after each iteration of the draw loop
  ship.render();
  ship.turn();
  ship.update();
  ship.edges();
}

function keyReleased() 
{
  //if no key is pressed, then we stop the ship's rotation
  //and its movement forward
  ship.setRotation(0);
  ship.boosting(false);
}

function keyPressed() 
{
  //if the user hits the spacebar, then push a laser point object
  //onto the lasersArray based on the ship position and ship direction
  if (key == ' ') 
  {
    lasersArray.push(new Laser(ship.pos, ship.heading));
  } 
  //if the user hits the right arrow key, then the ship will turn 0.1 degrees
  //to the right, if we have a bigger decimal, then the ship will rotate slower
  else if (keyCode == RIGHT_ARROW) 
  {
    ship.setRotation(0.1);
  } 
  //if the user hits the left arrow key, then the ship will turn 0.1 degrees
  //in the negative direction (left)
  else if (keyCode == LEFT_ARROW) 
  {
    ship.setRotation(-0.1);
  } 
  //if the user hits the up arrow key, then the ship boolean will be true
  else if (keyCode == UP_ARROW) 
  {
    ship.boosting(true);
  }
}
