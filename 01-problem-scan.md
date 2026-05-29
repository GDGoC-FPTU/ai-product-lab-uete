# 01 — Problem Scan: AI Product Scoping

## Phase 1 — SCAN

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | Tốn thời gian | Cố vấn dịch vụ đọc mô tả lỗi xe điện, ảnh/video khách gửi và log app để phân loại lỗi pin/ADAS/điện trước khi đặt lịch sửa chữa; mất khoảng 15-20 phút/case. |
| 2 | **Xanh SM (GSM)** | Stakeholder Pain | Tài xế phản ánh điểm đón/trả trên bản đồ không khớp thực tế; điều phối viên phải gọi lại khách, hỏi mốc địa lý và chỉnh thủ công trong giờ cao điểm. |
| 3 | **Vinhomes** | AI-upgrade | Phân loại và route ticket cư dân trên App Vinhomes Resident (thang máy, nước, phí, an ninh, vệ sinh) hiện còn đọc tay và phản hồi mẫu, dễ trễ SLA. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ/điều dưỡng phải tổng hợp kết quả xét nghiệm, chẩn đoán, thuốc và dặn dò để viết tóm tắt ra viện/tái khám sau mỗi ca khám. |
| 5 | **Vinpearl / VinWonders** | Lặp lại | Nhân viên CSKH xử lý yêu cầu đổi ngày vé/phòng/combo do thời tiết hoặc lịch bay bằng cách kiểm tra nhiều hệ thống rồi soạn phản hồi gần giống nhau. |

## Phase 2 — QUICK-ASSESS

Chọn top 3 từ danh sách SCAN: **#3 (Vinhomes phân loại ticket cư dân), #4 (Vinmec tóm tắt hồ sơ ra viện/tái khám), #1 (VinFast triage lỗi xe điện trước lịch sửa chữa).**

## Quick Problem Card #1 — Vinhomes phân loại và route ticket cư dân

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Ticket cư dân gửi qua App Vinhomes Resident cần được phân loại, xác định mức ưu tiên, route đúng bộ phận và có phản hồi đầu tiên nhanh hơn. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | Nhân viên CSKH/Ban quản lý tòa nhà bị quá tải khi đọc ticket tự do; cư dân chờ phản hồi lâu và phải nhắc lại nhiều lần. |
| **Workflow thủ công hiện tại** | 1. Cư dân gửi mô tả + ảnh/video trên app -> 2. CSKH đọc nội dung và đoán nhóm vấn đề -> 3. Tra cứu quy định/SLA hoặc lịch đội kỹ thuật -> 4. Gán ticket cho kỹ thuật, an ninh, vệ sinh, kế toán phí -> 5. Soạn phản hồi đầu tiên cho cư dân. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2-4: đọc hiểu ticket, phân loại đúng bộ phận và tìm SLA phù hợp (10-12 phút/ticket, dễ route sai khi ticket có nhiều ý). |
| **AI hỗ trợ ở bước nào?** | AI tóm tắt ticket, phân loại intent, nhận diện mức khẩn cấp, đề xuất bộ phận xử lý và draft phản hồi đầu tiên; nhân viên CSKH bấm duyệt trước khi gửi. |
| **Metric có số** | Giảm thời gian xử lý phản hồi đầu tiên từ 12 phút xuống dưới 2 phút/ticket; >=90% ticket được phân loại đúng ngay lần đầu; 95% ticket thường có phản hồi đầu tiên dưới 30 phút. |
| **Quick Architecture** | [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent |

## Quick Problem Card #2 — Vinmec tạo nháp tóm tắt ra viện/tái khám

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Bác sĩ Vinmec cần bản nháp tóm tắt ra viện/tái khám từ hồ sơ bệnh án điện tử để giảm thời gian viết thủ công nhưng vẫn giữ bước duyệt y khoa. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  [x] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | Bác sĩ điều trị và điều dưỡng hành chính; bệnh nhân cũng bị chờ lâu ở bước hoàn tất hồ sơ sau khám/ra viện. |
| **Workflow thủ công hiện tại** | 1. Bác sĩ mở EMR sau ca khám/ra viện -> 2. Đọc diễn biến điều trị, kết quả xét nghiệm, chẩn đoán và thuốc -> 3. Viết tóm tắt, hướng dẫn dùng thuốc, dấu hiệu cần tái khám -> 4. Điều dưỡng nhập lịch hẹn/in giấy tờ -> 5. Bác sĩ kiểm tra và ký xác nhận. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2-3: tổng hợp nhiều nguồn dữ liệu và viết lại bằng ngôn ngữ dễ hiểu cho bệnh nhân (20-30 phút/bệnh nhân, dễ thiếu trường thông tin bắt buộc). |
| **AI hỗ trợ ở bước nào?** | AI tạo bản nháp tóm tắt từ EMR, kết quả xét nghiệm và đơn thuốc; đánh dấu trường còn thiếu để bác sĩ bổ sung; không tự động ký hoặc gửi cho bệnh nhân. |
| **Metric có số** | Giảm thời gian soạn tóm tắt từ 25 phút xuống dưới 6 phút/bệnh nhân; >=95% bản nháp có đủ các trường bắt buộc; 100% bản cuối phải được bác sĩ duyệt. |
| **Quick Architecture** | [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent |

## Quick Problem Card #3 — VinFast triage lỗi xe điện trước lịch sửa chữa

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Cố vấn dịch vụ VinFast cần phân loại nhanh lỗi xe điện từ mô tả khách hàng, lịch sử bảo dưỡng và log chẩn đoán để đặt đúng lịch, đúng kỹ thuật viên, đúng phụ tùng. |
| **Công ty thành viên** | [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | Cố vấn dịch vụ, kỹ thuật viên xưởng và khách hàng; lỗi mô tả mơ hồ khiến khách phải gọi lại nhiều lần hoặc đến xưởng nhưng thiếu phụ tùng. |
| **Workflow thủ công hiện tại** | 1. Khách gửi mô tả lỗi qua app/hotline kèm ảnh/video -> 2. Cố vấn gọi hỏi lại triệu chứng và thời điểm phát sinh -> 3. Mở lịch sử bảo dưỡng/log mã lỗi nếu có -> 4. Tra tài liệu kỹ thuật và phân loại mức ưu tiên -> 5. Đặt lịch xưởng, gán kỹ thuật viên/phụ tùng và báo khách. |
| **Bước tốn thời gian/lỗi nhất** | Bước 2-4: hỏi lại thông tin thiếu, đọc mã lỗi và đối chiếu tài liệu kỹ thuật (18-20 phút/case, dễ phân loại sai mức ưu tiên). |
| **AI hỗ trợ ở bước nào?** | Agent kéo dữ liệu từ CRM, lịch sử bảo dưỡng và log chẩn đoán; LLM tóm tắt triệu chứng, gợi ý câu hỏi còn thiếu, đề xuất nhóm lỗi và mức ưu tiên để cố vấn duyệt. |
| **Metric có số** | Giảm thời gian triage từ 20 phút xuống dưới 5 phút/case; tăng tỉ lệ đặt lịch đúng kỹ thuật viên/phụ tùng từ 80% lên >=92%; giảm case phải gọi lại vì thiếu thông tin xuống dưới 10%. |
| **Quick Architecture** | [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent |
