final int MAX_SWARM_SIZE = 15;
final int FRAME_RATE = 20;

float[][] collidingEllipsesArray = new float[MAX_SWARM_SIZE + 1][4]; // each item contains center (h,k), height (2*b) & width (2*a) of an ellipse 
float[][] swarmPosArray = new float[MAX_SWARM_SIZE][4]; // {previousX, previousY, deltaXWithLeader, deltaYWithLeader}
// final float[] swarmDistArray = { -30.0, 50.0, -50.0, 100.0, -70.0, 30.0, -120.0, 80.0, -100.0, 60.0};
float[][] enemyPoints = new float[1500][2];
int currentSwarmLimit = 5;
boolean isProgramEnded = false;
long runTime = 0;




boolean doesCollide(float x, float y) {
  // float h, float k, float a, float b
  for(int i=0; i<MAX_SWARM_SIZE; i++) {
    if(collidingEllipsesArray[i][0]<0 || collidingEllipsesArray[i][1]<0 || collidingEllipsesArray[i][2]<0 || collidingEllipsesArray[i][3]<0)
      break;
      
    float h = collidingEllipsesArray[i][0];
    float k = collidingEllipsesArray[i][1];
    float b = collidingEllipsesArray[i][3] / 2.0;
    float a = collidingEllipsesArray[i][2] / 2.0;
    
    float p = (pow((x - h), 2) / pow(a, 2)) + (pow((y - k), 2) / pow(b, 2)); 
    if(p <= 1)
      return true;
  }
  return false;
}

void drawEnemy() {
  int pos = (int) (runTime % 1000);
  float x = enemyPoints[pos][0], y = enemyPoints[pos][1];
  
  fill(0, 0, 0);
  circle(x, y, 10);
  
  strokeWeight(4);
  int milliSec = millis()%1000;
  float wy = y + 10 * ((milliSec<250 || (milliSec>500 && milliSec<750)) ? 1 : -1);
  line(x-30, wy, x, y);
  line(x, y, x+30, wy);
  
  // Detect & act on collision
  if( doesCollide(x, y) || doesCollide(x-30, wy) || doesCollide(x+30, wy))
    isProgramEnded = true;
}


void initEnemyPath() {
  int steps = 500;
  for (int i = 0; i < steps; i++) {
    float t = i / float(steps);
    enemyPoints[i][0] = bezierPoint(850, 100, 700, 150, t);
    enemyPoints[i][1] = bezierPoint(0, 100, 700, 800, t);
  }
  for (int i = 0; i < steps; i++) {
    float t = i / float(steps);
    enemyPoints[i+steps][0] = bezierPoint(0, 650, 250, 850, t);
    enemyPoints[i+steps][1] = bezierPoint(150, 100, 750, 800, t);
  }
  for (int i = 0; i < steps; i++) {
    float t = i / float(steps);
    enemyPoints[i+steps*2][0] = bezierPoint(600, 0, 800, 450, t);
    enemyPoints[i+steps*2][1] = bezierPoint(0, 250, 750, 800, t);
  }
}





