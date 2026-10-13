# Ss12ex5 — Chẩn đoán & khắc phục 3 sự cố Pipeline CI/CD

Repo minh họa cho **Bài 5 · Session 12**: phân tích log lỗi trên GitHub Actions và
sửa trực tiếp tệp workflow để khôi phục luồng CI/CD tích xanh.

## Nội dung

- `.github/workflows/broken-pipeline.yml` — workflow đã được sửa (2 job: build & test Gradle, build & push Docker image lên GHCR).
- `troubleshooting.md` — phân tích chi tiết 3 lỗi, log lỗi, nguyên nhân gốc và cách khắc phục.
- `Dockerfile`, `build.gradle`, `settings.gradle`, `gradlew*` — project Java tối thiểu dùng cho pipeline.

## 3 lỗi đã khắc phục

1. **Permission Denied** — thêm `chmod +x gradlew` trước khi chạy test.
2. **Authentication Failed** — sửa tên secret từ `REGISTRY_TOKEM` thành `REGISTRY_TOKEN`.
3. **Build Context Error** — đặt `context: .` và `file: ./Dockerfile`.

## Cấu hình secret

Tạo secret `REGISTRY_TOKEN` trong *Settings → Secrets and variables → Actions*
(PAT có scope `write:packages`) để bước login Docker Registry thành công.

## Chạy

```bash
./gradlew test
docker build -t ss12ex5 .
```
