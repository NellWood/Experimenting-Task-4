//# Experimenting-Task-4
//Task picked from lyceelecorbusier.eu 
var formResolution = 15;
var stepSize = 2;
var distortionFactor = 1;
var initRadius = 50;
var centerX, centerY;
var x = [formResolution];
var y = [formResolution];
var filled = false;
var freeze = false;
var r, g, b    //making a random colour for the stroke  - nell

function setup(){
  createCanvas(780, 780);
  smooth();
  r = random(255);
  g = random(255);
  b = random(255);  //making a random colour for the stroke  - nell

  // init form
  centerX = width/2; 
  centerY = height/2;
  var angle = radians(360/float(formResolution));
  for (var i=0; i<formResolution; i++){
    x[i] = cos(angle*i) * initRadius;
    y[i] = sin(angle*i) * initRadius;  
  }

  stroke(r ,g, b); 
  background(255);
}

function draw(){
  // floating towards mouse position
  if (mouseX != 0 || mouseY != 0) {
    centerX += (mouseX-centerX) * 0.10;
    centerY += (mouseY-centerY) * 0.10; //changed speed of following mouse - nell 
  }

  // calculate new points
  for (var i=0; i<formResolution; i++){
    x[i] += random(-stepSize,stepSize);
    y[i] += random(-stepSize,stepSize);
    // ellipse(x[i], y[i], 5, 5);
  }

  strokeWeight(0.5);
  if (filled) fill(random(255));  //tried to change colour of stroke, can't work out how to make it change during the sketch- nell 
  else noFill();

  beginShape();
  // start controlpoint
  curveVertex(x[formResolution-1]+centerX, y[formResolution-1]+centerY);

  // only these points are drawn
  for (var i=0; i<formResolution; i++){
    curveVertex(x[i]+centerX, y[i]+centerY);
  }
  curveVertex(x[0]+centerX, y[0]+centerY);

  // end controlpoint
  curveVertex(x[1]+centerX, y[1]+centerY);
  endShape();
}

// events
function mousePressed() {
  //init form on mouse position
  centerX = mouseX; 
  centerY = mouseY;
  var angle = radians(360/float(formResolution));
  var radius = initRadius * random(0.5,1.0);
  for (var i=0; i<formResolution; i++){
    x[i] = cos(angle*i) * radius;
    y[i] = sin(angle*i) * radius;
  }
}

function keyPressed(){
    if (keyCode == DELETE || keyCode == BACKSPACE) background (255);
}

function keyTyped() {
  if (key == 's' || key == 'S') save("P_2_2_3_01.png");
  if (key == '1') filled = false;
  if (key == '2') filled = true;
  // switch draw loop on/off
  if (key == 'f' || key == 'F') freeze = !freeze;
  if (freeze == true) noLoop();
  else loop();
}
