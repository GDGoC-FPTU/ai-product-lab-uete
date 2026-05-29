# Lab 02 — Worksheet: AI Product Scoping (Vin Smart Future)

---

## 🏛️ 1. Bối cảnh thực tế: Vin Smart Future (Vingroup)

**Vingroup** — Tập đoàn tư nhân lớn nhất Việt Nam — vừa sáp nhập toàn bộ các phòng ban công nghệ thuộc các công ty thành viên thành một đơn vị công nghệ thống nhất mang tên **Vin Smart Future**. 

Nhiệm vụ của **Vin Smart Future** là xây dựng các giải pháp AI, số hóa, và tự động hóa cốt lõi để nâng cao hiệu suất vận hành và trải nghiệm khách hàng xuyên suốt các công ty thành viên:
* 🚗 **VinFast:** Hệ thống xe điện thông minh (EV), trợ lý AI ảo trong xe, dự đoán bảo trì pin, và quản lý chuỗi cung ứng sản xuất.
* 🚕 **Xanh SM (GSM):** Vận hành đội xe taxi/xe máy điện thông minh, điều vận thông minh (Smart Dispatching), tối ưu hóa lộ trình di chuyển.
* 🏢 **Vinhomes:** Quản lý đô thị thông minh (Smart Cities), trợ lý cư dân thông minh, tối ưu hóa mức tiêu thụ năng lượng.
* 🏥 **Vinmec:** Y tế thông minh, chẩn đoán hình ảnh bằng AI, tối ưu hóa quản lý hồ sơ bệnh án.
* 🎢 **Vinpearl / VinWonders:** Trải nghiệm du lịch số hóa, quản lý phòng và luồng khách thông minh tại các khu vui chơi.

Trong buổi Lab hôm nay, nhóm của bạn sẽ đóng vai trò là **AI Product Engineer** tại **Vin Smart Future**, tiến hành tìm kiếm, scoping, phân tích độ khả thi, thiết lập ranh giới vận hành, và xây dựng một **bản mẫu kỹ thuật (prompt prototype)** cho một bài toán cụ thể thuộc một trong những mảng kinh doanh trên.

---

## 📊 2. Cơ cấu tính điểm bài lab

### 👥 Điểm nhóm (60 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **G1. Workflow Mapping** | 20 | Problem Deep-Dive | Vẽ chi tiết quy trình hiện tại: các bước, handoff, thời gian, bottleneck |
| **G2. Problem Statement** | 20 | Problem Deep-Dive | Problem Statement 6-field bám sát thực tế, metric có số và ranh giới rõ ràng |
| **G3. AI Fit & Future Flow** | 10 | Problem Deep-Dive | So sánh Rule vs LLM vs Agent, future flow có bước AI, ranh giới và Fallback |
| **G4. Decision Quality** | 10 | Problem Deep-Dive | Quyết định Go/Not Yet/No-Go trung thực và có chứng cứ rõ ràng |

### 👤 Điểm cá nhân (40 điểm)

| Gate | Điểm | Deliverable | Tiêu chí chấm |
|---|---:|---|---|
| **I1. Scan & Cards** | 15 | Quick Cards | Liệt kê 5 problems sử dụng 3 lenses, hoàn thiện 3 quick cards chất lượng |
| **I2. Prototyping** | 10 | 02-lab/ | Chạy thử nghiệm programmatic prompt prototype thành công |
| **I3. AI Log & Reflection** | 15 | 03-ai-log.md | Phản ánh trung thực về việc dùng AI làm thought-partner (giúp gì, sai gì, sửa gì) |

---

# 🚀 Phase 0 — worked Example: Xanh SM Intelligent Dispatcher (15 min)

*Giảng viên walk-through ví dụ thực tế từ Vin Smart Future để bạn hiểu rõ cách scoping một bài toán AI.*
Đọc chi tiết worked example tại file [02-deliverable-example.md](02-deliverable-example.md).

