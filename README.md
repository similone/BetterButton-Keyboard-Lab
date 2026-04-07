# BetterButton-Keyboard-Lab

#include "BetterButton.h"

int button1Pin = 29;
int button2Pin = 30;
int button3Pin = 31;

BetterButton button1(button1Pin, 72);
BetterButton button2(button2Pin, 74);
BetterButton button3(button3Pin, 76);

BetterButton* buttonArray[3] = {&button1, &button2, &button3};

void setup() {
  Serial.begin(9600);

  for (int i = 0; i < 3; i++) {
    buttonArray[i]->pressHandler(onPress);
    buttonArray[i]->releaseHandler(onRelease);
  }
}

void loop() {
  for (int i = 0; i < 3; i++) {
    buttonArray[i]->process();
  }
}

void onPress(int val) {
  Serial.print(val);
  Serial.println(" on");
  usbMIDI.sendNoteOn(val, 99, 1);
}

void onRelease(int val) {
  Serial.print(val);
  Serial.println(" off");

  usbMIDI.sendNoteOff(val, 0, 1);
}
