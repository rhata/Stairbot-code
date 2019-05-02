# Stairbot-code
// Include the wireless and motor libraries
#include <WirelessInput.h>
#include <Motor.h>

// Create your motor and wireless objects
Motor leftMotorFront;
Motor leftMotorBack;
Motor rightMotorFront;
Motor rightMotorBack;
WirelessInput gamepad(1);


void setup() {
  // start the wireless connection
  gamepad.begin(9600);

  // tell the arduino which pins control each motor
  leftMotorFront.attachDIR(26);
  leftMotorFront.attachPWM(2);
  leftMotorBack.attachDIR(27);
  leftMotorBack.attachPWM(3);

  rightMotorFront.attachDIR(25);
  rightMotorFront.attachPWM(5);
  rightMotorBack.attachDIR(24);
  rightMotorBack.attachPWM(4);
}

void loop() {
  // update the information from the gamepad
  gamepad.refresh();

  // get the left joystick y and x axis info
  int forward = gamepad.getJoystick(LEFT_Y);
  int turn = gamepad.getJoystick(LEFT_X);

  // calculate the motor speeds
  // assumes that forward is positive. may need to change some signs based on how you wired things up
  int leftSpeed = (forward + turn) / 4;
  int rightSpeed = (forward - turn) / 4;

  // if either left trigger is pressed, go into precision mode by cutting motor speeds in half
  if (gamepad.pressed(BUTTON_L1) || gamepad.pressed(BUTTON_L2)) {
    leftSpeed *= 2;
    rightSpeed *= 2;
  }

  // set the motor speed

  leftMotorFront.set(leftSpeed);
  leftMotorBack.set(leftSpeed);
  rightMotorFront.set(rightSpeed);
  rightMotorBack.set(rightSpeed);
}
