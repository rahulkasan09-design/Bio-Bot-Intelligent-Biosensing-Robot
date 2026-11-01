
// Bio Bot - Arduino Prototype

#define BIOSENSOR A0
#define TRIG 9
#define ECHO 10

#define IN1 2
#define IN2 3
#define IN3 4
#define IN4 5

int sensorValue;
long duration;
int distance;

void setup() {
  Serial.begin(9600);

  pinMode(TRIG, OUTPUT);
  pinMode(ECHO, INPUT);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
}

void loop() {
  // Read biosensor signal
  sensorValue = analogRead(BIOSENSOR);

  // Measure distance
  digitalWrite(TRIG, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG, LOW);

  duration = pulseIn(ECHO, HIGH, 30000);
  distance = duration * 0.034 / 2;

  Serial.print("Sensor: ");
  Serial.print(sensorValue);
  Serial.print(" | Distance: ");
  Serial.println(distance);

  // Stop when an obstacle is detected
  if (distance > 0 && distance < 25) {
    stopBot();
  } else {
    moveForward();
  }

  delay(100);
}

void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void stopBot() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
