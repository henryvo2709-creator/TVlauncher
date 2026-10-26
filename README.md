# TV Home – launcher tuỳ biến cho TV box Android

Launcher kiểu "widget + dock" (Kotlin, Jetpack Compose, điều khiển bằng remote D-pad).

## Tính năng

- **Dock**: thêm / bớt / đổi thứ tự app. Giữ OK (hoặc phím Menu) trên icon → menu *Dịch trái / Dịch phải / Gỡ khỏi dock / Thông tin ứng dụng*. Nút lưới 9 ô = tất cả ứng dụng (OK: mở, giữ OK: thêm vào dock).
- **Widget tự do** trên lưới 12×6: Đồng hồ (kèm âm lịch), Lời chào, Trình phát nhạc, Thời tiết, Lịch tháng (kèm âm lịch), Chứng khoán.
- **Chế độ chỉnh sửa** (nút thanh trượt trên dock): mũi tên chọn widget → OK để *cầm*, mũi tên để di chuyển, OK để thả. Thanh công cụ: thêm widget, đổi cỡ, cài đặt riêng từng widget, xoá, đổi hình nền.
- **Thanh trên**: tên app đang chọn, *Force Quit* / *Restart* app đó, mức dùng RAM / ROM, trạng thái Wi-Fi/Ethernet.
- **Hình nền**: 5 nền gradient có sẵn hoặc tải ảnh riêng từ một đường dẫn URL.

## Nguồn dữ liệu

| Widget | Nguồn | Ghi chú |
|---|---|---|
| Thời tiết | Open-Meteo | Miễn phí, không cần API key. Đổi thành phố trong *Cài đặt* của widget |
| Chứng khoán | Yahoo Finance (không chính thức) | Nhập mã Yahoo, vd `AAPL,^GSPC,BTC-USD,GC=F,USDVND=X,VNM.VN`. Có thể ngừng hoạt động bất cứ lúc nào |
| Nhạc | Phiên media của hệ thống | Cần cấp quyền **Truy cập thông báo** (widget có nút "Cấp quyền") |
| Âm lịch | `android.icu.util.ChineseCalendar` | Hiếm khi lệch 1 ngày so với âm lịch Việt Nam do chênh múi giờ |

## Build ra APK

### Cách 1 – GitHub Actions (không cần cài gì)
1. Tạo repo GitHub mới, đẩy toàn bộ thư mục này lên (nhánh `main`).
2. Vào tab **Actions** → workflow *Build APK* chạy tự động (hoặc bấm *Run workflow*).
3. Khi xong, tải artifact **TVLauncher-apk** (có `app-debug.apk` và `app-release.apk`, đều cài thẳng được).

### Cách 2 – Android Studio
Mở thư mục này bằng Android Studio (Ladybug trở lên) → *Build > Build Bundle(s)/APK(s) > Build APK(s)*.

### Cách 3 – Dòng lệnh
Cần JDK 17 và Android SDK (`ANDROID_HOME`):
```
./gradlew assembleDebug
# kết quả: app/build/outputs/apk/debug/app-debug.apk
```

## Cài lên TV box

1. Chép APK vào USB, hoặc dùng ADB: `adb connect <IP-TV>` rồi `adb install app-debug.apk`.
2. Mở app **TV Home** một lần. Khi bấm nút **Home** trên remote, chọn *TV Home* → *Luôn luôn* để đặt làm launcher mặc định.
   Một số TV box/Android TV không hiện hộp thoại này; khi đó vào *Cài đặt > Ứng dụng > Ứng dụng mặc định > Màn hình chính* (tên mục tuỳ máy).
3. Nếu muốn quay về launcher gốc: *Cài đặt > Ứng dụng > TV Home > Xoá mặc định*.

## Lưu ý

- *Force Quit* dùng `killBackgroundProcesses` nên chỉ đóng được app đang chạy nền (app nào được mở từ launcher rồi quay về là đang chạy nền, nên dùng bình thường được).
- Project chưa được biên dịch thử trong lúc tạo ra; nếu build báo lỗi cú pháp/API, gửi nguyên log lỗi để sửa.
