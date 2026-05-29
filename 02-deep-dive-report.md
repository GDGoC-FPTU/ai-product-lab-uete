# 02 — Deep-Dive Report: Vinhomes Ticket Routing Copilot

## Thông tin nhóm

| Mục | Nội dung |
|---|---|
| **Tên nhóm** | Khoa |
| **Thành viên** | Khoa / vkhoa2110 |
| **Công ty thành viên chọn phân tích** | Vinhomes |

## Quyết định lựa chọn

Nhóm chọn bài toán **Vinhomes phân loại và route ticket cư dân trên App Vinhomes Resident** để thực hiện deep-dive.

Lý do chọn:
- Tác vụ xuất hiện hằng ngày, có nhiều nội dung tiếng Việt tự do, phù hợp để LLM tóm tắt và phân loại.
- Rủi ro có thể kiểm soát bằng Human-in-the-loop: AI chỉ tạo nháp, nhân viên CSKH duyệt trước khi gửi hoặc route.
- Metric rõ: thời gian phản hồi đầu tiên, tỉ lệ route đúng, tỉ lệ ticket trễ SLA.

Không chọn Vinmec vì dữ liệu y tế nhạy cảm và yêu cầu kiểm định an toàn cao hơn. Không chọn VinFast vì triage kỹ thuật xe có rủi ro an toàn, cần dữ liệu mã lỗi và quy trình xưởng chuẩn hóa trước.

## 3.1. Current-State Workflow

Quy trình hiện tại khi cư dân gửi phản ánh lên App Vinhomes Resident:

```text
1. Cư dân gửi ticket trên app
   Input: mô tả tự do, ảnh/video, tòa/căn hộ
   Output: ticket mới
   Thời gian: 1 phút
   Handoff: Cư dân -> hệ thống app -> CSKH

2. Nhân viên CSKH đọc và hiểu nội dung
   Input: ticket thô
   Output: tóm tắt vấn đề
   Thời gian: 4 phút

3. CSKH phân loại nhóm vấn đề và mức ưu tiên
   Ví dụ: thang máy, nước, điện, an ninh, vệ sinh, phí, tiếng ồn
   Output: category + priority
   Thời gian: 5 phút
   Bottleneck: dễ sai khi ticket có nhiều ý hoặc mô tả thiếu rõ ràng

4. CSKH tra quy định/SLA và gán bộ phận xử lý
   Output: bộ phận nhận việc + deadline
   Thời gian: 4 phút
   Handoff: CSKH -> kỹ thuật/an ninh/vệ sinh/kế toán phí

5. CSKH soạn phản hồi đầu tiên cho cư dân
   Output: tin nhắn xác nhận đã tiếp nhận + thời gian dự kiến
   Thời gian: 5 phút
   Bottleneck: phản hồi dễ rập khuôn hoặc thiếu thông tin SLA

6. Bộ phận xử lý cập nhật trạng thái
   Output: trạng thái xử lý cho cư dân
   Thời gian CSKH theo dõi: 1-2 phút/ticket
```

**Tổng thời gian xử lý thủ công trước phản hồi đầu tiên: khoảng 18-20 phút/ticket.**

## 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH/Ban quản lý tòa nhà Vinhomes, người đọc ticket cư dân và route cho các bộ phận vận hành. |
| **2. Current Workflow** | Cư dân gửi ticket tự do qua app. CSKH đọc mô tả, xem ảnh/video, tự phân loại nhóm vấn đề, tra SLA/quy định nội bộ, gán bộ phận xử lý và viết phản hồi đầu tiên. Quy trình dùng app cư dân, dashboard ticket và tài liệu quy định nội bộ, mất khoảng 18-20 phút/ticket. |
| **3. Bottleneck** | Bước 3-5: phân loại đúng category, xác định mức ưu tiên, route đúng bộ phận và soạn phản hồi phù hợp. Đây là phần cần hiểu ngôn ngữ tự nhiên và bối cảnh tòa nhà nên rule cứng không đủ linh hoạt. |
| **4. Business Impact** | Nếu mỗi ngày có khoảng 300 ticket/cụm đô thị, phần đọc-route-phản hồi có thể tiêu tốn 90-100 giờ công/ngày. Route sai làm tăng handoff lại, cư dân phải nhắc nhiều lần, ảnh hưởng SLA và điểm hài lòng cư dân. |
| **5. Success Metric** | 1. Giảm thời gian phản hồi đầu tiên từ 18 phút xuống dưới 3 phút/ticket. 2. >=90% ticket được phân loại đúng ngay lần đầu. 3. Giảm ticket bị route lại do sai bộ phận xuống dưới 8%. 4. 95% ticket thường có phản hồi đầu tiên dưới 30 phút. |
| **6. Operational Boundary** | AI được phép tóm tắt ticket, đề xuất category/priority, đề xuất bộ phận xử lý và soạn phản hồi dạng nháp. AI không được tự gửi phản hồi, không được hứa bồi thường/miễn phí, không được kết luận lỗi pháp lý, không được tự đóng ticket. Ticket liên quan an ninh, tai nạn, cháy nổ, tranh chấp phí hoặc đe dọa an toàn phải chuyển nhân viên trực phê duyệt ngay. |

