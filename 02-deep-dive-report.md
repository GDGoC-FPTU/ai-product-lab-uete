# Thông tin nhóm
Tên nhóm: UETE
Thành viên:
Trần Trung Kiên — 2A202600850
Lê Văn Khoa — 2A202600603
Lê Quang Hưng — 2A202600891
Nguyễn Văn Duy — 2A202600725
# Phase 3 - DEEP-DIVE

## Bai toan duoc chon

**Vinhomes - Tro ly cu dan ao ho tro thu tuc hanh chinh**

Tro ly cu dan ao ho tro cu dan Vinhomes tra cuu thu tuc va draft nhanh ho so dang ky thi cong noi that, dang ky ve gui xe hang thang ma khong can gap truc tiep ban quan ly.

---

## 3.1. Current-State Workflow Mapping

### Actor chinh

- **Cu dan Vinhomes:** can dang ky thi cong noi that, dang ky ve gui xe thang, hoi quy dinh va mau ho so.
- **Nhan vien ban quan ly:** tiep nhan cau hoi, gui quy dinh/bieu mau, kiem tra ho so, yeu cau bo sung, trinh phe duyet.
- **Bo phan van hanh/an ninh/ky thuat:** nhan thong tin da duoc phe duyet de cap quyen thi cong, cap the/ve xe, kiem soat ra vao.

### Quy trinh hien tai

1. **Cu dan gui cau hoi/yeu cau**
   - Kenh: app cu dan, email, dien thoai, quay dich vu.
   - Noi dung thuong gap: "Dang ky thi cong can gi?", "Lam ve xe thang nhu the nao?", "Can nop giay to nao?"

2. **Nhan vien ban quan ly doc va phan loai yeu cau**
   - Xac dinh loai thu tuc: thi cong noi that, ve gui xe, cap the cu dan, dang ky nha thau.
   - Xac dinh toa/khu can ap dung quy dinh rieng.
   - **Handoff:** tu cu dan sang ban quan ly.

3. **Nhan vien gui quy dinh, checklist va bieu mau**
   - Thuong la copy/paste tu file mau hoac tra cuu trong tai lieu noi bo.
   - Co nguy co gui nham version, thieu dieu kien theo tung toa/khu.

4. **Cu dan tu dien ho so va nop lai**
   - Ho so co the thieu CCCD, bien so xe, hop dong mua/cho thue, thong tin nha thau, thoi gian thi cong, cam ket an toan.
   - **Bottleneck:** cu dan khong biet truong nao bat buoc, nen nop sai/bo sung nhieu lan.

5. **Nhan vien kiem tra va yeu cau bo sung**
   - Neu thieu thong tin, nhan vien phai nhan lai qua app/email/dien thoai.
   - Mot yeu cau co the quay lai 2-3 vong truoc khi du dieu kien.

6. **Phe duyet va chuyen thong tin sang bo phan lien quan**
   - Thi cong noi that: chuyen cho ky thuat/an ninh de quan ly nha thau, gio thi cong, van chuyen vat lieu.
   - Ve gui xe: chuyen cho bo phan bai xe/an ninh de cap ve va ghi nhan bien so.
   - **Handoff:** ban quan ly sang van hanh/an ninh/ky thuat.

### Thoi gian van hanh trung binh

- Tiep nhan va phan loai yeu cau: **3-5 phut/luot**
- Tra cuu quy dinh va gui checklist/bieu mau: **5-10 phut/luot**
- Kiem tra ho so lan dau: **10-20 phut/luot**
- Yeu cau bo sung va theo doi lai: **10-15 phut/vong**
- **Tong cong uoc tinh:** 30-50 phut/yeu cau neu co 1 lan bo sung.

### Diem tac nghen

- **Bottleneck chinh:** buoc 3-5, dac biet la giai thich quy dinh va kiem tra ho so thieu/sai.
- **Nguyen nhan:** thong tin thu tuc dai, nhieu truong hop ngoai le, nhan vien phai copy/paste lap lai, cu dan khong co checklist ca nhan hoa truoc khi nop.

---

## 3.2. Problem Statement 6-field & Metrics

