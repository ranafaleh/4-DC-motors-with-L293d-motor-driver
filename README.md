# 4-DC-motors-with-L293d-motor-driver
 simple Arduino Uno project that drives 4 DC motors using  L293D motor driver channel. The motors run through a fixed movement sequence: forward, backward, then alternating right/left turns.

# how it works
1. Forward — all 4 motors move forward for 30 seconds
2. Backward — all 4 motors move backward for 60 seconds
3. Alternating turns — right turn (5s) and left turn (5s), repeated 6 times (60 seconds total)
4. After the sequence completes, the motors stop and the program halts (does not repeat)


#  Components Used

- Arduino Uno
- L293D Motor Driver 
- 4 DC Motors
- Jumper wires
- External power source for motors

#Circuit Connections

This project uses a single L293D driver chip. Each of its two channels controls a pair of motors together.

Channel 1 (Motors 1 & 2):
- Arduino Pin 2 → L293D Pin 1A (IN1)— Direction control
- Arduino Pin 3 → L293D Pin 2A (IN2) — Direction control
- Arduino Pin 5 → L293D Pin 1,2EN — Enable pin

Channel 2 (Motors 3 & 4):
- Arduino Pin 4 → L293D Pin 3A (IN3) — Direction control
- Arduino Pin 7 → L293D Pin 4A (IN4) — Direction control
- Arduino Pin 6 → L293D Pin 3,4EN — Enable pin

Motors 1 & 2 are wired to L293D outputs 1Y/2Y, and motors 3 & 4 are wired to outputs 3Y/4Y.

# code

const int IN1  = 2;  // L293D: 1A
const int IN2  = 3;  // L293D: 2A
const int EN12 = 5;  // L293D: 1,2EN


const int IN3  = 4;  // L293D: 3A
const int IN4  = 7;  // L293D: 4A
const int EN34 = 6;  // L293D: 3,4EN

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  pinMode(EN12, OUTPUT);
  pinMode(EN34, OUTPUT);

  digitalWrite(EN12, HIGH);
  digitalWrite(EN34, HIGH);
}

void loop() {

  // 1. جميع المحركات للأمام لمدة 30 ثانية
  forward();
  delay(30000);

  stopMotors();
  delay(1000);

  // 2. جميع المحركات للخلف لمدة دقيقة
  backward();
  delay(60000);

  stopMotors();
  delay(1000);

  // 3. يمين ويسار بالتناوب لمدة دقيقة
  // 5 ثوانٍ يمين + 5 ثوانٍ يسار
  // تتكرر 6 مرات = 60 ثانية
  for (int i = 0; i < 6; i++) {

    turnRight();
    delay(5000);

    stopMotors();
    delay(500);

    turnLeft();
    delay(5000);

    stopMotors();
    delay(500);
  }

  stopMotors();

  // إنهاء المهمة وعدم تكرارها
  while (true) {
  }
}

void enableMotors() {
  digitalWrite(EN12, HIGH);
  digitalWrite(EN34, HIGH);
}

void forward() {
  enableMotors();


  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);

 
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void backward() {
  enableMotors();
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);

  4
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void turnRight() {
  enableMotors();


  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);


  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void turnLeft() {
  enableMotors();

 
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);


  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}


