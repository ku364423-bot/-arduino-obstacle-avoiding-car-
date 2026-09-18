# ESP32 Obstacle Avoiding Car 🚗

HC-SR04 ultrasonic sensor နဲ့ servo motor ကို သုံးပြီး အတားအဆီးတွေကို အလိုအလျောက် ရှောင်တိမ်းသွားနိုင်တဲ့ ESP32-based robot car ပါ။ ရှေ့မှာ အတားအဆီးတွေ့ရင် ရပ်ပြီး ဘယ်/ညာ လှည့်ကြည့် (servo scanning) ပြီးမှ ပိုဟင်းလင်းတဲ့ဘက်ကို ရွေးချယ်ကွေ့ပါတယ်။

## Features

- Ultrasonic sensor (HC-SR04) ကို သုံးပြီး distance measurement (timeout ပါဝင်၊ sensor error ကို 0 အဖြစ် handle လုပ်ထား)
- Servo scanning — ဘယ်/ညာ လှည့်ကြည့်ပြီး ဘယ်ဘက်က ပိုဟင်းလင်းလဲ နှိုင်းယှဉ်ဆုံးဖြတ်
- Motor control functions — forward, backward, left, right, stop
- နှစ်ဖက်စလုံး (ဘယ်/ညာ) ပိတ်နေရင် အလိုအလျောက် နောက်ဆုတ်ခြင်း (fallback behavior)

## Hardware Components

| Component | Note |
|---|---|
| ESP32 Dev Board | Main controller |
| HC-SR04 Ultrasonic Sensor | Distance measurement |
| Servo Motor (SG90 စသည်) | Sensor ကို ဘယ်/ညာ လှည့်ဖို့ |
| Motor Driver (L298N/ တူညီသော 4-pin driver) | Motor ၂ လုံး ထိန်းချုပ်ရန် |
| DC Motors x2 + Wheels | |
| Battery Pack | ESP32 + Motor driver အတွက် power |
| Chassis | Car body |

## Pin Connections

| Component | ESP32 GPIO Pin |
|---|---|
| Servo Signal | 13 |
| Ultrasonic Trig | 5 |
| Ultrasonic Echo | 18 |
| Motor 1 - IN1 | 27 |
| Motor 1 - IN2 | 26 |
| Motor 2 - IN3 | 25 |
| Motor 2 - IN4 | 33 |

> ⚠️ **Note:** ESP32 board တွေမှာ GPIO 34–39 က input-only pins ဖြစ်လို့ motor/servo output အတွက် အထက်ပါ pin များကိုသာ သုံးပါ။ Board မတူညီရင် pin number တွေ ပြောင်းလဲပေးရန် လိုအပ်နိုင်ပါတယ်။

## Required Libraries

- [ESP32Servo](https://github.com/madhephaestus/ESP32Servo) — Arduino IDE Library Manager ကနေ install လုပ်နိုင်ပါတယ် (ESP32 board တွေမှာ standard `Servo.h` က အလုပ်မလုပ်ပါ)

## How It Works

1. Ultrasonic sensor က ရှေ့မှာ အကွာအဝေး (distance) ကို ဆက်တိုက် တိုင်းတယ်
2. Distance က 20cm–400cm ကြားရှိရင် (safe range) → ရှေ့ဆက်သွား
3. အတားအဆီးတွေ့ရင် (distance ≤ 20cm) သို့မဟုတ် sensor error (0) ဖြစ်ရင်:
   - ရပ်ပြီး servo ကို ဘယ်ဘက် (150°) လှည့်ကြည့် → distance တိုင်း
   - servo ကို ညာဘက် (30°) လှည့်ကြည့် → distance တိုင်း
   - servo ကို ပြန် ရှေ့ (90°) ပြန်ထား
   - ဘယ်/ညာ ကို နှိုင်းယှဉ်ပြီး ပိုဟင်းလင်းတဲ့ (distance ပိုကြီးတဲ့) ဘက်ကို ကွေ့သွား
   - နှစ်ဖက်စလုံး ပိတ်နေရင် (0) → နောက်ဆုတ်

## Setup & Upload

1. Arduino IDE ကို install လုပ်ပြီး **ESP32 board support** ကို Board Manager ထဲ ထည့်ပါ
2. Library Manager ကနေ **ESP32Servo** library ကို install လုပ်ပါ
3. ဒီ repo ကို Download ZIP (သို့) `git clone` လုပ်ပါ
4. Wiring diagram အတိုင်း hardware ချိတ်ဆက်ပါ
5. Arduino IDE မှာ Board: ESP32 Dev Module ရွေးပြီး code ကို upload လုပ်ပါ
6. Serial Monitor (115200 baud) ဖွင့်ရင် distance reading တွေကို real-time ကြည့်နိုင်ပါတယ်

## Tuning

- `dist > 20 && dist < 400` — safe distance threshold ကို hardware/environment အလိုက် ချိန်ညှိနိုင်ပါတယ်
- `delay(600)` (left/right turn duration) — car ရဲ့ speed/weight အလိုက် turn angle ပြောင်းချင်ရင် ဒီ delay ကို ချိန်ညှိပါ

## License

Free to use, modify, and share — ပညာရေးနှင့် hobby project အတွက် open ထားပါတယ်။
