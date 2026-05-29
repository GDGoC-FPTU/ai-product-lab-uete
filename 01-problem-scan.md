# 01 — Problem Scan: AI Product Scoping

## Phase 1 — SCAN

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** | Tốn thời gian | Cố vấn dịch vụ đọc mô tả lỗi xe điện, ảnh và video khách gửi và log app để phân loại lỗi pin/ADAS/điện trước khi đặt lịch sửa chữa; mất khoảng 15-20 phút/case. |
| 2 | **Xanh SM (GSM)** | Stakeholder Pain | Tài xế phản ánh điểm đón, trả trên bản đồ không khớp thực tế; điều phối viên phải gọi lại khách, hỏi mốc địa lý và chỉnh thủ công trong giờ cao điểm. |
| 3 | **Vinhomes** | AI-upgrade | Trợ lý cư dân ảo hỗ trợ tra cứu quy định và draft hồ sơ đăng ký thi công nội thất mà không cần hỏi trực tiếp ban quản lý nhiều lần. |
| 4 | **Vinmec** | Tốn thời gian | Bác sĩ hoặc điều dưỡng phải tổng hợp kết quả xét nghiệm, chẩn đoán, thuốc và dặn dò để viết tóm tắt ra viện hoặc tái khám sau mỗi ca khám. |
| 5 | **Vinpearl / VinWonders** | Lặp lại | Nhân viên CSKH xử lý yêu cầu đổi ngày vé, phòng, combo do thời tiết hoặc lịch bay bằng cách kiểm tra nhiều hệ thống rồi soạn phản hồi gần giống nhau. |

## Phase 2 — QUICK-ASSESS

Chọn top 3 từ danh sách SCAN: **#3 (Vinhomes trợ lý đăng ký thi công nội thất), #4 (Vinmec tóm tắt hồ sơ ra viện/tái khám), #1 (VinFast triage lỗi xe điện trước lịch sửa chữa).**

## Quick Problem Card #1 — Vinhomes trợ lý đăng ký thi công nội thất

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Cư dân Vinhomes cần trợ lý ảo tra cứu thủ tục, hỏi thông tin còn thiếu và draft hồ sơ đăng ký thi công nội thất nhanh hơn. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khác |
| **Ai đang đau (Actor)?** | Cư dân phải hỏi đi hỏi lại về giấy tờ và quy định thi công; nhân viên ban quản lý, CSKH phải trả lời lặp lại và kiểm tra hồ sơ thiếu thông tin. |
| **Workflow thủ công hiện tại** | 1. Cư dân nhắn/gọi hỏi thủ tục -> 2. Ban quản lý tra quy định nội bộ -> 3. Giải thích giấy tờ cần chuẩn bị -> 4. Cư dân chuẩn bị hồ sơ và nộp lại -> 5. Ban quản lý kiểm tra hồ sơ, yêu cầu bổ sung nếu thiếu. |
| **Bước tốn thời gian/lỗi nhất** | Bước 3 và 5: cá nhân hóa checklist và kiểm tra hồ sơ thiếu/sai thông tin (18 phút/hồ sơ, dễ phải bổ sung nhiều lần). |
| **AI hỗ trợ ở bước nào?** | AI tra cứu knowledge base quy định thi công, hỏi thông tin còn thiếu, tạo checklist cá nhân hóa, draft form/hướng dẫn; ban quản lý duyệt trước khi gửi kết quả cuối. |
| **Metric có số** | Giảm thời gian hướng dẫn + kiểm tra hồ sơ từ 25 phút xuống dưới 5 phút/hồ sơ; tăng tỉ lệ hồ sơ đủ thông tin ngay lần đầu từ 55% lên >=85%; giảm lượt hỏi lại/bổ sung ít nhất 40%. |
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
