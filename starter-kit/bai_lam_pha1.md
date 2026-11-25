# BÀI LÀM PHA 1 

- **Họ tên:** Trần Bảo Trân
- **Lớp:** CNTT2-KS26-HCM
- **Link repository Pha1:**

> Pha 1 (70 phút đầu, KHÔNG dùng AI). Viết xong, kéo thả file này vào repository Pha1 trước mốc giờ giám thị công bố.

## 4.1. Bảo vệ dữ liệu và chống ảo giác

### a) Ghi chú đã ẩn danh hoá

**GHI CHU DA AN DANH**

```text
Khán giả [TEN_KHACH] , [so_dien_thoai],[so_cccd], [email], đặt 3 vé gói SVT trận bán kết Cúp Sao Vàng ngày 18/12/2026 cho 2 người lớn và 1 trẻ em 9 tuổi. Khán giả đã chuyển khoản đặt cọc 1.500.000 đồng. Khán giả yêu cầu 3 ghế ngồi cạnh nhau gần lối đi.

```

### b) Hai câu lệnh chống ảo giác

**CAU LENH**

```text
•	– Câu 1: buộc AI chỉ trả lời dựa trên file đính kèm, và nêu AI phải trả lời thế nào nếu file không có thông tin.
•	– Câu 2: buộc AI ghi rõ câu trả lời lấy từ mục hoặc điều khoản nào trong file.

```

## 4.2. Trích xuất tin nhắn đặt vé ra JSON

**PROMPT**

```text
Bạn là hệ thống trích xuất dữ liệu đặt vé của khách hàng. Hãy phân tích tin nhắn và xuất ra định dạng JSON với các Trường sau: {"ma_goi:["VIP","FZ","SVT"]","ngay_su_kien:YYYY-MM-DD","nguoi_lon:[so_luong]","tre_em:[so_luong]","loai_ve:["VIP","FZ","SVT"]","ghi_chu:[khong_co_se_la_null]"}. 
Và làm đúng theo 5 quy tắc chuẩn hóa sau "
1. ma_goi lấy theo bảng mã gói:
"SVT – Sân Vận Động: vé trận đấu
FZ – Fan Zone: khu cổ vũ
VIP – Khán đài VIP"
2. ngay_su_kien dạng YYYY-MM-DD; tin nhắn không ghi năm thì lấy năm 2026; không có ngày cụ thể thì null.
3. Khách không nhắc trẻ em thì tre_em là 0 và mọi khách là người lớn.
4. loai_ve chỉ nhận "thuong", "vip" hoặc null.
5. ghi_chu chỉ ghi yêu cầu đặc biệt về chỗ ngồi, hỗ trợ di chuyển hoặc ăn uống; không có thì null.
"
Ví dụ:
"Tin nhắn mẫu 1:
Đặt giúp mình 2 vé VIP trận chung kết ngày 15/11/2026, 2 người lớn và 1 bé. Cần chỗ ngồi cạnh lối đi.
Kết quả đúng:
{"ma_goi": "VIP", "ngay_su_kien": "2026-11-15", "nguoi_lon": 2, "tre_em": 1, "loai_ve": "vip", "ghi_chu": "chỗ ngồi cạnh lối đi"}

Tin nhắn mẫu 2:
Cho mình một vé Fan Zone, ngày nào cũng được, mình đi một mình.
Kết quả đúng:
{"ma_goi": "FZ", "ngay_su_kien": null, "nguoi_lon": 1, "tre_em": 0, "loai_ve": "thuong", "ghi_chu": null}
"
Ba tin nhắn bạn cần xử lý là:
<tin_nhan>
TN1: Mình muốn đặt gói Sân Vận Động trận tứ kết ngày 20/12 nha, 2 người lớn với 1 bé 8 tuổi, vé thường.

TN2: Cho 3 người đi Fan Zone trận ngày 05/01/2027 ạ. Một người cần hỗ trợ xe lăn.

TN3: Trận chung kết Cúp Sao Vàng tháng 12 còn vé không ạ? Nhà em 4 người lớn, có một người khiếm thị cần hỗ trợ tìm chỗ, lấy vé VIP.
</tin_nhan>
```

## 4.3. Báo giá vé bằng Chain-of-Thought

### a) Prompt

**PROMPT**

```text
Bạn là hệ thống tính tổng tiền cho yêu cầu báo giá đây là bảng giá"
BẢNG GIÁ VÉ (đồng/người lớn)
SVT – Sân Vận Động: 1.200.000
FZ – Fan Zone: 800.000
VIP – Khán đài VIP: 2.500.000

QUY TẮC TÍNH GIÁ
1. Trẻ em từ 5 đến 11 tuổi tính 70% giá người lớn. Trẻ dưới 5 tuổi miễn phí.
2. Nhóm từ 5 người lớn trở lên được giảm 5% trên tiền vé (tiền người lớn + tiền trẻ em).
3. Phụ thu khu ghế ưu tiên: 300.000 đồng/người. Phụ thu không được giảm giá.
4. Tổng tiền = tiền vé sau giảm + phụ thu.

YÊU CẦU BÁO GIÁ
Nhóm cổ động viên đi xem trận bán kết, mua gói SVT, gồm 5 người lớn, 2 trẻ em (8 tuổi và 10 tuổi) và 1 bé 3 tuổi. Có 1 người lớn chọn khu ghế ưu tiên.
", quy tắc và yêu cầu báo giá vào cặp dấu phân tách; yêu cầu suy luận từng bước theo thứ tự tiền người lớn → tiền trẻ em → tiền vé → giảm giá nhóm → phụ thu → tổng tiền, mỗi bước ghi phép tính; dòng cuối có dạng [RESULT: <tổng tiền> đồng].
```

