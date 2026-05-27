# M5Unit-CardKB2-UserDemo

**Factory firmware** for the [M5Stack Unit CardKB2](https://docs.m5stack.com/en/products/sku/u215) (**SKU: U215**), built with [ESP-IDF](https://github.com/espressif/esp-idf). It provides matrix keyboard scanning, RGB status LED, and key output over **I2C / UART / ESP-NOW / BLE HID**—including manufacturing / QA hooks such as I2C-triggered production-test mode.

-----------------
## Operating mode

| Mode | Description |
|------|-------------|
| **I2C** (default) | I2C slave reports characters / key data to the host. |
| **UART** | Custom frames with key index and press / release over UART. |
| **ESP-NOW** | Broadcasts key frames (similar framing to UART). |
| **BLE HID** | Bluetooth HID keyboard, including arrow keys via Fn combos. |

### Switching modes

Hold **Fn + Sym**, then press **1 / 2 / 3 / 4** on the top number row to select **I2C / UART / ESP-NOW / BLE HID** respectively.

**Note:** **Fn + 1** alone (top-left number key) sends **ESC** in most modes; it does **not** switch modes.

--------------------
## Build environment

* idf version: ESP-IDF v6.1-dev-dirty

```bash
cd src
idf.py set-target esp32c61
idf.py build
idf.py -p <PORT> flash monitor
```

### Merge binary

After building, generate a single flash image from the bootloader, partition table, and app binaries:

```bash
esptool --chip esp32c61 merge-bin \
  -o UnitCardKB2-ESP32C61-Factory-V2_0x00.bin \
  --flash-mode dio \
  --flash-freq 80m \
  --flash-size 2MB \
  0x0 build/bootloader/bootloader.bin \
  0x8000 build/partition_table/partition-table.bin \
  0x10000 build/main.bin
```

Flash the merged binary:

```bash
esptool --chip esp32c61 -p /dev/ttyACM0 -b 1500000 write-flash 0x0 UnitCardKB2-ESP32C61-Factory-V2_0x00.bin
```

## License

This project is licensed under the MIT License—see [LICENSE](LICENSE).
