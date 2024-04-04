

# Graphic
 -----------------------------------------------------------------
let cols, rows;
let scl = 20;
let w = 1400;
let h = 1000;
let pox = 100
let poz = -50;
let stepX = 7;
let stepZ = 4.5;
let flying = 0;
let pos = 25;
let an = -40;
let angle = 1.3;
let terrain = [];

function setup() {
  createCanvas(600, 600, WEBGL);
  cols = w / scl;
  rows = h / scl;

  for (let x = 0; x < cols; x++) {
    terrain[x] = [];
    for (let y = 0; y < rows; y++) {
      terrain[x][y] = 0; //specify a default value for now
    }
  }
   strokeWeight(0.5);
}

function draw() {

 
  flying -= 0.1;
  let xoff = flying;
  for (let x = 0; x < rows; x++) {
    var yoff = 0;
    for (let y = 0; y < cols; y++) {
      terrain[x][y] = map(noise(xoff, yoff), 0, 1, -100, 100);
      yoff += 0.05;
    }
    xoff += 0.05;
  }
  background(0, 200, 255);
  rotateX(PI / 2.4);
  translate(-w / 3, -h / 3);
  for (let y = 0; y < rows - 1; y++) {
    beginShape(TRIANGLE_STRIP);
    for (let x = 0; x < cols; x++) {
      fill(0, 60, 255,150);//바다
      vertex(x * scl, y * scl, terrain[x][y]);
      vertex(x * scl, (y + 1) * scl, terrain[x][y + 1]);
      }
      endShape();
  }
  ambientLight(150);

 
  pointLight(255, 255, 255, -10, -250,0);//윈이 그려진 부분에 빛을 배치
  push();
  noStroke();
  fill(255,155,50);
  translate(400, 0, 200);
  sphere(30);//태양에서 빛이나는 효과를 표현
  pop();
  
  let locX = mouseX - width/4 ;
  let locY = mouseY - height /2;
  if(key == '0') {
  push();
  scale(0.5);
  translate(600, h / 2);
  rotateX(-PI/2);
  translate(locX*2, locY*2,0);
  specularMaterial(255,192,203);//재질
  rotateZ(pos/2);
  draw돌고래();
  pop();
  }
  if(key == '1') {
    pox = pox + stepX;//x좌표이동
    poz = poz + stepZ;//z좌표이동 
  translate(pox+150, h / 2,poz+100);
    an = an + angle;
  push();
  scale(0.5);
  rotateX(-PI/2);
  specularMaterial(255,192,203);//재질
    rotateZ(radians(an));
  draw돌고래();
    
  pop();  
  
  
    
    if(pox >= 700) {//너비를 넘어갈때 다시 초기값으로 포맷
    pox = 100;
    poz = -50;
    stepZ = 5;
    an = -40;
  }
    if(poz >= 75) {//위로 올라가다가 다시 내려가기,회전 반대로
    stepZ = -1*stepZ;
    
  }
 
  
  }
}

function draw돌고래() {
  

  push();
  noStroke();
  push();
  fill(255,192,203);
  ellipsoid(150, 50, 60);//위쪽 몸통
  translate(0,0.1);
  fill(255,255,255);
  ellipsoid(150, 50, 60);//아래쪽 몸통
  translate(140,0);
  fill(255,192,203);
  ellipsoid(50, 10, 10);//위쪽 주둥이
  translate(0,0.1);
  fill(255,255,255);
  ellipsoid(50, 10, 10);//아래쪽 주둥이
  translate(7,0);
  fill(255,0,0);
  ellipsoid(45, 3,10);//입
  pop();
 
  push();//왼쪽 눈
  translate(127,-4, 26);
  fill(0,0,0);
  ellipsoid(7, 7);
  pop();
 
  push();// 오른쪽 눈
  translate(127,-4, -26);
  fill(0,0,0);
  ellipsoid(7, 7);
  pop();
 
  push();//오른쪽 팔
  translate(0,0,60);
  rotateY(sin(PI/9));
  fill(255,182,193);
  ellipsoid(60, 25, 25);
  pop();
 
  push();//왼쪽 팔
  translate(0,0,-60);
  rotateY(-sin(PI/9));
  fill(255,182,193);
  ellipsoid(60, 25, 25);
  pop();
 
  push();//꼬리
  translate(-140,-10);
  fill(255,192,203);
  rotateZ(PI/25);
  ellipsoid(50, 17, 17);
  translate(-15,0);
  rotateZ(PI/15);
  ellipsoid(50, 14, 14);
  translate(-35,0, 2);
  rotateY(PI/6);
  ellipsoid(24, 8, 8);
  translate(0,0, -4);
  rotateY(-2*(PI/6));
  ellipsoid(24, 8, 8);
  pop();
 
  push();//머리 지느러미
  fill(255,182,193);
  translate(60,-40);
  rotateZ(PI/6);
  ellipsoid(60, 20, 20);
  pop();
  pop();
}
function mouseWheel(event) {
  pos += 0.005*event.delta;
}