---

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
| 1 | VinFast / V-Green | Repetitive + Stakeholder Pain | So khớp trạng thái sạc giữa trụ, xe, app và hóa đơn/phí phạt; giảm lỗi tính phí đỗ quá giờ hoặc phiên sạc không đồng bộ. |
| 2 | VinFast / V-Green | Time-consuming + Stakeholder Pain | Dự báo tình trạng trạm sạc còn chỗ, thời gian chờ và lỗi trạm để tránh việc app báo còn chỗ nhưng khách tới nơi lại đầy/hỏng. |
| 3 | Xanh SM | AI-upgrade + Stakeholder Pain | Tối ưu ETA và gợi ý điểm đón chính xác hơn cho khách/tài xế, nhất là ở sân bay, chung cư, trung tâm thương mại. |
| 4 | Vinhomes | Time-consuming + AI-upgrade | Tự động phân loại, ưu tiên và soạn phản hồi cho ticket cư dân về vệ sinh, bảo trì, phí dịch vụ, thang máy, an ninh. |
| 5 | Vinmec | Time-consuming + Stakeholder Pain | Tự động kiểm tra hồ sơ bảo hiểm/direct billing, trích xuất quyền lợi và chuẩn bị claim packet để giảm thời gian chờ của bệnh nhân. |
---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)

Chọn **top 3 bài toán** từ danh sách trên và hoàn thiện **3 Quick Problem Cards** dưới đây (10 phút/card).

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #___                                     │
│                                                             │
│ Bài toán (1 câu): ________________________________________  │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? ______________________________________ │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. ___ ──> 2. ___ ──> 3. ___ ──> 4. ___                   │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? ___ (⏱ ___ phút/lượt)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào? _____________________ │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? ______________________ │
│   VD: "Giảm thời gian soạn phản hồi từ 10 min ──> under 2 min"│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘

### 3 Quick Problem Cards da hoan thien

#### QUICK PROBLEM CARD #1

