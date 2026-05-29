# 02 — Deep-Dive Report: Vinhomes Interior Renovation Assistant

## Thông tin nhóm

| Mục | Nội dung |
|---|---|
| **Tên nhóm** | UETE |
| **Thành viên** | Trần Trung Kiên — 2a202600850<br>Lê Văn Khoa — 2a202600603<br>Lê Quang Hưng — 2a202600891<br>Nguyễn Văn Duy — 2a202600725 |
| **Công ty thành viên chọn phân tích** | Vinhomes |

## Quyết định lựa chọn

Nhóm chọn bài toán **Vinhomes - Trợ lý cư dân ảo hỗ trợ đăng ký thi công nội thất** để thực hiện deep-dive.

Lý do chọn:
- Đây là thủ tục hành chính phổ biến ở các khu căn hộ, nhiều cư dân hỏi lặp lại về giấy tờ, quy định thi công, khung giờ làm việc, đặt cọc và hồ sơ cần chuẩn bị.
- Ban quản lý phải tra quy định nội bộ, giải thích từng trường hợp, gửi checklist, kiểm tra hồ sơ thiếu/sai và yêu cầu cư dân bổ sung nhiều lần.
- AI có thể hỗ trợ tốt ở phần tra cứu quy định, hỏi thông tin còn thiếu và draft checklist/hướng dẫn, nhưng quyết định duyệt thi công vẫn bắt buộc do ban quản lý thực hiện.

Nhóm thu hẹp scope vào **hướng dẫn và kiểm tra hồ sơ đăng ký thi công nội thất trước khi duyệt**, không tự động cấp phép thi công. Scope này đủ rõ để làm prototype và có ranh giới vận hành an toàn.

## 3.1. Current-State Workflow

Quy trình hiện tại khi cư dân Vinhomes muốn đăng ký thi công nội thất:

```text
1. Cư dân hỏi thủ tục qua app/quầy
   Input: câu hỏi tự do về thi công nội thất
   Output: yêu cầu tư vấn ban đầu
   Thời gian: 2 phút
   Handoff: Cư dân -> App/quầy -> Ban quản lý

2. Nhân viên ban quản lý tra cứu quy định nội bộ
   Input: tòa/căn hộ, loại hạng mục thi công, thời gian dự kiến
   Output: mục liên quan trong quy định thi công
   Thời gian: 5 phút
   Bottleneck: quy định có nhiều điều kiện theo tòa/khu và loại hạng mục

3. Nhân viên ban quản lý giải thích giấy tờ cần chuẩn bị
   Input: case cụ thể của cư dân
   Output: checklist giấy tờ, bản vẽ, cam kết, đặt cọc, thời gian được thi công
   Thời gian: 8 phút
   Bottleneck: phải cá nhân hóa hướng dẫn và dễ thiếu mục bắt buộc

4. Cư dân chuẩn bị hồ sơ và nộp lại
   Input: checklist từ ban quản lý
   Output: bộ hồ sơ thi công
   Thời gian chờ: 1-2 ngày
   Handoff: Cư dân -> Ban quản lý

5. Ban quản lý kiểm tra hồ sơ, yêu cầu bổ sung nếu thiếu
   Input: hồ sơ cư dân nộp
   Output: hồ sơ đủ điều kiện/chờ bổ sung
   Thời gian: 10 phút
   Bottleneck: phải đối chiếu nhiều trường và phát hiện thiếu/sai giấy tờ
```

**Tổng thời gian thao tác thủ công của ban quản lý: khoảng 25 phút/hồ sơ, chưa tính 1-2 ngày cư dân chuẩn bị và bổ sung hồ sơ.**

## 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên CSKH/Ban quản lý tòa nhà Vinhomes phụ trách hướng dẫn cư dân đăng ký thi công nội thất và kiểm tra hồ sơ trước khi trình duyệt. |
| **2. Current Workflow** | Cư dân hỏi thủ tục qua app/quầy. Nhân viên ban quản lý tra quy định nội bộ, giải thích giấy tờ cần chuẩn bị, gửi checklist, nhận hồ sơ, kiểm tra các trường bắt buộc và yêu cầu bổ sung nếu thiếu. Quy trình dùng app/quầy tiếp nhận, file quy định nội bộ, mẫu checklist và dashboard hồ sơ cư dân. |
| **3. Bottleneck** | Bước 3 và 5: giải thích đúng checklist theo case cụ thể và kiểm tra hồ sơ thiếu/sai. Hai bước này mất khoảng 18 phút/hồ sơ, dễ gây vòng lặp bổ sung vì cư dân không biết rõ cần chuẩn bị bản vẽ, cam kết, đặt cọc hay giấy tờ nhà thầu nào. |
| **4. Business Impact** | Nếu một cụm tòa có khoảng 50 hồ sơ thi công/tháng, ban quản lý mất hơn 20 giờ công/tháng chỉ cho hướng dẫn và kiểm tra lặp lại. Hồ sơ thiếu làm kéo dài thời gian duyệt, tăng số lượt trao đổi, gây bức xúc cho cư dân và làm quá tải quầy CSKH vào các giai đoạn bàn giao căn hộ. |
| **5. Success Metric** | 1. Giảm thời gian hướng dẫn + kiểm tra hồ sơ từ 25 phút xuống dưới 5 phút/hồ sơ. 2. Tăng tỉ lệ hồ sơ đủ thông tin ngay lần đầu từ 55% lên >=85%. 3. Giảm số lượt cư dân phải hỏi lại/bổ sung hồ sơ ít nhất 40%. 4. 95% yêu cầu thường có phản hồi đầu tiên dưới 10 phút. |
| **6. Operational Boundary** | AI được phép tra cứu quy định đã duyệt, hỏi cư dân thông tin còn thiếu, tạo checklist cá nhân hóa, draft hướng dẫn và draft form/hồ sơ cho nhân viên duyệt. AI không được tự phê duyệt thi công, không được cho phép thi công ngoài giờ, không được bỏ qua đặt cọc/cam kết, không được miễn/giảm phí phạt, không được tư vấn thay đổi kết cấu chịu lực hoặc hạng mục có rủi ro an toàn. Mọi kết quả cuối cùng phải có nhân viên ban quản lý duyệt. |

