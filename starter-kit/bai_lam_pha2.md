# BÀI LÀM PHA 2 

- **Họ tên:**
- **Lớp:**
- **Link repository Pha2:**
- **Công cụ AI đã dùng (ChatGPT/Claude/Gemini):**

> Pha 2 (20 phút cuối). Mỗi mục: mở cuộc trò chuyện mới, gửi nguyên văn prompt đã viết trong bai_lam_pha1.md, dán câu trả lời và link chia sẻ vào đây. Xong, kéo thả file này vào repository Pha2.

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**KET QUA AI**

```text
[
  {
    "ma_goi": "SVT",
    "ngay_su_kien": "2026-12-20",
    "nguoi_lon": 2,
    "tre_em": 1,
    "loai_ve": "thuong",
    "ghi_chu": null
  },
  {
    "ma_goi": "FZ",
    "ngay_su_kien": "2027-01-05",
    "nguoi_lon": 3,
    "tre_em": 0,
    "loai_ve": null,
    "ghi_chu": "Một người cần hỗ trợ xe lăn"
  },
  {
    "ma_goi": null,
    "ngay_su_kien": null,
    "nguoi_lon": 4,
    "tre_em": 0,
    "loai_ve": "vip",
    "ghi_chu": "Một người khiếm thị cần hỗ trợ tìm chỗ"
  }
])
```

**Link trò chuyện:**

## 4.3. Báo giá vé bằng Chain-of-Thought

**KET QUA AI**

```text
Bước 1 – Tiền người lớn: 5 × 1.200.000 = 6.000.000 đồng

Bước 2 – Tiền trẻ em:
- Bé 8 tuổi và bé 10 tuổi (5 đến 11 tuổi) tính 70%: 1.200.000 × 70% = 840.000 đồng/bé
- 2 × 840.000 = 1.680.000 đồng
- Bé 3 tuổi (dưới 5 tuổi) miễn phí: 0 đồng
- Tổng tiền trẻ em: 1.680.000 đồng

Bước 3 – Tiền vé: 6.000.000 + 1.680.000 = 7.680.000 đồng

Bước 4 – Giảm giá nhóm: có 5 người lớn nên đủ điều kiện giảm 5% trên tiền vé.
- Số tiền giảm: 7.680.000 × 5% = 384.000 đồng
- Tiền vé sau giảm: 7.680.000 − 384.000 = 7.296.000 đồng

Bước 5 – Phụ thu: 1 người × 300.000 = 300.000 đồng (không được giảm giá)

Bước 6 – Tổng tiền: 7.296.000 + 300.000 = 7.596.000 đồng

[RESULT: 7.596.000 đồng]
```

**Link trò chuyện:**

### b) Kiểm tra kết quả AI
``

Đúng

``
## 4.4. Dây chuyền xử lý khiếu nại – 
kết quả Trạm 1

**KET QUA AI**

```text

{
    "Tên khách hàng": null,
    "Số điện thoại": null,
    "Loại vé": "SVT",
    "Số lượng vé": 2,
    "Ngày đặt vé": null,
    "Lý do khiếu nại": "sai_khu_vuc"
}

Đền bù theo chính sách:

Loại khiếu nại: sai_khu_vuc. Khách được cam kết khu có mái che nhưng bị xếp sang khu không mái trong 2 hiệp đầu.
Phép tính: 500.000 đồng × 2 khách × 2 hiệp = 2.000.000 đồng
Tổng 2.000.000 đồng vượt 1.500.000 đồng, nên phải được Trưởng ban Chăm sóc khán giả duyệt trước khi gửi khách.
```

**Link trò chuyện:**

## 4.5. Prompt chấm bài theo Rubric

**KET QUA AI**

```text
(Dán câu trả lời của AI vào đây (bấm nút Copy dưới câu trả lời) – xoá dòng này)
```

**Link trò chuyện:**
