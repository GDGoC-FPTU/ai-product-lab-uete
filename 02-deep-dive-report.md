# 02 Deep-Dive Report — Vinhomes Resident Procedure Assistant

## Thông tin nhóm

- **Tên nhóm:** UETE
- **Thành viên:**
  - Trần Trung Kiên — 2a202600850
  - Lê Văn Khoa — 2a202600603
  - Lê Quang Hưng — 2a202600891
  - Nguyễn Văn Duy — 2a202600725

---

# 🗳️ Quyết định lựa chọn của nhóm

Nhóm quyết định chọn bài toán **Inspiration #7 — Vinhomes: Trợ lý cư dân ảo hỗ trợ thủ tục hành chính** để thực hiện Deep-Dive.

## Bài toán nhóm chọn

**Trợ lý cư dân ảo hỗ trợ cư dân Vinhomes tra cứu quy định và chuẩn bị hồ sơ đăng ký thi công nội thất.**

## Lý do lựa chọn và thu hẹp scope

Trong Quick Card ban đầu, bài toán #7 bao gồm nhiều thủ tục như đăng ký thi công nội thất và đăng ký vé gửi xe hằng tháng. Sau khi thảo luận theo hướng Product Scoping, nhóm quyết định **thu hẹp phạm vi prototype vào thủ tục đăng ký thi công nội thất** vì:

- Đây là thủ tục có nhiều bước, nhiều loại giấy tờ và dễ phát sinh thiếu sót hồ sơ.
- Cư dân thường hỏi bằng ngôn ngữ tự nhiên, không theo form cố định, nên LLM có giá trị trong việc diễn giải và hỏi lại thông tin thiếu.
- Ban quản lý phải trả lời lặp lại các câu hỏi giống nhau, gây tốn thời gian vận hành.
- Rủi ro có thể kiểm soát bằng Human-in-the-loop: AI chỉ hướng dẫn/draft checklist, không tự phê duyệt hồ sơ.

Các thủ tục khác như vé gửi xe tháng có thể triển khai sau bằng rule-based form hoặc checklist cố định.

---

# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow Mapping

Quy trình hiện tại khi cư dân Vinhomes muốn đăng ký thi công nội thất thường diễn ra như sau:

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân hỏi   │     │ Nhân viên BQL│     │ Nhân viên BQL│     │ Cư dân chuẩn │
│ thủ tục qua  │ ──→ │ tra cứu quy  │ ──→ │ giải thích   │ ──→ │ bị hồ sơ và  │
│ app/quầy     │     │ định nội bộ  │     │ giấy tờ cần  │     │ nộp lại      │
│              │     │              │     │ chuẩn bị     │     │              │
│ Ai: Cư dân   │     │ Ai: BQL      │     │ Ai: BQL      │     │ Ai: Cư dân   │
│ ⏱ 2 phút     │     │ ⏱ 5 phút 🔴  │     │ ⏱ 8 phút 🔴  │     │ ⏱ 1-2 ngày   │
│ In: Câu hỏi  │     │ In: Quy định │     │ In: Case cụ  │     │ In: Checklist│
│ Out: Yêu cầu │     │ Out: Mục liên│     │ thể cư dân   │     │ Out: Hồ sơ   │
│              │     │ quan         │     │ cần làm      │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ┌──────────────┐
                                                               │ Bước 5       │
                                                               │ BQL kiểm tra │
                                                               │ hồ sơ, yêu   │
                                                               │ cầu bổ sung  │
                                                               │ nếu thiếu    │
                                                               │ Ai: BQL      │
                                                               │ ⏱ 10 phút 🔴 │
                                                               └──────────────┘
