# Starbie: Totoro the pocket pet

This is my Week 1 project for Hack Club's Half-Life. Starbie is a handheld virtual pet, like a Tamagotchi, that runs on a circuit board I designed myself. My pet's name is **Totoro**.

I had never done anything with hardware before this. I had never opened KiCad, never drawn a schematic, never written Arduino code and never used GitHub. Everything in this repo is the first time I have done it.

<!-- IMAGE: picture of the finished PCB (3D view or PCB editor screenshot) -->
<img width="791" height="646" alt="Screenshot 2026-10-11 165947" src="https://github.com/user-attachments/assets/c6738299-d40b-43aa-976d-e81e958a1b8f" />
<img width="1918" height="1078" alt="Screenshot 2026-10-11 165858" src="https://github.com/user-attachments/assets/b9a15854-f209-4ef4-812a-4cb3217cd8be" />



## What it does

- Totoro walks back and forth across a small OLED screen with a sparkle trail blinking behind it, and its name sits in the top corner.
- **Button 1** opens a round menu with four choices: **NAP**, **PLAY**, **FEED** and **PET**. You tilt the board to roll a ball onto the choice you want, then press Button 1 again to confirm.
- Each choice changes Totoro's three stats (joy, energy and fullness) and plays a reaction: sleeping with little Zs, running laps, jumping, or hearts.
- **Button 2** shows the stats screen, with the temperature and humidity from the DHT11 sensor.
- Shaking the board wakes Totoro up and makes it happier.
- The stats are saved, so Totoro is the same after the power is turned off and on.

## Parts (BOM)

| Qty | Part | What it does |
|---|---|---|
| 1 | Seeed Studio XIAO ESP32-C3 | The brain. Runs the code. |
| 1 | 0.96" OLED display, 128x64, I2C (SSD1306) | The screen Totoro lives on. |
| 1 | MPU6050 module | Senses tilt and shaking. |
| 1 | DHT11 sensor | Measures temperature and humidity. |
| 1 | 10k resistor | Pull-up for the DHT11 data wire. |
| 2 | Cherry MX style switches | Button 1 and Button 2. |
| 1 | Custom PCB (this repo) | Holds and connects everything. |

## Wiring

| Part | Pin on the part | XIAO pin | Number in the code |
|---|---|---|---|
| OLED and MPU6050 | SCL | D4 | GPIO 6 |
| OLED and MPU6050 | SDA | D5 | GPIO 7 |
| DHT11 | DATA (with 10k pull-up to 3.3V) | D1 | GPIO 3 |
| Button 1 | one side (other side to GND) | D2 | GPIO 4 |
| Button 2 | one side (other side to GND) | D3 | GPIO 5 |
| All modules | power and ground | 3V3 and GND | not in the code |

<!-- IMAGE: screenshot of the schematic -->
<img width="1012" height="766" alt="Screenshot 2026-10-11 170527" src="https://github.com/user-attachments/assets/1da3169d-1d0e-4cb3-bbbb-857d81f723e0" />


## Software I used

- **KiCad 10**: drawing the schematic, laying out the PCB, running the design rules check and exporting the Gerber files.
- **KiCad Image Converter**: turning my pictures into silkscreen art for the board.
- **Arduino IDE 2**: editing and compiling the firmware.
- **image2cpp**: turning a picture into the 32x32 pixel sprite for Totoro.
- **GitHub**: hosting this repo.
- **Claude**: the guide was hard for me to follow at first, so I had Claude turn it into smaller steps and help me understand my errors.

Arduino libraries: Adafruit GFX, Adafruit SSD1306, Adafruit MPU6050 and DHT sensor library. Board package: esp32 by Espressif, with the board set to XIAO_ESP32C3.

## What I changed from the guide

- **Board art**: I added my own silkscreen art (Attack on Titan fan art, including the Wings of Freedom emblem) and my name.
- **The pet**: I replaced the default sprite with my own 32x32 creature.
- **The name**: the screen shows "Totoro" in the top corner.
- **Sparkle trail**: a small sparkle blinks behind Totoro while it walks.
- **Pin numbers**: on my board SDA is on D5 and SCL is on D4, so the code uses `I2C_SDA_PIN = 7` and `I2C_SCL_PIN = 6`.

<!-- IMAGE: close-up of the silkscreen art -->

## How I built it

1. Installed KiCad and added the symbol and footprint libraries from the Half-Life care package.
2. Drew the schematic: the XIAO, the screen, the motion sensor, the DHT11 with its resistor and the two buttons, connected with net labels.
3. Gave every symbol a footprint, which is the real copper shape the part gets soldered onto.
4. Moved everything into the PCB editor, drew the star outline on the Edge.Cuts layer and arranged the parts inside it.
5. Routed every connection as a copper track and added a ground fill on the back of the board.
6. Ran the design rules check and fixed errors until only the two that the guide says are expected were left.
7. Added my art and name on the silkscreen layer.
8. Set up Arduino IDE, changed the code to make the pet mine, and compiled it.
9. Exported the Gerber and drill files and zipped them.

<!-- IMAGE: PCB editor screenshot showing the routed tracks -->

## What was hard

This was very challenging because every single part of it was new to me. Some of the things I got stuck on:

- **Getting started.** I spent most of the first hour stuck before I had even placed a part. The KiCad download link I tried gave a 404, I was not sure the mirror was safe, and I did not know which folder the library files were supposed to go in.
- **Not knowing the words.** Symbol, footprint, net label, Gerber, repo. I had to learn what each one meant before the steps made sense.
- **SDA and SCL.** The guide's schematic and the guide's code did not agree about which pin was which. I followed the schematic, then had to change two numbers in the code so it matched my board.
- **The design rules check.** My first check had 10 errors. One track ended on the wrong pad, which shorted two signals together. Three connections were missing and I had no ground fill yet. I fixed them one at a time and got down to the 2 expected errors.
- **Code that would not compile.** I pasted new lines into the middle of another line and broke it. I also got a "Missing FQBN" error, which only meant I had not picked a board yet.
- **The sprite.** The first version came out with the colors inverted and one row short (32x31 instead of 32x32).
- **Files and folders.** Arduino would not let me save the sketch into a folder inside itself, and at one point I thought I had deleted all my code while moving it.

<img width="1918" height="1077" alt="Screenshot 2026-10-11 002403" src="https://github.com/user-attachments/assets/9d28928f-4d5d-4775-b2ca-f6d062bc0353" />
<img width="1918" height="1078" alt="Screenshot 2026-10-11 003915" src="https://github.com/user-attachments/assets/70035e5c-f16a-4c3c-bc21-af5069a2805c" />
<img width="1025" height="750" alt="Screenshot 2026-10-11 005233" src="https://github.com/user-attachments/assets/90befa07-fdc9-47ea-a253-1e9b7521e509" />
It was very hard and took multiple attempts as you can see from the images above




## What I learned

- A schematic is the plan and a PCB is the real thing. The schematic says what connects to what, and the PCB is where the copper actually goes.
- One pin can have two names. D4 is printed on the XIAO, but the code calls the same pin GPIO 6.
- Net labels with the same name are connected even when no wire is drawn between them, so the spelling has to match exactly.
- The design rules check finds mistakes I could not see by looking, like a track touching the wrong pad.
- A ground fill connects every ground pin without drawing each track by hand.
- Reading an error message slowly usually tells you what is wrong.
- Save often, and copy files instead of moving them.

## What's in this repo

```
PCB/        KiCad project, schematic and board files
Gerbers/    zipped Gerber and drill files, ready to send to a board maker
Firmware/   Starbie/Starbie.ino, the Arduino code
```

Made by Pratyush Raghuvanshi.
## Credits

Built for [Hack Club Half-Life](https://hackclub.com), following the Starbie guide by SharKingStudios. Attack on Titan belongs to its creators; the board art is fan art.

Made by Pratyush Raghuvanshi.
