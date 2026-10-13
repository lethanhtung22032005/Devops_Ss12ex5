# Hướng dẫn nộp bài — Session 12 · Bài 5

Làm lần lượt 5 bước dưới đây để pipeline xanh và có đủ 3 sản phẩm nộp bài.

Sản phẩm cần nộp:
1. `.github/workflows/broken-pipeline.yml` (đã sửa) — **đã có sẵn trong repo**.
2. `troubleshooting.md` — **đã có sẵn trong repo**.
3. Ảnh chụp màn hình pipeline **tích xanh** — bạn cần tự chụp.

---

## Bước 1 — Tạo Personal Access Token (PAT)

Docker push lên GHCR cần token. Làm theo:

1. Đăng nhập GitHub → bấm avatar góc phải → **Settings**.
2. Menu trái kéo xuống → **Developer settings**.
3. **Personal access tokens** → **Tokens (classic)**.
4. **Generate new token** → **Generate new token (classic)**.
5. Điền:
   - **Note**: `ghcr-push-ss12ex5`
   - **Expiration**: 90 days (hoặc No expiration nếu chỉ làm bài).
   - **Scopes**: tick ✅ `write:packages`, ✅ `read:packages`, ✅ `delete:packages` (tùy chọn).
6. Kéo xuống → **Generate token**.
7. **Copy ngay** chuỗi token (dạng `ghp_xxx...`) — rời trang là không xem lại được.

> Nếu quên copy, phải tạo token mới. Không dán token vào file hay commit lên repo.

---

## Bước 2 — Thêm secret `REGISTRY_TOKEN` vào repo

1. Mở repo: https://github.com/lethanhtung22032005/Devops_Ss12ex5
2. Tab **Settings** → menu trái **Secrets and variables** → **Actions**.
3. Bấm **New repository secret**.
4. Điền:
   - **Name**: `REGISTRY_TOKEN`  ← phải đúng chính tả, giống hệt trong workflow.
   - **Secret**: dán chuỗi token `ghp_...` ở Bước 1.
5. Bấm **Add secret**.

> Nếu tên secret sai → đúng lỗi số 2 sẽ tái xuất hiện (`Authentication Failed`).

---

## Bước 3 — Đợi workflow chạy trên tab Actions

1. Mở tab **Actions** của repo.
2. Cửa sổ trái chọn workflow **Broken Pipeline (Fixed)**.
3. Bấm vào lần chạy mới nhất (commit `fix(ci): resolve 3 pipeline failures ...`).
4. Xem 2 job:
   - `Build & Test` — phải **xanh ✅**
   - `Build & Push Docker Image` — phải **xanh ✅**
5. Nếu còn đỏ, bấm vào job đỏ để xem dòng log lỗi và xử lý (xem mục "Xử lý sự cố" cuối file).
6. Trạng thái tổng ở đầu trang phải hiện dấu **✅** màu xanh.

---

## Bước 4 — Chụp ảnh màn hình kết quả xanh

Trên Windows (máy bạn) có 2 cách:

**Cách A — Snipping Tool (khuyến nghị):**
1. Nhấn `Win + Shift + S` → màn hình mờ đi.
2. Kéo chuột chọn vùng cần chụp (chọn trọn phần hiển thị 2 job xanh).
3. Ảnh được copy vào clipboard → mở **Paint** → `Ctrl + V` → `Ctrl + S`.
4. Lưu thành `pipeline-green.png`.

**Cách B — chụp cả cửa sổ:**
1. Mở trang Actions cho to đẹp.
2. Nhấn `Alt + PrtScn` để chụp cửa sổ đang aktif.
3. Dán vào Paint và lưu `pipeline-green.png`.

> Ảnh cần thể hiện rõ: tên repo, tên workflow, và cả 2 job đều **màu xanh ✅**.

---

## Bước 5 — Đưa ảnh vào repo và push

1. Copy file `pipeline-green.png` vào thư mục `docs/` của project:
   ```
   D:\devops\Ss12\Ss12ex5\docs\pipeline-green.png
   ```
2. Commit và push:
   ```powershell
   git add docs/pipeline-green.png
   git commit -m "docs: add green pipeline screenshot"
   git push origin main
   ```

`troubleshooting.md` đã nhúng sẵn ảnh này bằng dòng:

```markdown
![Pipeline green](docs/pipeline-green.png)
```

Sau khi push, mở `troubleshooting.md` trên GitHub sẽ thấy ảnh hiển thị.

---

## Xử lý sự cố khi pipeline vẫn đỏ

| Triệu chứng | Nguyên nhân | Cách sửa |
|---|---|---|
| `Permission denied` ở bước test | Chưa có `chmod +x gradlew` | Kiểm tra Bước "Grant execute permission for gradlew" có trong file không |
| `denied: denied` / `401 Unauthorized` | Secret sai tên hoặc token thiếu quyền | Kiểm tra secret tên đúng `REGISTRY_TOKEN`, token có `write:packages` |
| `failed to read dockerfile` | Sai context/file | Phải là `context: .` và `file: ./Dockerfile` |
| `Resource not accessible by integration` | Thiếu quyền ghi package | Job Docker phải có `permissions: packages: write` (đã có sẵn) |
| Workflow không chạy | Chưa push đúng branch | Workflow chỉ chạy khi push/PR lên nhánh `main` |

---

## Checklist nộp bài

- [ ] Đã tạo PAT và thêm secret `REGISTRY_TOKEN`.
- [ ] Tab Actions: cả 2 job màu xanh ✅.
- [ ] Đã lưu ảnh `docs/pipeline-green.png`.
- [ ] Đã commit + push ảnh lên `main`.
- [ ] Repo có đủ: `broken-pipeline.yml` (đã sửa), `troubleshooting.md`, ảnh xanh.
