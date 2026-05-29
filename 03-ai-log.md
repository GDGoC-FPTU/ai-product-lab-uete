# 03 — AI Log & Reflection

Trong lab này, tôi dùng AI như một thought-partner để brainstorm các pain point vận hành của Vingroup, đặc biệt ở Vinhomes, Vinmec và VinFast. AI giúp tôi chuyển các ý tưởng còn chung chung thành problem card có đủ actor, workflow thủ công, bottleneck, bước AI có thể hỗ trợ và metric đo thành công.

AI hữu ích nhất ở bước phản biện. Khi tôi đưa bài toán "phân loại ticket cư dân Vinhomes", AI gợi ý rằng không nên để hệ thống tự động gửi phản hồi hoặc tự đóng ticket, vì ticket có thể liên quan đến tranh chấp phí, an ninh, cháy nổ hoặc khiếu nại nhạy cảm. Từ đó tôi bổ sung Human-in-the-loop và fallback manual review vào boundary.

Một điểm AI trả lời chưa tốt là ban đầu nó đề xuất dùng Agent tự động đọc ticket, route ticket, gửi phản hồi và cập nhật trạng thái cho cư dân. Cách này nghe hiện đại nhưng rủi ro cao vì AI có thể route sai bộ phận, hứa sai SLA hoặc xử lý nhầm ticket khẩn cấp. Tôi sửa prompt bằng cách yêu cầu AI chỉ được tạo nháp, bắt buộc trả output có `confidence`, `missing_info`, `suggested_team`, và không được gửi phản hồi nếu chưa có CSKH duyệt.

Sau khi điều chỉnh, hướng giải pháp hợp lý hơn là LLM Feature kết hợp rule-based guardrail: rule dùng để bắt trường hợp khẩn cấp và kiểm tra SLA; LLM dùng để tóm tắt, phân loại và draft phản hồi. Bài học chính là không chọn AI vì muốn dùng AI, mà phải giới hạn AI vào đúng bước có giá trị và có ranh giới vận hành rõ.
