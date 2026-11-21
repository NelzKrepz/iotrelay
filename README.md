# ESP32 IoT Relay Controller

Sistem kontrol relay berbasis Internet of Things (IoT) menggunakan ESP32 dan modul relay 4 channel. Project ini memungkinkan pengguna untuk mengontrol perangkat elektronik melalui web interface yang dapat diakses menggunakan browser pada HP maupun laptop.

ESP32 beroperasi dalam mode **Access Point (AP)** sehingga dapat membuat jaringan Wi-Fi sendiri tanpa memerlukan router atau koneksi internet. Web server juga dijalankan langsung pada ESP32, sehingga pengguna cukup terhubung ke jaringan Wi-Fi perangkat untuk mengakses halaman kontrol.

## Features

- ESP32 sebagai Wi-Fi Access Point.
- Web server berbasis HTTP.
- Kontrol 4 channel relay secara independen.
- Web interface yang dapat diakses melalui browser.
- Tidak membutuhkan koneksi internet atau aplikasi tambahan.

## Default Configuration

Berikut konfigurasi jaringan dan perangkat yang digunakan:

| Parameter | Default |
|---|---|
| Wi-Fi SSID | `IoT-Relay` |
| Wi-Fi Password | `12345678` |
| IP Address | `192.168.4.1` |
| HTTP Port | `80` |
| Serial Baud Rate | `115200` |
| Wi-Fi Mode | Access Point |
| Total Relay | 4 Channel |

### Accessing the Web Interface

1. Nyalakan ESP32.
2. Hubungkan HP atau laptop ke jaringan Wi-Fi `IoT-Relay`.
3. Masukkan password `12345678`.
4. Buka browser dan kunjungi alamat berikut:

   `http://192.168.4.1`

5. Web interface akan menampilkan empat channel relay yang dapat dikontrol secara independen.

## Pin Configuration

### ESP32 to Relay Module

| ESP32 GPIO | Relay Channel | Input Pin |
|---|---|---|
| GPIO 16 | Relay 1 | IN1 |
| GPIO 17 | Relay 2 | IN2 |
| GPIO 5 | Relay 3 | IN3 |
| GPIO 18 | Relay 4 | IN4 |

### Power Supply Connections

Project ini menggunakan dua sumber daya terpisah:

**ESP32**
- Mendapatkan daya melalui USB dari laptop atau USB charger.

**Relay Module**
- Menggunakan external power supply 5V.
- PSU positif (+5V) terhubung ke VCC relay.
- PSU negatif (GND) terhubung ke GND relay.

**Common Ground**

Meskipun menggunakan sumber daya yang berbeda, GND ESP32 dan GND relay tetap dihubungkan menjadi satu referensi melalui negative rail pada breadboard.

### Pull-down Resistors

Setiap jalur kontrol GPIO menggunakan resistor 10 kΩ yang dihubungkan antara jalur sinyal dan GND.

| GPIO | Resistor | Connection |
|---|---|---|
| GPIO 16 | 10 kΩ | GPIO 16 → GND |
| GPIO 17 | 10 kΩ | GPIO 17 → GND |
| GPIO 5 | 10 kΩ | GPIO 5 → GND |
| GPIO 18 | 10 kΩ | GPIO 18 → GND |

Resistor digunakan untuk menentukan kondisi logika input ketika GPIO berada dalam kondisi high-impedance.

**Catatan:** Firmware menggunakan logika active-LOW. Oleh karena itu, LOW berarti relay ON dan HIGH berarti relay OFF. Penggunaan pull-down pada modul active-LOW dapat menyebabkan relay aktif ketika GPIO belum diinisialisasi. Pastikan konfigurasi resistor sesuai dengan karakteristik modul relay yang digunakan.

## Relay Logic

Project ini menggunakan konfigurasi relay active-LOW.

| GPIO State | Relay State |
|---|---|
| LOW | ON |
| HIGH | OFF |

Pada saat ESP32 melakukan inisialisasi, setiap GPIO diatur ke HIGH sehingga kondisi awal relay adalah OFF.

## Project Structure

```text
iotrelay/
├── iotrelay.ino
├── Relay.cpp
└── Relay.h
```

| File | Description |
|---|---|
| `iotrelay.ino` | Main program, Wi-Fi Access Point, HTTP server, dan web interface. |
| `Relay.cpp` | Implementasi fungsi dan pengelolaan kondisi relay. |
| `Relay.h` | Deklarasi class Relay. |

## Requirements

- ESP32 Development Board
- 4-Channel Relay Module (5V)
- 4 × 10 kΩ Resistors
- External 5V Power Supply
- Breadboard dan jumper wires
- Arduino IDE
- ESP32 Arduino Core

## Uploading the Firmware

1. Buka folder project menggunakan Arduino IDE.
2. Pastikan file `iotrelay.ino`, `Relay.cpp`, dan `Relay.h` berada dalam satu folder.
3. Pilih board ESP32 yang sesuai.
4. Pilih COM port yang digunakan.
5. Upload firmware ke ESP32.
6. Buka Serial Monitor dengan baud rate `115200` untuk melihat informasi Access Point dan status HTTP server.

---

**Note:** Project ini dibuat sebagai implementasi sederhana sistem kontrol IoT menggunakan ESP32, GPIO, relay, Wi-Fi Access Point, dan web server.
