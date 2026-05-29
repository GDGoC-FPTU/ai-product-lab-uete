# Phase 1 - SCAN: Tìm kiếm cơ hội (Cá nhân)

Dùng **4 Lenses** quét qua vận hành của các công ty thành viên Vingroup. Tôi chọn tập trung vào **Vinmec** vì các quy trình y tế có nhiều bước thủ công, có rủi ro sai sót rõ và có thể đo hiệu quả bằng thời gian xử lý, tỷ lệ lỗi hoặc số ca cần con người review.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Vinmec** | Lặp lại | Điều dưỡng đọc file PDF/giấy in từ máy xét nghiệm rồi nhập từng chỉ số như WBC, RBC, HGB, glucose vào phần mềm HIS. Mỗi phiếu có 20-30 chỉ số, gây tốn khoảng 8-12 phút/phiếu và dễ sai số nhập liệu. |
| 2 | **Vinmec** | Tốn thời gian | Bác sĩ mất 10-20 phút đọc toàn bộ bệnh sử, kết quả xét nghiệm, đơn thuốc cũ và ghi chú điều trị trước mỗi ca khám, làm giảm năng lực phục vụ trong giờ cao điểm. |
| 3 | **Vinmec** | AI-upgrade | Nhân viên tiếp nhận phải tự đọc triệu chứng ngắn của bệnh nhân để xếp hàng đợi khám theo mức độ khẩn cấp, trong khi các ca có dấu hiệu cần ưu tiên có thể bị xếp chung với ca nhẹ. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ tự soạn discharge summary và tóm tắt điều trị theo mẫu cứng, phải gom chẩn đoán, quá trình điều trị, thuốc xuất viện, hướng dẫn tái khám; mỗi bệnh nhân tốn 25-40 phút. |
| 5 | **Vinmec** | Stakeholder Pain | Điều phối viên lập lịch phòng mổ bằng Excel/bảng giấy, phải cân đối tay nghề bác sĩ, loại phòng mổ, thời gian dụng cụ tiệt trùng và kíp ekip; xung đột lịch gây hủy/đổi ca mổ phút chót. |

---

# Phase 2 - QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Vinmec nhập kết quả xét nghiệm vào HIS), #2 (Vinmec tóm tắt hồ sơ bệnh án trước khi khám), #3 (Vinmec phân loại ưu tiên hàng đợi khám).**

## Thẻ bài toán tiêu biểu: Card #1 - Vinmec Nhập kết quả xét nghiệm vào HIS

```text
QUICK PROBLEM CARD #1

Bài toán:
Điều dưỡng/nhân viên y tế đang phải nhập thủ công kết quả xét nghiệm từ PDF,
ảnh chụp hoặc giấy in vào phần mềm HIS, gây tốn thời gian và dễ sai số.

Công ty thành viên:
[x] Vinmec

Ai đang đau?
Điều dưỡng, nhân viên hành chính y tế, kỹ thuật viên phòng xét nghiệm,
và gián tiếp là bác sĩ chờ dữ liệu xét nghiệm sạch để ra quyết định.

Workflow thủ công hiện tại (5 bước):
1. Nhận kết quả xét nghiệm từ máy/PDF/bản in.
→ 2. Mở hồ sơ bệnh nhân trên HIS.
→ 3. Đọc từng chỉ số và gõ thủ công vào các trường tương ứng.
→ 4. Đối chiếu lại một số chỉ số quan trọng.
→ 5. Lưu kết quả và chuyển cho bác sĩ xem.

Bước nào tốn nhất?
Bước 3-4 (8-12 phút/phiếu). Lỗi thường gặp là nhập nhầm đơn vị,
nhầm dấu thập phân, hoặc bỏ sót chỉ số bất thường.

AI có thể nhảy vào hỗ trợ ở bước nào?
Bước 3-4: OCR/LLM trích xuất chỉ số, đơn vị, ngưỡng tham chiếu,
đánh dấu giá trị bất thường và yêu cầu con người xác nhận trước khi ghi vào HIS.

Đo thành công bằng gì (Metric có số)?
Giảm thời gian nhập liệu từ 8-12 phút/phiếu xuống dưới 2 phút/phiếu.
Độ chính xác trích xuất đạt >= 98% với mẫu biểu chuẩn.
100% kết quả bất thường được highlight để nhân viên review.

Quick Architecture:
[x] LLM Feature + OCR + Human-in-the-loop
```