| Field | Noi dung chi tiet |
|---|---|
| **1. Actor / Operator** | Nhan vien ban quan ly Vinhomes dang xu ly cau hoi va ho so hanh chinh moi ngay; cu dan la nguoi bi anh huong truc tiep khi phai hoi lai/nop lai. |
| **2. Current Workflow** | Cu dan hoi thu tuc qua app/email/quay dich vu -> nhan vien doc va phan loai -> tra cuu quy dinh/bieu mau -> cu dan dien ho so -> nhan vien kiem tra -> yeu cau bo sung neu thieu -> phe duyet va chuyen cho bo phan lien quan. |
| **3. Bottleneck** | Kiem tra ho so va giai thich quy dinh lap lai: thieu truong bat buoc, sai mau, thieu giay to, nham dieu kien theo toa/khu. Buoc nay ton 10-20 phut/luot va de phat sinh nhieu vong bo sung. |
| **4. Business Impact** | Ban quan ly mat nhieu gio cho cau hoi lap lai; cu dan phai cho lau hoac den truc tiep; ticket ton dong lam giam SLA va diem hai long cu dan. Neu 100 yeu cau/thang, moi yeu cau mat 30 phut thi ton khoang 50 gio van hanh/thang. |
| **5. Success Metric** | Giam thoi gian tu van thu tuc tu 15 phut xuong duoi 3 phut/yeu cau; >=70% ho so nop lan dau du thong tin; giam >=40% ticket lap lai ve cung mot thu tuc; 90% cau hoi thu tuc pho bien co draft tra loi trong duoi 30 giay. |
| **6. Operational Boundary** | AI duoc phep tra cuu quy dinh, tao checklist, draft noi dung ho so va canh bao thieu thong tin. AI khong duoc tu phe duyet ho so, tu cap ve xe, tu cho phep thi cong, tu thay doi phi/dieu kien, hoac dua cam ket phap ly. Moi ho so truoc khi phe duyet can nhan vien ban quan ly review. |

---

## 3.3. Future-State Flow & AI Fit

### AI-Fit Matrix

**De xuat:** [x] Rule / State-Machine + [x] LLM Feature + [ ] Agentic Loop giai doan dau

- **Rule / State-Machine:** dung cho checklist bat buoc, dieu kien theo loai thu tuc, validate truong du lieu, routing ticket.
- **LLM Feature:** dung cho hoi dap tu nhien, tom tat quy dinh, draft form, giai thich loi thieu ho so bang ngon ngu de hieu.
- **Agentic Loop:** chua nen tu dong hoa hoan toan o MVP vi co rui ro phe duyet sai, cap quyen sai hoac anh huong an ninh/toa nha.

### Future-State Flow

1. **Cu dan chon loai thu tuc tren app**
   - Vi du: dang ky thi cong noi that, dang ky ve gui xe thang.
   - He thong yeu cau thong tin co cau truc: toa, can ho, chu ho/nguoi thue, bien so xe hoac thong tin nha thau.

2. **AI Step - Tra cuu va ca nhan hoa checklist**
   - AI lay quy dinh tu knowledge base da duoc ban quan ly phe duyet.
   - Tao checklist theo dung loai thu tuc va khu/toa.
   - Neu thieu thong tin quan trong, AI hoi lai cu dan truoc khi tao ho so.

3. **AI Step - Draft ho so va noi dung phan hoi**
   - Dang ky thi cong: draft form gom thoi gian thi cong, thong tin nha thau, danh sach nhan su, cam ket an toan, hang muc thi cong.
   - Dang ky ve xe: draft form gom thong tin cu dan, can ho, bien so, loai xe, thoi han dang ky, giay to dinh kem.

4. **Rule Step - Validate truong bat buoc**
   - Kiem tra CCCD/hop dong/giay to xe/bien so/thoi gian thi cong/co mat file dinh kem.
   - Gan trang thai: `ready_for_review`, `missing_info`, `out_of_policy`.

5. **Human Step (HITL) - Nhan vien ban quan ly review**
   - Nhan vien xem checklist, draft ho so, ly do AI danh dau thieu/sai.
   - Nhan vien phe duyet, yeu cau bo sung, hoac escalate cho bo phan lien quan.

6. **Handoff - Chuyen sang van hanh/an ninh/ky thuat**
   - Neu duoc phe duyet, he thong tao ticket cho bo phan an ninh/bai xe/ky thuat.
   - AI chi tao draft/noi dung chuyen giao, khong tu cap quyen.

7. **AI Step - Draft phan hoi cho cu dan**
   - Draft thong bao: ho so da tiep nhan, can bo sung gi, du kien thoi gian xu ly.
   - Tin nhan gui ra ngoai can nhan vien review hoac rule approval theo policy.

### Fallback khi AI loi hoac khong tu tin

