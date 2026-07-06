# esp32-image-frame

Internet picture frame for the ESP32-C6-LCD-1.3 (ST7789 240x240).

Open the [web page](https://mohammadshaireefshaikh.github.io/esp32-image-frame/),
pick a photo, press **Send** — it appears on the display within seconds,
from anywhere in the world.

## How it works

```
phone/PC browser --wss--> public MQTT broker (broker.emqx.io) --tcp--> ESP32
```

- The page converts the photo to raw RGB565 240x240 in the browser and
  publishes it as 30 retained MQTT chunks (8 rows each, 3842 bytes).
- The ESP32 subscribes and writes each chunk straight to the display.
- Retained messages mean the board redraws the last image after reboot.
- Both sides connect outbound only — no port forwarding, works behind
  any router.

## Chunk format

Topic `esp32frame/<mac>/img/<n>`, payload:
`[startRow lo] [startRow hi] [rows * 480 bytes RGB565 little-endian]`

## Firmware

Arduino sketch `MqttImageFrame` (kept on the controlling PC).
Libraries: Adafruit GFX, Adafruit ST7789, PubSubClient.

## Note

The public broker topic is unauthenticated — anyone who knows the topic
string can push an image to the frame. Fine for a hobby frame; use a
private broker with credentials if that ever matters.
