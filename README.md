#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, -1);

// ===== EYES =====
#define EYE_W 26
#define EYE_Y 18
#define LEFT_X 30
#define RIGHT_X 72

// ===== STATES =====
enum State {
  E_IDLE,
  E_HAPPY,
  E_SAD,
  E_SURPRISE,
  E_FOCUS,
  E_LOVE,
  E_ANGRY,
  E_TIRED,
  E_PROUD
};

State state = E_IDLE;
unsigned long lastChange = 0;
int idx = 0;

// ===== DRAW EYES =====
void drawEyes(int hL, int hR) {
  display.clearDisplay();
  display.fillRoundRect(LEFT_X, EYE_Y, EYE_W, hL, 6, SSD1306_WHITE);
  display.fillRoundRect(RIGHT_X, EYE_Y, EYE_W, hR, 6, SSD1306_WHITE);
  display.display();
}

// ===== DRAW TEXT =====
void drawText(const char* title, const char* msg) {
  display.clearDisplay();
  display.setTextColor(SSD1306_WHITE);

  display.setTextSize(2);
  display.setCursor(20, 10);
  display.println(title);

  display.setTextSize(1);
  display.setCursor(28, 42);
  display.println(msg);

  display.display();
}

// ===== EXPRESSIONS =====
void showExpression(State s) {
  switch (s) {
    case E_IDLE:      drawEyes(22, 22); break;
    case E_HAPPY:     drawEyes(22, 22); break;
    case E_SAD:       drawEyes(10, 10); break;
    case E_SURPRISE:  drawEyes(30, 30); break;
    case E_FOCUS:     drawEyes(14, 14); break;
    case E_LOVE:      drawEyes(26, 26); break;
    case E_ANGRY:     drawEyes(16, 12); break;
    case E_TIRED:     drawEyes(8, 8);   break;
    case E_PROUD:     drawEyes(24, 24); break;
  }
}

void showMessage(State s) {
  switch (s) {
    case E_IDLE:     drawText("HELLO", "HI THERE"); break;
    case E_HAPPY:    drawText("HAPPY", "GOOD DAY"); break;
    case E_SAD:      drawText("SAD", "YOU MATTER"); break;
    case E_SURPRISE: drawText("WOW", "GOOD NEWS"); break;
    case E_FOCUS:    drawText("FOCUS", "KEEP GOING"); break;
    case E_LOVE:     drawText("LOVE", "STAY KIND"); break;
    case E_ANGRY:    drawText("ANGRY", "BREATHE IN"); break;
    case E_TIRED:    drawText("TIRED", "REST WELL"); break;
    case E_PROUD:    drawText("PROUD", "WELL DONE"); break;
  }
}

// ===== SETUP =====
void setup() {
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);
  display.clearDisplay();
  display.display();
}

// ===== LOOP =====
void loop() {
  unsigned long now = millis();

  if (now - lastChange > 2500) {
    state = (State)(idx % 9);
    showExpression(state);
    delay(1200);
    showMessage(state);

    idx++;
    lastChange = now;
  }
}