```

**Ký hiệu:**

- 🔴 **Bottleneck:** Bước 2, 3 và 5. Nhân viên ban quản lý phải tra cứu quy định, giải thích lại thủ tục và kiểm tra hồ sơ thiếu/sai nhiều lần.
- 🔄 **Handoff:**
  - Cư dân → App/quầy lễ tân/ban quản lý.
  - Ban quản lý → Cư dân qua tin nhắn/email/hướng dẫn trực tiếp.
  - Cư dân → Ban quản lý khi nộp hồ sơ.

**Tổng thời gian vận hành trung bình:**

- Với một lượt hỏi thủ tục đơn giản: khoảng **15 phút/lượt hỏi** của nhân viên ban quản lý.
- Với một hồ sơ thiếu giấy tờ: có thể kéo dài thêm **1-2 ngày** do cư dân phải bổ sung và nộp lại.

---

## 3.2. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Nhân viên ban quản lý Vinhomes phụ trách hướng dẫn cư dân về thủ tục đăng ký thi công nội thất; cư dân là người cần chuẩn bị hồ sơ đúng ngay từ đầu. |
| **2. Current Workflow** | Cư dân hỏi qua app/quầy lễ tân về thủ tục thi công nội thất. Nhân viên ban quản lý tra cứu quy định nội bộ, giải thích danh sách giấy tờ cần chuẩn bị, gửi biểu mẫu, sau đó kiểm tra hồ sơ cư dân nộp. Nếu hồ sơ thiếu hoặc sai, nhân viên phải phản hồi lại và cư dân bổ sung nhiều vòng. |
| **3. Bottleneck** | Bước tra cứu quy định, diễn giải thủ tục theo từng trường hợp và kiểm tra hồ sơ thiếu/sai. Đây là tác vụ lặp lại, tốn khoảng 10-15 phút/lượt hỏi và dễ phát sinh sai sót nếu quy định thay đổi hoặc cư dân mô tả không rõ. |
| **4. Business Impact** | Ban quản lý mất nhiều thời gian cho các câu hỏi lặp lại, làm chậm SLA phản hồi cư dân. Cư dân phải điền/nộp lại hồ sơ nhiều lần, gây trải nghiệm không tốt trước khi thi công. Nếu mỗi ngày có 30 lượt hỏi thủ tục và mỗi lượt tốn trung bình 12 phút, BQL mất khoảng 6 giờ làm việc/ngày chỉ để hướng dẫn thủ tục. |
| **5. Success Metric** | 1. 80% câu hỏi thủ tục thi công nội thất phổ biến được AI trả lời/draft checklist trong dưới 1 phút.<br>2. Giảm thời gian nhân viên BQL xử lý một lượt hỏi từ 12 phút xuống dưới 3 phút.<br>3. Giảm tỷ lệ hồ sơ phải bổ sung do thiếu giấy tờ từ 30% xuống dưới 10%.<br>4. 100% hồ sơ/khuyến nghị nhạy cảm vẫn cần nhân viên BQL duyệt trước khi gửi hoặc phê duyệt. |
| **6. Operational Boundary** | AI được phép: tóm tắt câu hỏi của cư dân, hỏi thêm thông tin còn thiếu, tra cứu trong bộ quy định đã duyệt, draft checklist hồ sơ và draft câu trả lời cho nhân viên BQL/cư dân. **AI tuyệt đối không được:** tự phê duyệt hồ sơ thi công, tự cam kết thời gian cấp phép, tự tạo quy định mới, tư vấn về phí/phạt/tranh chấp ngoài tài liệu đã duyệt, hoặc gửi phản hồi chính thức khi chưa có nhân viên BQL xác nhận. |

---

## 3.3. Future-State Flow & AI Fit

### AI Fit Matrix

Giải pháp phù hợp nhất là **LLM Feature có Retrieval từ tài liệu quy định đã duyệt**, kết hợp với **Human-in-the-loop**.

| Lựa chọn | Đánh giá |
|---|---|
| **Rule / State-Machine** | Phù hợp cho các checklist cố định như danh sách giấy tờ bắt buộc, điều kiện form đầy đủ/chưa đầy đủ. Tuy nhiên rule thuần khó xử lý câu hỏi tự nhiên của cư dân và các trường hợp diễn đạt khác nhau. |
| **LLM Feature** | Phù hợp nhất cho prototype vì cư dân thường hỏi bằng tiếng Việt tự nhiên. LLM có thể tóm tắt nhu cầu, hỏi lại thông tin thiếu, diễn giải quy định thành checklist dễ hiểu và draft phản hồi thân thiện. |
| **Agentic Loop** | Chưa cần thiết trong scope đầu. Không nên để AI tự thực hiện nhiều hành động như phê duyệt hồ sơ, gửi thông báo chính thức hoặc cập nhật hệ thống cư dân vì rủi ro vận hành và pháp lý cao. |

**Kết luận AI Fit:** Chọn **LLM Feature + Rule Checklist + Human-in-the-loop**.

### Future-State Flow

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Cư dân nhập  │     │ 🔵 AI tóm tắt│     │ 🔵 AI tra cứu│     │ 🔵 AI draft  │
│ câu hỏi trên │ ──→ │ nhu cầu và   │ ──→ │ quy định đã  │ ──→ │ checklist hồ │
│ app/quầy     │     │ hỏi thông tin│     │ được duyệt   │     │ sơ cần nộp   │
│              │     │ còn thiếu    │     │              │     │              │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
┌──────────────┐     ┌──────────────┐                         ┌──────────────┐
│ Bước 6       │     │ Bước 5       │                         │ ↩️ Fallback  │
│ Gửi hướng dẫn│ ←── │ 🟢 Nhân viên │ ←────────────────────── │ Nếu AI không │
│ chính thức   │     │ BQL duyệt/   │                         │ tự tin hoặc  │
│ cho cư dân   │     │ chỉnh sửa    │                         │ câu hỏi nhạy │
│              │     │ draft        │                         │ cảm          │
└──────────────┘     └──────────────┘                         └──────────────┘
```

### Human-in-the-loop (HITL)

Nhân viên ban quản lý là người duyệt cuối cùng trước khi hướng dẫn được gửi chính thức cho cư dân. AI chỉ tạo bản nháp và checklist, không có quyền tự phê duyệt hồ sơ hoặc tự gửi quyết định hành chính.

### Fallback

Nếu AI gặp một trong các tình huống sau, hệ thống phải chuyển sang nhân viên BQL xử lý thủ công:

- Câu hỏi liên quan đến phí phạt, tranh chấp giữa cư dân và nhà thầu, khiếu nại về quy định.
- Cư dân yêu cầu cam kết ngày được phê duyệt hoặc yêu cầu bỏ qua một giấy tờ bắt buộc.
- Tài liệu quy định không có thông tin phù hợp.
- AI confidence thấp hoặc phát hiện thiếu thông tin quan trọng như mã căn hộ, loại thi công, thời gian thi công dự kiến.
