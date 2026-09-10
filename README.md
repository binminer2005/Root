# ROOT

## Chạy ứng dụng

```powershell
pip install -r requirements.txt
python app.py
```

SQLite được khởi tạo tự động tại `root.sqlite3`. Lần chạy đầu tiên sẽ seed các bài mẫu từ `content.json`; sau đó ứng dụng đọc bài viết, tài khoản và các tập Radio/Podcast từ database, không đọc dữ liệu mẫu cho request. Bảng `episodes` được migration tự động khi ứng dụng khởi động.

Để chạy bằng PostgreSQL, cài dependencies rồi cấu hình connection string:

```powershell
pip install -r requirements.txt
$env:DATABASE_URL = "postgresql://user:password@localhost:5432/roof"
python app.py
```

Khi có `DATABASE_URL`, ứng dụng tự tạo các bảng `users`, `articles` và `episodes`, sau đó seed dữ liệu mẫu còn thiếu từ `content.json`. Khi bỏ biến này, ứng dụng tiếp tục dùng SQLite như mặc định.

Tài khoản admin mặc định dùng `admin@roof.local` / `admin12345`. Nên đổi trước khi chạy thật bằng:

```powershell
$env:ADMIN_EMAIL = "admin@example.com"
$env:ADMIN_PASSWORD = "mật-khẩu-mạnh"
$env:SECRET_KEY = "chuỗi-bí-mật-dài"
```

## Google OAuth

Tạo OAuth 2.0 Web Client trong Google Cloud Console và thêm redirect URI đúng với môi trường, ví dụ `http://127.0.0.1:5000/auth/google/callback`. Sau đó cấu hình:

```powershell
$env:GOOGLE_CLIENT_ID = "...apps.googleusercontent.com"
$env:GOOGLE_CLIENT_SECRET = "..."
```

Khi hai biến này có mặt, nút Google sẽ xuất hiện ở trang đăng nhập và đăng ký. Nếu chạy bằng `localhost` thay vì `127.0.0.1`, hãy thêm cả redirect URI `http://localhost:5000/auth/google/callback` trong Google Cloud Console.

## Luồng bài viết

- Người dùng đăng ký/đăng nhập rồi vào `/viet-bai` để gửi bài.
- Bài cộng đồng ở trạng thái `pending` và chưa xuất hiện ngoài trang công khai.
- Admin vào `/admin` để duyệt hoặc từ chối.
- Admin có thể đăng trực tiếp tại `/admin/viet-bai`.