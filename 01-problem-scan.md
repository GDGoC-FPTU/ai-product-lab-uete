# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)

Hãy sử dụng **4 Lenses** dưới đây để quét qua hoạt động vận hành của các công ty thành viên Vingroup. Ghi lại **ít nhất 5 bài toán/bottleneck** thực tế.

### 4 Lenses tìm bài toán AI cho Vingroup:
1. **Lặp lại (Repetitive):** Tác vụ lặp đi lặp lại nhiều lần hằng ngày. (Ví dụ: So khớp hóa đơn sạc điện tại VinFast, route lại chuyến taxi tại Xanh SM).
2. **Tốn thời gian (Time-consuming):** Tác vụ ngốn thời gian xử lý thủ công của nhân viên. (Ví dụ: Soạn thảo phản hồi đánh giá 1-star của cư dân Vinhomes).
3. **AI có thể tốt hơn (AI-upgrade):** Dịch vụ khách hàng hiện tại còn chậm hoặc phản hồi rập khuôn. (Ví dụ: Chatbot CSKH Vinpearl hỗ trợ đặt vé vui chơi).
4. **Pain từ người khác (Stakeholder Pain):** Bottleneck khiến khách hàng hoặc nhân viên thực địa phàn nàn. (Ví dụ: Tài xế Xanh SM phàn nàn về việc hệ thống gợi ý điểm đón khách không chính xác).

