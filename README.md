# 🚀 Hướng Dẫn Nạp Firmware & Sử Dụng Tool

> 📌 Tất cả thao tác đều thực hiện trên **Windows**. Nhớ cắm sẵn board ESP32-S3 vào máy trước khi bắt đầu nhé!

---

## 🛠️ 1. Cài đặt môi trường

### Bước 1 — Cài **Python** (nếu chưa có)

Tải Python 3.10+ tại: https://www.python.org/downloads/

> ⚠️ Khi cài nhớ **tick vào ô "Add Python to PATH"** ở màn hình đầu tiên!

Kiểm tra lại:

```bash
python --version
```

✅ Hiện ra số version (ví dụ `Python 3.10.11`) → OK.

---

### Bước 2 — Cài các thư viện cần thiết

Mở **CMD** (`Win + R` → gõ `cmd` → Enter), chạy:

```bash
pip install esptool pyserial opencv-python numpy
```

Kiểm tra nhanh:

```bash
python -c "import serial, cv2, numpy; print('OK')"
```

✅ Hiện `OK` → xong bước cài đặt.

---

## 💾 2. Nạp firmware vào **T-DISPLAY-S3**

### Bước 1 — Di chuyển đến thư mục chứa file `.bin`

```bash
cd đường_dẫn_đến_thư_mục
```

### Bước 2 — Xóa flash cũ (tránh xung đột)

```bash
python -m esptool --chip esp32s3 --port COMx erase_flash
```

### Bước 3 — Nạp firmware mới

```bash
python -m esptool --chip esp32s3 --port COMx --baud 921600 write_flash 0x0 T_DISPLAY_S3_USB.bin
```

> ⚠️ **Lưu ý quan trọng:**
> Thay `COMx` bằng cổng COM thực tế mà ESP32 đang cắm.
> 👉 *Ví dụ:* ESP32 đang ở `COM1` → thay `COMx` thành `COM1`
>
> 💡 **Không biết đang ở COM mấy?**
> Mở **Device Manager** → mục `Ports (COM & LPT)` → xem tên cổng có chữ *USB-SERIAL* hoặc *CP210x*.

---

## 🐍 3. Chạy Tool bằng Python (`Usb.py, WiFi.py, Cam.py`)

### Bước 1 — Mở file `Usb.py` bằng Notepad / VS Code

Tìm dòng:

```python
PORT = 'COM1' Hoặc ESP32_IP = "192.168.1.100"
```

Sửa lại **đúng cổng COM** mà ESP32 đang kết nối (ví dụ `COM1`, `COM5`...).
Sửa lại **đúng IP** mà ESP32 đang hiển thị (ví dụ `192.168.1.180`, `192.168.1.150`...).

> 💡 Nếu không muốn sửa file, bạn có thể để nguyên và xem hướng dẫn nâng cao ở phần dưới.

### Bước 2 — Mở CMD tại thư mục chứa `Usb.py`

Cách nhanh: mở thư mục trong File Explorer → gõ `cmd` vào thanh địa chỉ → Enter.

Hoặc:

```bash
cd đường_dẫn_đến_thư_mục_chứa_Usb.py
```

### Bước 3 — Chạy tool

```bash
python Usb.py
```

✅ Nếu thấy log `FPS: ...` chạy liên tục → tool đang hoạt động, hình ảnh đang được truyền sang board!

### Bước 4 — Dừng tool

Nhấn **`Ctrl + C`** trong CMD để thoát an toàn.

> 🚫 **Cảnh báo:** Trong lúc tool đang chạy — **KHÔNG ĐƯỢC TẮT** cửa sổ CMD, nếu không sẽ ngắt kết nối với board!

---

## 📋 Tóm tắt nhanh (Cheat Sheet) Tương Tự Cho WiFi Cam

| Bước | Lệnh / Thao tác |
|------|-----------------|
| 1️⃣ | Cài Python 3.10+ (tick **Add to PATH**) |
| 2️⃣ | `pip install esptool pyserial opencv-python numpy` |
| 3️⃣ | `python -m esptool --chip esp32s3 --port COMx erase_flash` |
| 4️⃣ | `python -m esptool --chip esp32s3 --port COMx --baud 921600 write_flash 0x0 T_DISPLAY_S3_USB.bin` |
| 5️⃣ | Sửa `PORT = 'COMx'` trong `Usb.py` cho đúng |
| 6️⃣ | `python Usb.py` → chạy tool |
| 7️⃣ | `Ctrl + C` để thoát |

---

## ❓ Xử lý lỗi thường gặp

| Lỗi | Cách khắc phục |
|---|---|
| `'python' is not recognized` | Chưa cài Python hoặc chưa tick **Add to PATH** → cài lại |
| `pip` not found | Dùng `python -m pip install ...` thay vì `pip install ...` |
| `could not open port COMx` | Sai cổng COM, hoặc board chưa cắm, hoặc bị tool khác chiếm |
| `ModuleNotFoundError: No module named 'cv2'` | Chạy lại `pip install opencv-python` |
| `serial.serialutil.SerialException` | Đóng Arduino IDE / Serial Monitor nếu đang mở |
| Board không nhận firmware | Giữ nút **BOOT** trên board khi bắt đầu nạp, nhả ra sau khi hiện `Connecting...` |

---

<div align="center">

**Made by Thoai for the ESP32 community**

</div>