## 3.3. Future-State Flow & AI Fit

**AI Fit:** Chọn **LLM Feature kết hợp Rule/State-Machine**.

| Lựa chọn | Đánh giá |
|---|---|
| **No AI** | Không giải quyết được bottleneck đọc hiểu tiếng Việt tự do; CSKH vẫn mất nhiều thời gian. |
| **Rule / State-Machine** | Phù hợp để bắt keyword khẩn cấp, map category rõ ràng và kiểm tra SLA, nhưng yếu khi cư dân mô tả mơ hồ hoặc một ticket có nhiều ý. |
| **LLM Feature** | Phù hợp nhất cho MVP: tóm tắt, phân loại intent, phát hiện thông tin thiếu, draft phản hồi. |
| **Agentic Loop** | Chưa cần cho MVP vì tự động route/gửi/đóng ticket có rủi ro vận hành; nên để con người duyệt. |

Future-state flow:

```text
1. Cư dân gửi ticket
   -> App ghi nhận nội dung, ảnh/video, tòa/căn hộ

2. Rule pre-check
   -> Nếu có keyword khẩn cấp: cháy, tai nạn, mất an ninh, kẹt thang máy
   -> Gắn nhãn URGENT và đẩy nhân viên trực

3. AI Step: LLM tóm tắt và phân loại
   -> summary
   -> category
   -> priority
   -> missing_info
   -> suggested_team
   -> draft_reply

4. Human Step (HITL): CSKH review
   -> Duyệt/sửa category, priority, team, draft_reply
   -> Không cho phép gửi nếu chưa có người duyệt

5. Hệ thống route ticket
   -> Gửi sang kỹ thuật/an ninh/vệ sinh/kế toán phí
   -> Gửi phản hồi đầu tiên cho cư dân sau khi CSKH duyệt

6. Fallback
   -> Nếu AI confidence thấp, thiếu dữ liệu, hoặc output sai format
   -> Ticket quay về workflow thủ công và được gắn nhãn "manual_review"
```

## Phase 5 — EVALUATE

### AI Readiness Checklist

1. [x] Có dữ liệu mẫu/logs để test: ticket cư dân cũ, category đã xử lý, lịch sử route và thời gian SLA.
2. [x] Rủi ro khi AI sai nằm trong tầm kiểm soát: AI chỉ draft; CSKH duyệt trước khi gửi/route.
3. [x] Stakeholders có động lực thay đổi: CSKH giảm thời gian đọc-route, cư dân nhận phản hồi nhanh hơn, ban quản lý giảm ticket trễ SLA.

### Quyết định cuối cùng

[x] **GO (Bắt đầu xây dựng Prototype):** bắt đầu với scope hẹp cho 5 nhóm ticket phổ biến: thang máy, nước, điện, vệ sinh, an ninh.

[ ] **NOT YET:** cần thêm dữ liệu/xác lập baseline.

[ ] **NO-GO:** rule-based tốt hơn hoặc không khả thi.

### Justification

Nên GO với MVP có phạm vi hẹp vì bài toán có dữ liệu lịch sử, metric rõ và rủi ro kiểm soát được bằng HITL. LLM không được thay thế nhân viên CSKH mà chỉ giảm thời gian đọc hiểu, phân loại và soạn nháp.

Ước lượng chi phí kỹ thuật ban đầu:
- 1-2 tuần để gom 500-1.000 ticket mẫu và chuẩn hóa nhãn category/priority.
- 2-3 tuần xây prototype API: nhận ticket, gọi LLM, trả JSON, ghi log đánh giá.
- 1 tuần UAT với CSKH tại một cụm tòa.
- Chi phí inference có thể kiểm soát vì mỗi ticket ngắn; ưu tiên batch evaluation offline trước khi bật real-time.

Điều kiện GO:
- Không gửi tự động khi chưa có CSKH duyệt.
- Lưu đủ log input/output/decision để audit.
- Nếu confidence thấp hơn 0.75 hoặc ticket thuộc nhóm nhạy cảm, hệ thống bắt buộc chuyển manual review.