- Neu AI khong tim thay quy dinh phu hop: chuyen ticket sang nhan vien ban quan ly, hien ly do `missing_policy_source`.
- Neu thong tin cu dan mau thuan: yeu cau cu dan xac nhan lai truoc khi tao draft.
- Neu ho so lien quan phi, tranh chap, vi pham noi quy, thay doi ket cau, hoac truong hop ngoai le: bat buoc escalate cho nhan vien.
- Neu confidence < 0.75: khong draft cau tra loi ket luan, chi liet ke thong tin can nhan vien kiem tra.

### Du lieu can co cho prototype

- Bo quy dinh/FAQ da duoc phe duyet cho dang ky thi cong noi that va ve gui xe.
- Mau form dang ky hien tai.
- 20-50 ticket mau da an danh hoa.
- Danh sach truong bat buoc theo tung loai thu tuc.
- Label cho cac ket qua: du ho so, thieu thong tin, sai dieu kien, can escalate.

### Scope MVP

- Chi ho tro 2 thu tuc dau tien: **dang ky thi cong noi that** va **dang ky ve gui xe hang thang**.
- Chi tao checklist, draft form va draft phan hoi.
- Khong tu phe duyet, khong tu cap quyen, khong xu ly thanh toan/phi.
- Human review bat buoc truoc moi quyet dinh van hanh.

---

# Phase 5 - EVALUATE

### AI Readiness Checklist

1. [x] Chung toi co san du lieu mau/logs sach de test?
   - Co the bat dau tu FAQ/quy dinh noi bo, mau form dang ky thi cong noi that, mau form dang ky ve gui xe, va 20-50 ticket cu da an danh hoa.
   - Can lam sach them cac truong nhay cam nhu ten cu dan, so dien thoai, CCCD, bien so xe, ma can ho truoc khi dung de test.

2. [x] Rui ro khi AI sai co nam trong tam kiem soat qua HITL hoac Fallback?
   - Co. AI chi duoc tao checklist, draft form, draft phan hoi va canh bao thieu thong tin.
   - AI khong duoc tu phe duyet ho so, tu cap ve xe, tu cho phep thi cong, tu thay doi phi/dieu kien, hoac dua cam ket phap ly.
   - Moi truong hop `out_of_policy`, confidence < 0.75, tranh chap, phi, vi pham noi quy, thay doi ket cau deu phai escalate cho nhan vien ban quan ly.

3. [x] Stakeholders san sang thay doi quy trinh lam viec cu?
   - Phu hop de pilot voi ban quan ly vi khong thay the buoc phe duyet, chi giam viec lap lai: tra loi FAQ, tao checklist, kiem tra truong thieu.
   - Cu dan co loi ich truc tiep vi biet can nop gi truoc khi gui ho so, giam so lan bo sung.

### Quyet dinh cuoi cung cua Ban Giam Doc Vin Smart Future

[x] **GO:** Bat dau phat trien voi scope hep.
[ ] **NOT YET:** Can tich luy them du lieu/xac lap baseline.
[ ] **NO-GO:** Khong kha thi hoac rule-based tot hon.

**Justification:**
> Chon GO vi bai toan co tan suat lap lai cao, tac dong truc tiep den SLA ban quan ly va trai nghiem cu dan, trong khi rui ro co the kiem soat bang Human-in-the-loop. MVP chi nen gioi han trong 2 thu tuc pho bien: dang ky thi cong noi that va dang ky ve gui xe hang thang. AI khong ra quyet dinh van hanh cuoi cung ma chi tra cuu quy dinh, tao checklist, draft form va danh dau ho so thieu/sai cho nhan vien review.
>
> Thanh cong duoc do bang cac metric ro rang: giam thoi gian tu van thu tuc tu 15 phut xuong duoi 3 phut/yeu cau, >=70% ho so nop lan dau du thong tin, giam >=40% ticket lap lai ve cung mot thu tuc, va 90% cau hoi thu tuc pho bien co draft tra loi trong duoi 30 giay. Neu prototype khong dat cac nguong nay sau pilot, du an nen quay ve rule-based checklist va FAQ search thay vi mo rong LLM.
>
> Chi phi prototype thap hon cac bai toan can tich hop thiet bi/IoT vi du lieu dau vao chu yeu la van ban: FAQ, quy dinh, bieu mau, ticket cu. De giam rui ro, giai doan dau can dung RAG tren knowledge base da phe duyet, logging day du, an danh hoa du lieu ca nhan, va bat buoc nhan vien ban quan ly phe duyet truoc khi gui thong tin cho cu dan hoac chuyen ticket sang van hanh/an ninh/ky thuat.