> [!TIP]
> **🤖 AI Prompts — Partner brainstorm:**
> Hãy sử dụng prompt sau để brainstorm các bài toán thực tế nếu bạn chưa có ý tưởng:
> *"Tôi là AI Engineer tại Vin Smart Future (Vingroup). Tôi đang tìm kiếm các pain point vận hành cụ thể có thể tối ưu bằng AI cho mảng [Chọn một: VinFast / Xanh SM / Vinhomes / Vinmec]. Hãy gợi ý cho tôi 5 quy trình nghiệp vụ thủ công, tốn nhiều thời gian và gây rò rỉ hiệu suất kèm con số thống kê ước tính về tổn thất."*

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 2 | VinFast | AI có thể tốt hơn | Tự động đề xuất lịch trình sạc tối ưu và trạm sạc trống phù hợp với loại cổng sạc (CCS2/GBT) của từng dòng xe điện (VF5, VF8, VF9). |
| 3 | VinFast | Lặp lại | So khớp dữ liệu sạc điện hằng tuần từ hàng nghìn trụ sạc liên kết ngoài với hóa đơn thực tế gửi về hệ thống tài chính. |
| 4 | Xanh SM | Pain từ người khác | Tự động nghe ghi âm cuộc gọi hủy chuyến và ghi chú của tài xế để phân loại 10 lý do phổ biến nhất gây rò rỉ cuốc. |
| 7 | Vinhomes | AI có thể tốt hơn | Hỗ trợ cư dân tra cứu và draft nhanh hồ sơ đăng ký thi công nội thất, đăng ký vé gửi xe hằng tháng mà không cần gặp trực tiếp ban quản lý. |
| 12 | VinUni | Lặp lại | Hệ thống chấm code autograder, tự động dùng LLM để phân tích lỗi cú pháp/logic và draft phản hồi mang tính sư phạm hỗ trợ sinh viên học tập. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1 — Inspiration #3                     │
│                                                             │
│ Bài toán (1 câu): Tự động đối chiếu dữ liệu sạc điện đối    │
│ tác với hóa đơn thực tế gửi về hệ thống tài chính VinFast.  │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên tài chính/kế toán vận hành   │
│ trạm sạc và đối tác thanh toán.                             │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Nhận file log sạc từ đối tác ──>                       │
│   2. Nhận hóa đơn/đối soát từ nhà cung cấp ──>              │
│   3. Mở Excel để so khớp mã trạm, thời gian, số kWh, tiền ──>│
│   4. Đánh dấu dòng lệch và gửi email hỏi lại đối tác.       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? So khớp từng dòng dữ liệu  │
│ và phát hiện chênh lệch bất thường (⏱ 4-6 giờ/tuần).        │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Tự động đọc bảng dữ   │
│ liệu, phát hiện mismatch, nhóm lỗi và draft email đối soát. │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian đối    │
│ chiếu từ 6 giờ xuống dưới 1 giờ/tuần; phát hiện ≥ 95% dòng  │
│ lệch cần kiểm tra thủ công.                                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2 — Inspiration #7                     │
│                                                             │
│ Bài toán (1 câu): Trợ lý cư dân ảo hỗ trợ tra cứu và draft  │
│ hồ sơ thủ tục hành chính tại Vinhomes.                      │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? Cư dân cần làm thủ tục và nhân viên    │
│ ban quản lý phải trả lời lặp lại nhiều câu hỏi giống nhau.  │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân hỏi qua app/quầy lễ tân ──>                     │
│   2. Nhân viên tra quy định/hồ sơ cần nộp ──>               │
│   3. Nhân viên hướng dẫn cư dân điền biểu mẫu ──>           │
│   4. Cư dân bổ sung giấy tờ thiếu và nộp lại.               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Tra cứu thủ tục và giải    │
│ thích hồ sơ cần chuẩn bị cho từng trường hợp (⏱ 10-15 phút │
│ /lượt hỏi).                                                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? LLM tra cứu theo bộ   │
│ quy định nội bộ, hỏi thêm thông tin còn thiếu và draft danh │
│ sách hồ sơ/biểu mẫu cần chuẩn bị.                           │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? 80% câu hỏi thủ tục   │
│ phổ biến được trả lời dưới 1 phút; giảm lượt cư dân phải    │
│ bổ sung hồ sơ từ 30% xuống dưới 10%.                        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3 — Inspiration #12                    │
│                                                             │
│ Bài toán (1 câu): Dùng LLM hỗ trợ autograder phân tích lỗi  │
│ code và draft phản hồi mang tính sư phạm cho sinh viên.     │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [x] Khác (Ghi rõ) VinUni   │
│                                                             │
│ Ai đang đau (Actor)? Giảng viên/trợ giảng phải đọc log lỗi  │
│ và giải thích nguyên nhân sai cho nhiều bài lab giống nhau. │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Sinh viên nộp code ──>                                 │
│   2. Autograder chạy test và xuất log pass/fail ──>         │
│   3. Trợ giảng đọc log lỗi, mở code liên quan ──>           │
│   4. Trợ giảng viết feedback ngắn cho sinh viên.            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Đọc log lỗi dài và chuyển  │
│ thành phản hồi dễ hiểu, không đưa thẳng đáp án (⏱ 5-8 phút │
│ /bài fail).                                                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? LLM tóm tắt lỗi, phân │
│ loại lỗi cú pháp/logic/test case và draft gợi ý học tập để  │
│ trợ giảng duyệt trước khi gửi.                              │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? Giảm thời gian viết   │
│ feedback từ 8 phút xuống dưới 2 phút/bài fail; ≥ 85% draft  │
│ được trợ giảng chấp nhận sau chỉnh sửa nhỏ.                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

### 🤖 Stress-Test Quick Problem Cards

#### Card #1 — VinFast: Đối chiếu hóa đơn sạc điện đối tác

**3 điểm yếu khi nhìn từ CFO/Operations:**
1. **Metric phát hiện ≥ 95% dòng lệch cần baseline rõ hơn:** cần định nghĩa thế nào là “dòng lệch” và có tập dữ liệu lịch sử đã được kế toán xác nhận để đo precision/recall hay chưa.
2. **Rủi ro tài chính nếu AI diễn giải sai:** nếu hệ thống tự động kết luận chênh lệch mà không có người kiểm tra, có thể gây tranh chấp thanh toán với đối tác hoặc bỏ sót khoản cần thu.
3. **Nguồn dữ liệu có thể không đồng nhất:** mỗi đối tác có thể gửi file với format, tên cột, đơn vị tính, múi giờ, mã trạm khác nhau; nếu không chuẩn hóa trước, AI dễ xử lý thiếu ổn định.

