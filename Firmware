#include <SPI.h>
#include <TFT_eSPI.h>
#include "FS.h"
#include "LittleFS.h"

TFT_eSPI tft = TFT_eSPI();

const int audioOutPin = 25;
const int touchPin1 = T0;
const int touchPin2 = T3;

void setup() {
  Serial.begin(115200);

  if (!LittleFS.begin(true)) {
    Serial.println("An Error has occurred while mounting LittleFS");
    return;
  }

  tft.init();
  tft.setRotation(1);
  tft.fillScreen(TFT_BLACK);
  
  tft.setTextColor(TFT_WHITE, TFT_BLACK);
  tft.drawString("ESP32 Touch Soundboard", 20, 30, 2);
  tft.drawString("Ready for Touch Input...", 20, 70, 1);
}

void loop() {
  int touchValue1 = touchRead(touchPin1);
  int touchValue2 = touchRead(touchPin2);

  if (touchValue1 < 30) {
    playSound("sound1.wav");
    delay(300);
  }
  
  if (touchValue2 < 30) {
    playSound("sound2.wav");
    delay(300);
  }
}

void playSound(const char *filename) {
  File audioFile = LittleFS.open(filename, "r");
  if (!audioFile) {
    Serial.println("Failed to open audio file");
    return;
  }
  
  while (audioFile.available()) {
    int sample = audioFile.read();
    dacWrite(audioOutPin, sample);
    delayMicroseconds(125);
  }
  audioFile.close();
}
