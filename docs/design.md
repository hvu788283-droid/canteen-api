# Thiết kế API – Canteen API

## 1. Tài nguyên MenuItem

| Trường | Kiểu | Ràng buộc | Ví dụ |
| --- | --- | --- | --- |
| id | int | Hệ thống tự sinh | 1 |
| name | str | 2–100 ký tự, duy nhất không phân biệt hoa thường (BR1) | Bún cá Hải Phòng |
| category | enum | com, bun_pho, do_uong, an_vat (BR3) | bun_pho |
| price | int | 0 < giá ≤ 200000, bội số của 1000 (BR2) | 30000 |
| available | bool | Mặc định true (BR4) | true |
| updated_at | datetime | Hệ thống tự cập nhật, chỉ đọc | 2026-10-20T11:30:00 |

## 2. Danh sách endpoint

| Phương thức | URI | User story | Thành công | Lỗi |
| --- | --- | --- | --- | --- |
| GET | /menu-items | US1 (có query) | 200 | 422 |
| GET | /menu-items/{id} | US2 | 200 | 404, 422 |
| POST | /menu-items | US3 | 201 + Location | 409, 422 |
| PUT | /menu-items/{id} | US4 (toàn bộ) | 200 | 404, 409, 422 |
| PATCH | /menu-items/{id} | US4 (một phần) | 200 | 404, 409, 422 |
| DELETE | /menu-items/{id} | US5 | 204 | 404 |

Query params của `GET /menu-items`: `category`, `available`, `max_price`, `sort` (id, price, name), `order` (asc, desc).

Ví dụ: `/menu-items?category=do_uong&available=true&sort=price&order=desc`

## 3. Quy tắc nghiệp vụ và mã lỗi

| Mã | Quy tắc | Mã trả về khi vi phạm |
| --- | --- | --- |
| BR1 | Tên món duy nhất, không phân biệt hoa thường | 409 Conflict |
| BR2 | Giá > 0, ≤ 200.000, là bội số của 1.000 | 422 |
| BR3 | Loại món thuộc com, bun_pho, do_uong, an_vat | 422 |
| BR4 | Món tạm hết vẫn giữ trong thực đơn, chỉ đặt available = false | — |

Ngoài ra: id không tồn tại trả 404; id không hợp lệ (≤ 0) trả 422.