---

## Thẻ bài toán tiêu biểu: Card #2 - Vinmec Tóm tắt hồ sơ bệnh án trước khi khám

```text
QUICK PROBLEM CARD #2

Bài toán:
Bác sĩ mất nhiều thời gian đọc lại bệnh sử, đơn thuốc cũ, kết quả xét nghiệm
và ghi chú điều trị trước mỗi ca khám.

Công ty thành viên:
[x] Vinmec

Ai đang đau?
Bác sĩ khám ngoại trú, điều dưỡng phòng khám, bệnh nhân đang chờ khám.

Workflow thủ công hiện tại (5 bước):
1. Bác sĩ mở hồ sơ bệnh nhân trên HIS/EMR.
→ 2. Đọc bệnh sử, tiền sử, lần khám gần nhất, đơn thuốc cũ.
→ 3. Kiểm tra các kết quả xét nghiệm/hình ảnh gần đây.
→ 4. Ghi chú các điểm cần hỏi thêm trong buổi khám.
→ 5. Bắt đầu khám và hỏi bệnh trực tiếp.

Bước nào tốn nhất?
Bước 2-4 (10-20 phút/ca khám). Rủi ro là bỏ sót thông tin quan trọng
nếu bệnh nhân có lịch sử điều trị dài hoặc phức tạp.

AI có thể nhảy vào hỗ trợ ở bước nào?
Bước 2-4: LLM tóm tắt hồ sơ thành các mục ngắn gồm lý do đến khám,
chẩn đoán cũ, thuốc đang dùng, kết quả bất thường và việc cần bác sĩ xác minh.

Đo thành công bằng gì (Metric có số)?
Giảm thời gian đọc hồ sơ từ 10-20 phút xuống dưới 5 phút/ca.
>= 90% bản tóm tắt được bác sĩ đánh giá "đủ dùng để chuẩn bị khám".
AI không được tự đưa ra chẩn đoán mới.

Quick Architecture:
[x] LLM Feature có ranh giới an toàn, bác sĩ là người quyết định cuối cùng.
```

---

## Thẻ bài toán tiêu biểu: Card #3 - Vinmec Phân loại ưu tiên hàng đợi khám

```text
QUICK PROBLEM CARD #3

Bài toán:
Nhân viên tiếp nhận phải phân loại mức độ ưu tiên khám dựa trên mô tả triệu chứng
ban đầu của bệnh nhân, dễ chậm hoặc bỏ sót ca cần ưu tiên.

Công ty thành viên:
[x] Vinmec

Ai đang đau?
Nhân viên tiếp nhận, điều dưỡng sàng lọc, bệnh nhân đang chờ khám.

Workflow thủ công hiện tại (5 bước):
1. Bệnh nhân mô tả lý do đến khám và triệu chứng chính.
→ 2. Nhân viên tiếp nhận ghi nhanh thông tin vào hệ thống.
→ 3. Điều dưỡng/nhân viên tự ước lượng mức độ ưu tiên.
→ 4. Xếp bệnh nhân vào hàng đợi phòng khám phù hợp.
→ 5. Điều chỉnh lại khi có phản ánh hoặc ca nặng lên.

Bước nào tốn nhất?
Bước 3-4 (3-5 phút/bệnh nhân). Lỗi nghiêm trọng là xếp nhầm ca có dấu hiệu
cần ưu tiên vào hàng đợi thông thường.

AI có thể nhảy vào hỗ trợ ở bước nào?
Bước 2-4: LLM phân tích triệu chứng từ đoạn mô tả ngắn, gợi ý mức ưu tiên,
đưa ra lý do và đánh dấu "red flags" cần điều dưỡng xem ngay.

Đo thành công bằng gì (Metric có số)?
Giảm thời gian phân hạng từ 3-5 phút xuống dưới 1 phút/bệnh nhân.
Giảm >= 30% ca bị xếp nhầm mức độ ưu tiên.
100% ca có dấu hiệu nguy hiểm được đẩy sang review của điều dưỡng.

Quick Architecture:
[x] LLM Feature + rule-based safety checklist + bắt buộc Human-in-the-loop.
```
