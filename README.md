# esp32-image-frame

Remote picture frame for the ESP32-C6-LCD-1.3 (ST7789 240x240).

The ESP32 polls `image.bin` from this repo's GitHub Pages site every 30
seconds and redraws the screen whenever the file changes.

## How to change the picture

1. Open the [converter page](https://mohammadshaireefshaikh.github.io/esp32-image-frame/)
   (works on phone or PC).
2. Pick any photo, download the generated `image.bin`.
3. Replace `image.bin` in this repo: **Add file → Upload files → Commit**.
4. Wait ~1 minute for Pages to redeploy. The ESP32 updates itself.

## Format

`image.bin` = raw RGB565 pixels, little-endian, 240x240, exactly
115200 bytes. No header. Row-major, top-left first.

## Firmware

Arduino sketch `WebImageFrame` (on the controlling PC) polls:

```
https://mohammadshaireefshaikh.github.io/esp32-image-frame/image.bin
```

using an `If-None-Match` ETag conditional request, so unchanged images
cost almost no bandwidth.
