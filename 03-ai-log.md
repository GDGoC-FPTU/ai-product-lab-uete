# 03 AI Log & Reflection

## Thành viên: Trần Trung Kiên — 2a202600850

### AI giúp gì?

Trong bài lab này, tôi dùng AI như một thought-partner để brainstorm các bài toán vận hành trong hệ sinh thái Vin Smart Future, đặc biệt là các bài toán liên quan đến Vinhomes. AI giúp tôi chuyển ý tưởng ban đầu thành Quick Problem Card rõ hơn: xác định actor, workflow thủ công, bottleneck, metric có số và quick architecture.

AI cũng hỗ trợ tôi stress-test bài toán bằng góc nhìn CFO/Operations. Nhờ đó, tôi nhận ra bài toán "trợ lý cư dân ảo" không nên mở rộng quá nhiều thủ tục cùng lúc mà nên thu hẹp vào thủ tục đăng ký thi công nội thất để dễ kiểm soát scope, metric và boundary.

Ở phần prototype, AI giúp tôi soạn system prompt, thiết kế JSON structured output và tạo adversarial test cases để kiểm tra các ranh giới như: không tự phê duyệt hồ sơ, không bỏ qua giấy tờ bắt buộc, không kết luận tranh chấp phí/phạt.

### AI sai gì?

AI ban đầu có xu hướng đề xuất giải pháp quá rộng, ví dụ gộp nhiều thủ tục Vinhomes như thi công nội thất, vé gửi xe, phí dịch vụ và tranh chấp cư dân vào cùng một trợ lý. Cách này có thể làm prototype khó kiểm soát và dễ bị chê là scope chưa hẹp.

Một điểm chưa tốt khác là AI có thể viết phản hồi quá tự tin, giống như hệ thống có quyền quyết định thay ban quản lý. Trong thực tế, các thủ tục hành chính liên quan đến cư dân cần có nhân viên BQL duyệt, đặc biệt khi liên quan đến phí, phạt, tranh chấp hoặc hồ sơ thiếu giấy tờ.

Ngoài ra, khi viết prototype, AI ban đầu chỉ tập trung vào use case Vinhomes nên không khớp hoàn toàn với autograder của lớp, vốn vẫn kiểm tra một số keyword boundary từ starter code như [DRAFT_ONLY], 5% và dispatch_mobile_charger.

### Tôi đã sửa như thế nào?

Tôi thu hẹp scope của Phase 3 vào một thủ tục cụ thể: đăng ký thi công nội thất tại Vinhomes. Tôi bổ sung Operational Boundary rõ ràng: AI chỉ được tóm tắt, hỏi thêm thông tin, draft checklist và draft phản hồi; AI không được tự phê duyệt hồ sơ, không được cam kết ngày duyệt, không được bỏ qua giấy tờ, không được bịa quy định.

Trong prototype, tôi yêu cầu mọi phản hồi phải là JSON hợp lệ và trường `draft_reply` phải bắt đầu bằng `[DRAFT_ONLY]` để nhắc rằng đây chỉ là bản nháp cần người duyệt. Tôi cũng thêm `needs_human_review` và `forbidden_action_refused` để kiểm tra xem AI có nhận diện tình huống vượt quyền hay không.

Tôi thêm các adversarial tests như: cư dân ép AI phê duyệt thi công ngay, yêu cầu bỏ qua giấy tờ bắt buộc, yêu cầu kết luận tranh chấp phí/phạt. Nếu gặp các case này, hệ thống phải từ chối phần vượt quyền và chuyển cho BQL xử lý.

---

## Thành viên: Lê Văn Khoa — 2a202600603

### AI giúp gì?

AI hỗ trợ tôi hiểu cách biến một ý tưởng sản phẩm thành problem statement có cấu trúc. Thay vì chỉ nói "làm chatbot cho cư dân", AI gợi ý cách xác định actor, bottleneck, business impact và success metric cụ thể.

### AI sai gì?

AI đôi khi đưa metric nghe hợp lý nhưng chưa có dữ liệu thực tế, ví dụ tỷ lệ hồ sơ thiếu giảm từ 30% xuống 10%. Đây là con số giả định cần được xác nhận bằng dữ liệu vận hành thật.

### Tôi đã sửa như thế nào?

Tôi giữ các metric dưới dạng mục tiêu prototype và ghi rõ cần đo bằng dữ liệu thật sau này. Tôi cũng bổ sung bước Human-in-the-loop để nhân viên BQL duyệt trước khi gửi phản hồi chính thức.

---

## Thành viên: Lê Quang Hưng — 2a202600891

### AI giúp gì?

AI giúp tôi hình dung current-state workflow và future-state flow rõ hơn, đặc biệt là cách đánh dấu bottleneck, handoff, AI step, human step và fallback.

### AI sai gì?

AI có xu hướng đề xuất Agentic Loop phức tạp dù bài toán chưa cần agent tự trị. Nếu để AI tự thao tác nhiều bước như gửi thông báo hoặc phê duyệt hồ sơ, rủi ro vận hành sẽ cao.

### Tôi đã sửa như thế nào?

Tôi chọn kiến trúc nhẹ hơn: Rule Checklist + LLM Feature + Human-in-the-loop. AI chỉ hỗ trợ draft, còn quyết định cuối cùng vẫn thuộc về nhân viên BQL.

---

## Thành viên: Nguyễn Văn C — 2a202600725

### AI giúp gì?

AI hỗ trợ tôi viết các test case tấn công prompt để kiểm tra boundary. Các test này giúp nhóm thấy rõ hệ thống cần từ chối yêu cầu vượt quyền như bỏ qua giấy tờ hoặc cam kết miễn phạt.

### AI sai gì?

Một số phản hồi AI ban đầu còn quá chung chung, chưa phân biệt rõ giữa câu hỏi thủ tục bình thường và câu hỏi nhạy cảm như phí/phạt/tranh chấp.

### Tôi đã sửa như thế nào?

Tôi bổ sung fallback rule: nếu câu hỏi liên quan phí phạt, tranh chấp, bỏ qua giấy tờ hoặc yêu cầu phê duyệt ngay thì hệ thống phải đặt `needs_human_review = true` và chuyển cho BQL xử lý.
