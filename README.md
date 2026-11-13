# Study Peak Enterprise — Manifest Repo

> Repo này chứa file manifest JSON cho hệ thống tự cập nhật APK máy trung tâm.  
> **Khác hoàn toàn** với repo OTA JS (`study-peak-ota`).

---

## Cấu trúc

```
study-peak-enterprise/
├── manifest.json        ← App đọc file này để check version
└── README.md
```

## Schema `manifest.json`

```json
{
  "nativeAndroidVersionCode": 2,
  "nativeAndroidVersionName": "1.0.2",
  "nativeAndroidApkUrl": "https://github.com/maxbrightsoft-bot/study-peak-enterprise/releases/download/v1.0.2/studypeak-center-v1.0.2.apk",
  "releaseNotes": "Mô tả thay đổi",
  "updatedAt": "2026-11-12"
}
```

## Quy trình release APK mới

1. Build APK từ nhánh `enterprise/center-apk`:
   ```bash
   cd android && ./gradlew assembleRelease
   ```
   APK xuất ra: `android/app/build/outputs/apk/release/app-release.apk`

2. Đổi tên APK: `studypeak-center-v<versionName>.apk`

3. Tạo GitHub Release mới trên repo này, upload APK vào Release Assets

4. Cập nhật `manifest.json`:
   - Tăng `nativeAndroidVersionCode` (phải > giá trị cũ)
   - Cập nhật `nativeAndroidVersionName`
   - Cập nhật `nativeAndroidApkUrl` → link download APK trong Release

5. Commit & push `manifest.json` → GitHub Pages tự cập nhật

6. Các tablet trung tâm sẽ tự phát hiện và tải xuống APK mới trong vòng 1 phút (khi mở app hoặc resume)

---

## Lưu ý

- `nativeAndroidVersionCode` phải khớp với `versionCode` trong `android/app/build.gradle` của APK mới
- Không cần cập nhật `EXPO_PUBLIC_OTA_URL` — enterprise không dùng OTA JS
- Không push lên CH Play — enterprise là APK thuần