**Vì sao rule-based có thể tốt hơn AI:**
- Phần so khớp mã trạm, thời gian, số kWh và số tiền nên ưu tiên dùng rule-based/SQL vì cần tính chính xác, có thể audit và dễ giải thích.
- AI chỉ nên hỗ trợ phần khó chuẩn hóa như giải thích nguyên nhân lệch, nhóm lỗi bất thường, và draft email đối soát cho nhân viên duyệt.

**Điều chỉnh sau phản biện:**
- Kiến trúc phù hợp hơn là **Rule + LLM**: rule-based thực hiện reconciliation chính, LLM hỗ trợ tóm tắt/chú giải kết quả.
- Bổ sung Human-in-the-loop: nhân viên tài chính duyệt trước khi gửi email hoặc ghi nhận chênh lệch chính thức.

#### Card #2 — Vinhomes: Trợ lý cư dân ảo hỗ trợ thủ tục hành chính

**3 điểm yếu khi nhìn từ CFO/Operations:**
1. **Phạm vi thủ tục dễ bị quá rộng:** đăng ký thi công, vé gửi xe, thẻ cư dân, phí dịch vụ... có nhiều quy định khác nhau theo từng tòa/khu đô thị.
2. **Metric giảm hồ sơ bổ sung từ 30% xuống dưới 10% cần dữ liệu thật:** nếu chưa có thống kê hiện tại, con số này chỉ là giả định và khó chứng minh hiệu quả.
3. **Rủi ro trả lời sai quy định:** nếu AI hướng dẫn thiếu giấy tờ hoặc sai deadline, cư dân có thể mất thời gian và giảm niềm tin vào ban quản lý.

**Vì sao rule-based có thể tốt hơn AI:**
- Các thủ tục có checklist cố định có thể dùng form/rule-based decision tree để đảm bảo không thiếu giấy tờ.
- AI chỉ có lợi khi cư dân hỏi bằng ngôn ngữ tự nhiên, mô tả tình huống không theo mẫu, hoặc cần draft hướng dẫn dễ hiểu.

**Điều chỉnh sau phản biện:**
- Thu hẹp scope ban đầu vào 2 thủ tục phổ biến: đăng ký thi công nội thất và đăng ký vé gửi xe tháng.
- Dùng **LLM Feature có retrieval từ tài liệu quy định đã duyệt**, không để AI tự bịa quy định.
- Bổ sung fallback: nếu câu hỏi liên quan phí/phạt/tranh chấp, chuyển cho nhân viên ban quản lý.

#### Card #3 — VinUni: Autograder feedback cho bài lab

**3 điểm yếu khi nhìn từ CFO/Operations:**
1. **Nguy cơ AI đưa đáp án quá trực tiếp:** phản hồi có thể vô tình tiết lộ solution thay vì gợi ý học tập, làm giảm giá trị đánh giá cá nhân.
2. **Metric “85% draft được chấp nhận” cần tiêu chí chấm rõ:** trợ giảng chấp nhận vì nhanh hơn chưa chắc đồng nghĩa feedback đúng và có tính sư phạm.
3. **Log lỗi không đủ ngữ cảnh:** nếu chỉ có traceback/test output mà không có rubric hoặc mục tiêu bài lab, AI có thể suy luận sai nguyên nhân.

**Vì sao rule-based có thể tốt hơn AI:**
- Autograder rule-based vẫn là phần chính để chấm pass/fail vì nhất quán, tái lập được và công bằng.
- AI không nên thay điểm số; AI chỉ nên diễn giải lỗi và draft feedback để trợ giảng duyệt.

**Điều chỉnh sau phản biện:**
- Kiến trúc phù hợp là **Rule Autograder + LLM Feedback Assistant**.
- Operational Boundary: AI không được thay đổi điểm, không được tiết lộ full solution, không được khẳng định nguyên nhân nếu thiếu log.
- Bổ sung Human-in-the-loop: trợ giảng duyệt feedback trước khi gửi cho sinh viên.