void drawBee(int serial, float scale) {
  // int shouldFlicker = runTime%200==0 ? 1 : 0;
  // float dx = ; //random(-1,1) * deviation + random(-1,1) * deviation * scale * shouldFlicker;///10 * scale * shouldFlicker;
  // float x = mouseX + dx;
  // float dy = ; //random(-1,1) * deviation + random(-1,1) * deviation * scale * shouldFlicker;///10 * scale * shouldFlicker;
  // float y = mouseY + dy;
  
  float x, y;
  if(serial>0) {
    float targetX = mouseX + swarmPosArray[serial-1][2];
    float dx = targetX - swarmPosArray[serial-1][0];
    swarmPosArray[serial-1][0] += dx * 0.1;
    x = swarmPosArray[serial-1][0];
    
    float targetY = mouseY + swarmPosArray[serial-1][3];
    float dy = targetY - swarmPosArray[serial-1][1];
    swarmPosArray[serial-1][1] += dy * 0.1;
    y = swarmPosArray[serial-1][1];
  } else {
    x = mouseX;
    y = mouseY;
  }
  
  // draw wing
  fill(250, 250, 250, 200);
  noStroke();
  ellipse(x, (y+6*scale), 55*scale, 15*scale);
  ellipse(x, y, 60*scale, 15*scale);
  
  // draw sting
  fill(0, 0, 0);
  ellipse(x, y+25*scale, 10*scale, 10*scale);
  
  // draw antennas
  noFill();
  stroke(0, 0, 0);
  strokeWeight(1*scale);
  arc( (x+10*scale), (y-25*scale), 12*scale, 12*scale, PI, (PI+HALF_PI) );
  arc( (x-10*scale), (y-25*scale), 12*scale, 12*scale, (PI+HALF_PI), (PI+PI) );
  
  // draw body
  noStroke();
  fill(250,250,0);
  ellipse(x, y, 40*scale, 50*scale);
  
  // draw stripes
  stroke(0, 0, 0);
  strokeWeight(2*scale);
  line(x-17.3*scale, y-6*scale, x+17.3*scale, y-6*scale);
  line(x-18*scale, y, x+18*scale, y);
  line(x-17*scale, y+6*scale, x+17*scale, y+6*scale);
  line(x-15.2*scale, y+12*scale, x+15.2*scale, y+12*scale);
  line(x-11.9*scale, y+18*scale, x+11.9*scale, y+18*scale);
  strokeWeight(1.7*scale);
  line(x-7*scale, y+23*scale, x+7*scale, y+23*scale);
  
  // draw face
  arc(x, (y-15*scale), 8*scale, 8*scale, 0, PI);
  fill(0, 0, 0);
  circle(x-10*scale, (y-17*scale), 2*scale);
  circle(x+10*scale, (y-17*scale), 2*scale);
  
  // define colliding area
  //noFill();
  //noStroke();
  //ellipse(x, y-2*scale, (40*scale + 15*scale), (50*scale + 10*scale) );
  collidingEllipsesArray[serial][0] = x;
  collidingEllipsesArray[serial][1] = y-2*scale;
  collidingEllipsesArray[serial][2] = 40*scale + 15*scale;
  collidingEllipsesArray[serial][3] = 50*scale + 10*scale;
}



void initBeePos() {
  currentSwarmLimit = 1;
  for(int i=0; i<MAX_SWARM_SIZE; i++) {
    swarmPosArray[i][0] = -100; // previousX
    swarmPosArray[i][1] = -100; // previousY
    
    swarmPosArray[i][2] = random(-120, 120); // deltaXWithLeader
    if(swarmPosArray[i][2]>-40 && swarmPosArray[i][2]<40)
      swarmPosArray[i][2] = random(40, 120) * (swarmPosArray[i][2] > 0 ? 1 : -1);
      
    swarmPosArray[i][3] = random(-120, 120); // deltaYWithLeader
    if(swarmPosArray[i][3]>-40 && swarmPosArray[i][3]<40)
      swarmPosArray[i][3] = random(40, 120) * (swarmPosArray[i][3] > 0 ? 1 : -1);
  }
}


/**
*  System Initiation
*/
void setup(){
  size(850, 850);
  frameRate(FRAME_RATE);
  for(int i=0; i<MAX_SWARM_SIZE; i++)
    collidingEllipsesArray[i][0] = collidingEllipsesArray[i][1] = collidingEllipsesArray[i][2] = collidingEllipsesArray[i][3] = -1;
  runTime = 0;
  
  initBeePos();
  initEnemyPath();
}

void mouseClicked() {
  if(isProgramEnded && mouseX>340 && mouseX<460 && mouseY>630 && mouseY<670) {
    initBeePos();
    initEnemyPath();
    isProgramEnded = false;
  }
}

/**
*  Called for each frame
*/
void draw() {
  if(isProgramEnded) {
    runTime = 0;
    textSize(24);
    textAlign(CENTER, CENTER);
    fill(250, 102, 153, 51);
    text("Oh no!\nA bee has been eaten. :(\nTry Again!", 400, 400); 
    
    noStroke();
    fill(240, 150, 150);
    rect(350, 630, 100, 40);
    ellipse(350, 650, 30, 40);
    ellipse(450, 650, 30, 40);
    fill(50, 200, 60);
    text("RESET", 400, 647); 
    
    return;
  }
  ++runTime;
  background(180,220,250);
  
  drawBee(0, 1.0);
  for(int i=0; i<currentSwarmLimit; i++)
    drawBee(i+1, 0.3);
  if( runTime % (FRAME_RATE*20) == 0)  // increasing the swarm-size by 1 in every 20 seconds
    currentSwarmLimit = (currentSwarmLimit+1) % MAX_SWARM_SIZE;
  
  drawEnemy();
}
