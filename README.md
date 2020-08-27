# StarsPractice
P5.js twinkling star practice 
//Assignment #7 Spaceship follows lissajous curves, the Spaceship shoots a beam when the mouse clicks
//Sun and Planet change colors
//The functions Flight and Orbit contain the sin() and cos() for the curve
//The functions Sun and Orbit contain the sin() and cos() that change the color 
const Y_AXIS = 1;
const X_AXIS = 2;
let SpaceBlu, SpaceBlk;
let PlanetGlow;
let OuterHalo;
let xpos = 0;
var angle = 0.0;
let backgroundColor;
var waveLengthOne = 125.0;
var waveLengthTwo = 90.0;
var pointCount = 0;
var amplitude = 400;

function setup() {
	createCanvas(2000, 2000);
	//Space Colors
	SpaceBlu = color(1, 38, 97);
  SpaceBlk = color(0, 7, 18);
	
}
//Sun
function TheSun(){
  noStroke();
	//Sun Body
	fill(color(252, 241, 30));
	ellipse(width/2, height/2, 200, 200);
	//Outer Halo
	OuterHalo = color(252, 186, 3, 170);
	OuterHalo.setBlue(128 + 128 * sin(millis() / 1000));
	fill(OuterHalo);
	ellipse(width/2, height/2, 210, 210);
	//Highlight
	fill('white');
  ellipse(width/2 - 50, height/2 - 50, 20, 15);
 }

//Mars
function TheMars(){
	noStroke();
	fill(color(247, 10, 34));
	ellipse(width/2 + 400, height/2 + 100, 70, 70);	
	//Inner Glow
	fill(color(255, 88, 46, 220));
	ellipse(width/2 + 398, height/2 + 98, 60, 60);	
 }

//Jupiter
function TheJupiter(){
	noStroke();
	fill(color(252, 242, 227));
	ellipse(width/2 - 250, height/2 - 145, 60, 60);
	fill(color(158, 108, 14, 100));
	ellipse(width/2 - 250, height/2 - 155, 45, 45);
	fill(color(240, 51, 41, 150));
	ellipse(width/2 - 250, height/2 - 150, 50, 50);
 }


function Orbit(){	
	translate(width/2,height/2);
  //Making the Planet orbit the Sun
	var amplitude = height/5;
  angle += 0.0;
  var x,y;
  y = sin(radians(angle)) * amplitude;
  x = 0.5 * cos(radians(angle)) * amplitude;
	fill('white')
	ellipse(x, y, 70, 70);	
	//Inner Glow
	PlanetGlow = color(100, 50, 150);
	//using cos() to change color of orbiting planet
	PlanetGlow.setRed(128 + 128 * cos(millis() / 1000));
	fill(PlanetGlow);
	//Planet Body
	ellipse(x+2, y, 60, 60);	
 }

function StarBunch(){	
	push();
  fill('white');
	noStroke();
	//Many stars that will appear randomly
	stars(random(0,2000), random(0,2000), 8, 21, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
  stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 8, 21, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
	stars(random(0,2000), random(0,2000), 5, 10, 5);
  pop();
 }

//Makes the star shape
function stars(x, y, radius1, radius2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius2;
    let sy = y + sin(a) * radius2;
    vertex(sx, sy);
    sx = x + cos(a + halfAngle) * radius1;
    sy = y + sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
   endShape(CLOSE);
  }

//Makes the Spaceship Fly
function Flight() {	
beginShape();
  for(var i=0; i < pointCount; i++){
    angle = i / waveLengthOne * PI;
    var y = sin(angle)* 0.5*amplitude;
    angle = i / waveLengthTwo * TWO_PI;
    var x = 2*cos(angle)* 0.5*amplitude;
    vertex(x,y); 
	}
  endShape();
	//Brings back the Background so there's no copies 
	AllofSpace();
  pointCount++;
	strokeWeight(3);
	//Building the Spaceship
	//Ship Dome
	fill('white');
	arc(x, y, 40, 40, PI, TWO_PI);
	//Body of Ship
	fill('grey');
	quad(x - 20, y, x + 20, y, x + 40, y + 25, x - 40, y + 25);
	//Bottom of Ship
	fill('yellow');
	arc(x, y + 25, 40, 20, TWO_PI, PI);
	if (mouseIsPressed) {
	  //Alien Ray
	  strokeWeight(3);
	  fill(99, 237, 40, 150)
	  quad(x - 20, y + 25, x + 20, y + 25, x + 50, y + 70, x - 50, y + 70);	    
	}
 }

function AllofSpace() {
	// Space Background
  Space(50, 75, 2000, 1500, SpaceBlu, SpaceBlk, Y_AXIS);
  Space(15, 25, 2000, 1500, SpaceBlk, SpaceBlu, X_AXIS);
	//Stars and Planets
	StarBunch();
	StarBunch();
	StarBunch();
	StarBunch();
	TheSun(); 
	TheMars();
	TheJupiter();
	Orbit();
 }

function draw() {
  Flight();
 }

//Makes the graident for the space background
function Space(x, y, w, h, SpaceBlu, SpaceBlk, axis) {
  noFill();

  if (axis === Y_AXIS) {
    // Top to bottom gradient
    for (let i = y; i <= y + h; i++) {
      let inter = map(i, y, y + h, 0, 1);
      let c = lerpColor(SpaceBlu, SpaceBlk, inter);
      stroke(c);
      line(x, i, x + w, i);
    }
  } 		
}