## 3.3. Future-State Flow & AI Fit

**AI Fit:** Chọn **LLM Feature + Rule/RAG validation**.

| Lựa chọn | Đánh giá |
|---|---|
| **No AI** | Không giảm được lượng câu hỏi lặp lại và vòng lặp hồ sơ thiếu thông tin. |
| **Rule / State-Machine** | Hữu ích để kiểm tra trường bắt buộc như căn hộ, loại hạng mục, thời gian thi công, giấy cam kết, đặt cọc. Tuy nhiên rule cứng không đủ linh hoạt khi cư dân hỏi bằng ngôn ngữ tự nhiên. |
| **LLM Feature** | Phù hợp nhất cho MVP: hiểu câu hỏi tiếng Việt, tra knowledge base, hỏi thông tin thiếu, draft checklist và hướng dẫn. |
| **Agentic Loop** | Chưa phù hợp ở MVP vì tự động duyệt hồ sơ thi công có rủi ro vận hành, an toàn tòa nhà và trách nhiệm pháp lý. |

Future-state flow:

```text
1. Cư dân nhập câu hỏi trên app/quầy
   -> App ghi nhận tòa/căn hộ, hạng mục thi công, thời gian dự kiến

2. AI Step: LLM tóm tắt nhu cầu và hỏi thông tin còn thiếu
   -> Loại hạng mục
   -> Thời gian thi công
   -> Nhà thầu
   -> Bản vẽ/hồ sơ hiện có

3. AI Step: Tra cứu quy định đã được duyệt
   -> Knowledge base quy định thi công nội thất Vinhomes
   -> Rule check cho hạng mục nhạy cảm, ngoài giờ, ảnh hưởng kết cấu

4. AI Step: Draft checklist hồ sơ cần nộp
   -> Danh sách giấy tờ
   -> Mẫu cam kết
   -> Lưu ý đặt cọc/phí
   -> Khung giờ thi công hợp lệ

5. Human Step (HITL): Nhân viên ban quản lý duyệt/chỉnh sửa draft
   -> Xác nhận quy định áp dụng
   -> Kiểm tra rủi ro an toàn
   -> Không cho phép AI tự phê duyệt

6. Gửi hướng dẫn chính thức cho cư dân
   -> Cư dân nhận checklist đã duyệt và nộp hồ sơ đầy đủ hơn

Fallback
   -> Nếu AI không tự tin, không tìm thấy quy định, hạng mục nhạy cảm hoặc có tranh chấp
   -> Chuyển manual_review cho ban quản lý xử lý thủ công
```

## Phase 5 — EVALUATE

### AI Readiness Checklist

1. [x] Có dữ liệu mẫu/logs để test: FAQ cư dân, quy định thi công nội thất, mẫu checklist, mẫu cam kết, lịch sử hồ sơ thiếu/sai.
2. [x] Rủi ro khi AI sai nằm trong tầm kiểm soát: AI chỉ draft checklist/hướng dẫn; ban quản lý duyệt trước khi gửi kết quả cuối cùng.
3. [x] Stakeholders có động lực thay đổi: cư dân muốn biết thủ tục nhanh, ban quản lý muốn giảm câu hỏi lặp lại và giảm hồ sơ thiếu thông tin.

### Quyết định cuối cùng

[x] **GO (Bắt đầu xây dựng Prototype):** bắt đầu với scope hẹp cho hướng dẫn hồ sơ đăng ký thi công nội thất tại một cụm tòa.

[ ] **NOT YET:** cần thêm dữ liệu/xác lập baseline.

[ ] **NO-GO:** rule-based tốt hơn hoặc không khả thi.

### Justification

Nên GO với MVP phạm vi hẹp vì bài toán có quy trình rõ, dữ liệu có thể chuẩn hóa và rủi ro kiểm soát được bằng HITL. AI không thay ban quản lý phê duyệt thi công, chỉ hỗ trợ tra cứu, hỏi thông tin thiếu và soạn nháp checklist/hướng dẫn.

Ước lượng chi phí kỹ thuật ban đầu:
- 1 tuần gom và chuẩn hóa knowledge base: quy định thi công, checklist giấy tờ, mẫu cam kết, quy định đặt cọc/phí.
- 1-2 tuần xây prototype: API nhận câu hỏi/hồ sơ, truy xuất quy định, gọi LLM trả JSON draft.
- 1 tuần UAT với ban quản lý tại một cụm tòa và đo baseline.
- Chi phí inference thấp vì mỗi case ngắn; ưu tiên log đầy đủ để đánh giá lỗi trước khi mở rộng.

Điều kiện GO:
- Không tự động duyệt hồ sơ thi công.
- Không cho phép thi công ngoài khung giờ/quy định đã duyệt.
- Không bỏ qua đặt cọc, cam kết hoặc phí phạt.
- Nếu confidence thấp hơn 0.75, không tìm thấy quy định hoặc hạng mục ảnh hưởng an toàn/kết cấu, bắt buộc chuyển manual review.