## 4.4. Dây chuyền xử lý khiếu nại (Prompt Chaining)

### a) Prompt Trạm 1

**PROMPT**

```text
Bạn là một trợ lý ảo chuyên xử lý khiếu nại về vé của giải bóng đá Cúp Sao Vàng. Hãy phân tích tin nhắn khiếu nại của khách hàng và xuất ra định dạng JSON với các trường sau: "Tên khách hàng", "Số điện thoại", "Loại vé", "Số lượng vé", "Ngày đặt vé", "Lý do khiếu nại". Nếu thông tin nào không có trong tin nhắn, hãy để giá trị là null. Trả về kết quả dưới dạng JSON.

    Trả về JSON dạng như này:
{
    "Tên khách hàng": null,
    "Số điện thoại": null,
    "Loại vé": null,
    "Số lượng vé": null,
    "Ngày đặt vé": null,
    "Lý do khiếu nại": null
}
Đơn khiếu nại:"
EMAIL KHIẾU NẠI CỦA KHÁN GIẢ

Tôi là khán giả mua vé gói SVT trận Cúp Sao Vàng ngày 02/11/2026, đi 2 người lớn. Ban tổ chức cam kết khu khán đài có mái che nhưng trong 2 hiệp đầu chúng tôi bị xếp sang khu không mái, trời mưa và ghế ướt. Nhân viên soát vé rất nhiệt tình, tôi không có gì phàn nàn về nhân viên. Tôi đề nghị ban tổ chức giải quyết thỏa đáng.

======================================================
CHÍNH SÁCH ĐỀN BÙ CỦA BAN TỔ CHỨC CÚP SAO VÀNG
======================================================

- tran_gio: trận đấu bắt đầu trễ trên 2 giờ do ban tổ chức: tặng voucher 200.000 đồng/khách.
- sai_khu_vuc: khán giả bị xếp sang khu vực thấp hơn quyền lợi đã cam kết: hoàn 500.000 đồng/khách cho mỗi hiệp bị ảnh hưởng.
- nhan_vien: khiếu nại về thái độ nhân viên: tặng voucher 300.000 đồng/khách.
- khac: chuyển bộ phận Chăm sóc khán giả xem xét, chưa đề xuất mức đền bù.
- Tổng mức đền bù trên 1.500.000 đồng phải được Trưởng ban Chăm sóc khán giả duyệt trước khi gửi khách."
```

### b) Hai điều cần kiểm tra ở JSON đầu ra của Trạm 1

1. Tính hợp lệ của các trường dữ liệu: Kiểm tra xem các trường : "Tên khách hàng","Số điện thoại","Loại vé","Số lượng vé","Ngày đặt vé",và "Lý do khiếu nại" có được đền đầy đủ và đúng định dạng hay không. Nếu có trường nào bị thiếu hoặc bị sai định dạng,cần thông báo lỗi và yêu cầu khách hàng cung cấp lại thông tin chính xác.

2. Xác thực thông tin đặt vé: Kiểm tra xem thông tin về "Loại vé",'Số lượng vé", và "Ngày đặt vé" có khớp vớiduwx liệu trong hệ thống của Cúp sao vàng hay không. Nếu thông tin không khớp cần thông báo lỗi và yêu cầu khách hàng cung cấp thông tin chính xác hoặc liên hệ bộ phận hỗ trợ xác minh

### c) Câu lệnh viết tiếp

**CAU LENH**

```text
Dựa trên dữ liệu JSON đầu ra từ Trạm 1, hãy thực hiện các bước 
1. Kiểm tra tính hợp lệ của các trường dữ liệu và xác thực thông tin đặt vé.
2. Nêu tất cả các trường dữ liệu hợp lệ và thông tin đặt vé khớp với dữ liệu trong hệ thống.
```

## 4.5. Prompt chấm bài theo Rubric

**PROMPT**

```text
Bạn là một trợ lý chuyên chấm bài theo Rubric. Hãy đọc kĩ thuật làm bài của học sinh và đánh giá dựa trên các tieu chí sau: 
1. Nội dung
```

## 4.6. Hoàn thiện System Prompt

> Phần Vai trò và Chuẩn đầu ra đã cho sẵn. Viết tiếp phần Quy tắc ràng buộc và Xử lý ngoại lệ.

```text
[Vai trò]
Bạn là "Trợ lý Cúp Sao Vàng", nhân viên tư vấn vé và dịch vụ trên website của giải bóng đá Cúp Sao Vàng, giúp khán giả chọn và đặt một trong 3 gói: SVT (Sân Vận Động – vé trận đấu), FZ (Fan Zone – khu cổ vũ), VIP (Khán đài VIP).

[Quy tắc ràng buộc]
1. (Viết quy tắc 1 – xoá dòng này)
2.
3.

[Chuẩn đầu ra]
Trả lời bằng tiếng Việt, lịch sự, tối đa 120 từ. Giá vé trình bày dạng gạch đầu dòng. Kết thúc bằng một câu hỏi về nhu cầu tiếp theo của khách.

[Xử lý ngoại lệ]
- Khách hỏi ngoài phạm vi: (Viết cách trả lời – xoá dòng này)
- Khách cung cấp thiếu thông tin:
```
