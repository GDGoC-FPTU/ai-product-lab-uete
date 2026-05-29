# 03 — AI Log & Reflection

Trong lab này, nhóm dùng AI như một thought-partner để brainstorm các pain point vận hành của Vingroup, đặc biệt ở Vinhomes, Vinmec và VinFast. AI giúp tôi chuyển các ý tưởng còn chung chung thành problem card có đủ actor, workflow thủ công, bottleneck, bước AI có thể hỗ trợ và metric đo thành công.

AI hữu ích nhất ở bước làm rõ scope. Với chủ đề "trợ lý cư dân ảo hỗ trợ thủ tục hành chính", AI giúp nhóm bóc tách riêng quy trình **đăng ký thi công nội thất** thay vì mô tả chung chung nhiều thủ tục hành chính. Sau khi đối chiếu với flow nhóm đang vẽ, tôi chọn scope này vì quy trình có nhiều bước thủ công rõ ràng: cư dân hỏi thủ tục, ban quản lý tra quy định, giải thích giấy tờ, cư dân nộp hồ sơ và ban quản lý kiểm tra bổ sung.

Một điểm AI trả lời chưa tốt là ban đầu nó đề xuất để trợ lý tự kiểm tra hồ sơ và tự phê duyệt cho cư dân được thi công. Cách này vượt quá quyền vận hành vì thi công nội thất có rủi ro về khung giờ, đặt cọc, cam kết, an toàn tòa nhà và các hạng mục có thể ảnh hưởng kết cấu. Nếu AI cho phép sai, cư dân có thể hiểu nhầm là hồ sơ đã được duyệt.

Nhóm em sửa prompt bằng cách bổ sung boundary: AI chỉ được tra cứu quy định đã duyệt, hỏi thông tin còn thiếu, tạo checklist và draft hướng dẫn. AI không được tự phê duyệt thi công, không được cho phép thi công ngoài giờ, không được bỏ qua đặt cọc/cam kết và mọi kết quả cuối cùng phải có nhân viên ban quản lý duyệt. Sau khi chỉnh, giải pháp hợp lý hơn là LLM Feature kết hợp rule/RAG validation, thay vì Agent tự động xử lý toàn bộ quy trình.