| Field | Noi dung |
|---|---|
| Bai toan (1 cau) | Phat hien bat thuong va doi soat tu dong giua tru sac, xe, app va hoa don de giam loi tinh phi/phat sai cho khach VinFast. |
| Cong ty thanh vien | [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [x] Khac: V-Green |
| Ai dang dau (Actor)? | Khach hang VinFast bi tinh phi/phat sai; nhan vien CSKH/van hanh tram sac phai tra log va xu ly khieu nai thu cong. |
| Workflow thu cong hien tai (3-5 buoc) | 1. Khach sac xe va ket thuc phien sac --> 2. App/tru/xe ghi nhan trang thai va phi --> 3. Neu co lech du lieu, khach gui khieu nai --> 4. CSKH tra log tu nhieu he thong --> 5. Nhan vien ket luan hoan phi/giu phi va phan hoi khach. |
| Buoc ton thoi gian/loi nhat | Buoc 4: tra log va doi soat thu cong giua app, tru sac, xe, thanh toan (15-25 phut/luot). |
| AI ho tro o buoc nao? | Buoc 2 va 4: tu dong phat hien anomaly trong phien sac, tom tat bang chung log, goi y nhan dinh "co kha nang tinh phi sai/khong sai" cho nhan vien duyet. |
| Metric co so | Giam thoi gian xu ly khieu nai sac tu 20 phut --> duoi 5 phut/case; tu dong gan co bat thuong cho >=80% case truoc khi khach goi CSKH; giam >=30% khieu nai lap lai. |
| Quick Architecture | [ ] No AI  [x] Rule  [x] LLM  [ ] Agent |

#### QUICK PROBLEM CARD #2

| Field | Noi dung |
|---|---|
| Bai toan (1 cau) | Du bao tinh trang tram sac con cho, thoi gian cho va nguy co loi truoc khi tai xe/khach di chuyen toi tram. |
| Cong ty thanh vien | [x] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [x] Khac: V-Green |
| Ai dang dau (Actor)? | Chu xe VinFast va tai xe Xanh SM can sac gap; bo phan van hanh tram sac phai xu ly qua tai, xe xep hang, khach phan nan app bao sai. |
| Workflow thu cong hien tai (3-5 buoc) | 1. Tai xe mo app xem tram sac gan nhat --> 2. App hien so tru/trang thai hien tai --> 3. Tai xe di toi tram --> 4. Neu tram day/loi thi phai cho hoac tim tram khac --> 5. Phat sinh thoi gian chet, huy chuyen hoac khieu nai. |
| Buoc ton thoi gian/loi nhat | Buoc 3-4: di toi tram nhung khong sac duoc, mat 10-30 phut/luot tuy khoang cach va hang cho. |
| AI ho tro o buoc nao? | Buoc 2: du bao occupancy/wait-time trong 15-60 phut toi, goi y tram thay the dua tren pin xe, vi tri, traffic, lich su qua tai, ca hoat dong Xanh SM. |
| Metric co so | Giam ty le "toi tram nhung khong sac duoc" tu baseline --> giam 40%; sai so du bao thoi gian cho <10 phut; tang utilization tram it qua tai them 15%. |
| Quick Architecture | [ ] No AI  [x] Rule  [ ] LLM  [x] Agent |

#### QUICK PROBLEM CARD #3

| Field | Noi dung |
|---|---|
| Bai toan (1 cau) | Toi uu ETA va goi y diem don chinh xac hon cho Xanh SM tai cac khu vuc kho don nhu san bay, chung cu, trung tam thuong mai. |
| Cong ty thanh vien | [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  [ ] Vinmec  [ ] Khac |
| Ai dang dau (Actor)? | Khach dat xe bi cho lau/sai diem don; tai xe mat thoi gian vong tim khach; tong dai ho tro va doi van hanh bi tang ticket. |
| Workflow thu cong hien tai (3-5 buoc) | 1. Khach dat xe va tha pin tren ban do --> 2. He thong gan tai xe va bao ETA --> 3. Tai xe di theo pin/duong goi y --> 4. Neu pin sai cong/loi vao, tai xe goi khach de tim nhau --> 5. Khach cho lau, huy chuyen hoac danh gia thap. |
| Buoc ton thoi gian/loi nhat | Buoc 3-4: tai xe va khach tim nhau bang goi dien/chat, mat 5-10 phut/luot o diem don phuc tap. |
| AI ho tro o buoc nao? | Buoc 1-2: goi y pickup point theo dia diem that su co the dung xe, lich su don thanh cong, gio cao diem, loai toa nha/cong vao; canh bao khi pin khach dat o vi tri kho don. |
| Metric co so | Giam thoi gian cho tai diem don tu 7 phut --> duoi 3 phut; giam ty le huy do khong tim thay tai xe/khach 25%; tang do chinh xac ETA len >=85% trong nguong sai so 3 phut. |
| Quick Architecture | [ ] No AI  [x] Rule  [ ] LLM  [x] Agent |
```

> [!TIP]
> **🤖 AI Prompts — Stress-Test thẻ bài toán:**
> Hãy dán nội dung thẻ bài toán của bạn vào LLM để nhận phản biện:
> *"Đây là một thẻ bài toán vận hành tôi đề xuất cho Vin Smart Future: [Dán nội dung]. Hãy đóng vai trò là một CFO và Trưởng phòng Vận hành cực kỳ khắt khe, chỉ ra cho tôi 3 điểm yếu về logic, metric, và giải thích vì sao rule-based code thông thường có thể giải quyết bài toán này tốt hơn là dùng AI."*

---

Nhóm quyết định lựa chọn:

### Quick Problem Card #7

| Field | Noi dung |
|---|---|
| Bai toan (1 cau) | Tro ly cu dan ao ho tro cu dan Vinhomes tra cuu thu tuc va draft nhanh ho so dang ky thi cong noi that, dang ky ve gui xe hang thang ma khong can gap truc tiep ban quan ly. |
| Cong ty thanh vien | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec  [ ] Khac |
| Ai dang dau (Actor)? | Cu dan Vinhomes can lam thu tuc hanh chinh; nhan vien ban quan ly phai tra loi lap lai, kiem tra ho so thieu va huong dan tung truong hop qua app/email/quay dich vu. |
| Workflow thu cong hien tai (3-5 buoc) | 1. Cu dan hoi ban quan ly ve thu tuc --> 2. Nhan vien gui quy dinh/bieu mau --> 3. Cu dan tu dien ho so va nop lai --> 4. Nhan vien kiem tra thieu/sai thong tin --> 5. Cu dan bo sung nhieu lan truoc khi duoc phe duyet. |
| Buoc ton thoi gian/loi nhat | Buoc 2-4: giai thich quy dinh va kiem tra ho so thieu/sai lap lai nhieu lan (10-20 phut/luot). |
| AI ho tro o buoc nao? | Buoc 1-4: chatbot tra cuu quy dinh theo toa/khu, hoi dap dieu kien, sinh checklist ca nhan hoa, draft form dang ky thi cong/ve xe va canh bao truong thong tin con thieu truoc khi nop. |
| Metric co so | Giam thoi gian tu van thu tuc tu 15 phut --> duoi 3 phut/yeu cau; >=70% ho so nop lan dau du thong tin; giam >=40% ticket lap lai ve cung mot thu tuc. |
| Quick Architecture | [ ] No AI  [x] Rule  [x] LLM  [x] Agent |


# 🏗️ Phase 3 — DEEP-DIVE (Nhóm, 85 min)

## 3.1. Current-State Workflow Mapping (25 min)
**Vẽ quy trình hiện tại lên bảng/giấy A3.** Sử dụng các ký hiệu:
* 🔴 **Bottleneck:** Bước gây tắc nghẽn, tốn thời gian, hoặc sai sót nhiều nhất.
* 🔄 **Handoff:** Điểm chuyển giao thông tin giữa người và hệ thống, hoặc giữa các bộ phận.
* Ghi rõ thời gian vận hành trung bình: **Tổng cộng = ____ phút/lượt**.

## 3.2. Problem Statement (6-field) & Metrics (15 min)
Điền đầy đủ 6 trường thông tin của bài toán:

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Ai đang thực hiện tác vụ hằng ngày? |
| **2. Current Workflow** | Mô tả tóm tắt quy trình thủ công hiện tại và công cụ sử dụng. |
| **3. Bottleneck** | Bước nào chậm, lỗi, hoặc cần xử lý ngôn ngữ tự động nhiều nhất? |
| **4. Business Impact** | Tổn thất thực tế đo bằng thời gian, chi phí, hoặc SLA của Vingroup. |
| **5. Success Metric** | AI giải quyết được thì đạt ngưỡng số mấy? (Ví dụ: *"85% vé được phân loại dưới 10s"*). |
| **6. Operational Boundary** | AI được phép làm gì, TUYỆT ĐỐI không được làm gì, điểm nào cần duyệt? |

## 3.3. Future-State Flow & AI Fit (25 min)
* **Xác định mức AI Fit (AI-Fit Matrix):** Giải pháp thuộc nhóm nào? [ ] Rule / State-Machine [ ] LLM Feature [ ] Agentic Loop.
* **Vẽ Future-State Flow:** Đánh dấu rõ:
  * 🔵 **AI Step:** Tác vụ LLM xử lý.
  * 🟢 **Human Step (HITL):** Bước con người phê duyệt/review (Human-in-the-loop).
  * ↩️ **Fallback:** Kế hoạch dự phòng khi LLM trả về kết quả lỗi hoặc không tự tin.

---

# 💻 Phase 4 — TECHNICAL PROMPT PROTOTYPE (Nhóm, 30 min)

Để đảm bảo kỹ sư của Vin Smart Future luôn giữ vững năng lực lập trình, nhóm của bạn sẽ tiến hành **lập trình bản mẫu prompt** trực tiếp trên **Gemini 2.5 Flash** bằng Python để stress-test hệ thống.

### Hướng dẫn thực hiện:
1. Mở file [starter-code/prompt_prototype.py](starter-code/prompt_prototype.py) bằng VS Code/Cursor.
2. Hoàn thiện các nội dung sau:
   * **System Prompt:** Viết chỉ thị cực kỳ nghiêm ngặt quy định vai trò, nhiệm vụ, định dạng output và **Operational Boundary (Ranh giới cấm)** của mô hình.
   * **Structured Output:** Định nghĩa định dạng JSON output rõ ràng.
   * **Adversarial Test Cases:** Viết ít nhất 3 prompts "tấn công" (Adversarial inputs) cố tình dụ AI vượt ranh giới hoặc đưa ra câu trả lời không được phép để kiểm tra xem ranh giới của bạn có thực sự vững chắc.
3. Chạy file python:
   ```bash
   python3 prompt_prototype.py
   ```
4. Kiểm tra xem các ranh giới an toàn có bị LLM phá vỡ hay không và ghi lại kết quả vào worksheet.

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:
1. [ ] Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?
2. [ ] Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?
3. [ ] Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:
[ ] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.
[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.
[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**
> *Viết lý giải chi tiết tại đây*

---

# 📝 Phase 6 — REFLECTION (Cá nhân)
*Ghi nhận phản ánh của cá nhân bạn về việc phối hợp với AI trong buổi học hôm nay vào file `03-ai-log.md`.*
