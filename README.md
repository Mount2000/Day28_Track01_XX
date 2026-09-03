# Báo Cáo Áp Dụng AI - Nhóm Day28_Track01_Van_Hanh

## 1. Danh sách thành viên và phân công công việc

| Họ tên | MSSV | Phần phụ trách chính | Góp ý đã đưa cho nhóm bạn (Chặng 3) |
| :--- | :--- | :--- | :--- |
| **Mai Hồng Sơn** | `2A202601921` | **Lead Chẩn đoán & Quản trị**: Khóa phạm vi, phân tích Gartner-Lite, xác định bằng chứng, thiết kế Kiến trúc tin cậy, xây dựng Lộ trình 30-60-90 ngày theo 3 Cổng quyết định, chủ trì viết `memo_quyet_dinh.md` & tổng hợp README. | *(Ghi nhận sau Chặng 3: Nhận xét nhóm bạn về tính khả thi của dữ liệu và phân chia vai trò)* |
| **Nguyễn Tuấn Vũ** | `2A202601845` | **Lead Quy trình & Chỉ số**: Phân tích hành vi ADKAR, phân bổ vai trò Mollick, thiết kế quy trình AS-IS / TO-BE, thiết kế & cập nhật `dashboard_hanh_dong_v1` và `v2`. | *(Ghi nhận sau Chặng 3: Nhận xét nhóm bạn về việc chuyển từ Activity metrics sang Decision metrics)* |

---

## 2. Phạm vi áp dụng (Scope Lock - Chặng 1)

* **Sản phẩm AI:** Trợ lý AI tra cứu tài liệu nội bộ (*Internal Knowledge Assistant / DocBot* tích hợp RAG).
* **Nhóm người dùng chính:** Nhân viên phòng Vận hành (Operations Staff) gồm 35 nhân sự xử lý nghiệp vụ hàng ngày.
* **2–4 Quy trình liên quan:**
  1. Tra cứu chính sách, quy định và quy trình thao tác chuẩn (SOP).
  2. Kiểm chứng tính hiệu lực, ngày cập nhật và điều kiện áp dụng văn bản nghiệp vụ.
  3. Hướng dẫn và giải đáp tình huống xử lý nghiệp vụ cho nhân sự mới.
* **Vấn đề quan sát được (Triệu chứng):** Sau 6 tuần cấp tài khoản (Deployment), mức độ sử dụng giảm 70%, nhân viên quay lại tìm kiếm thủ công trên thư mục mạng chia sẻ hoặc nhắn tin hỏi đồng nghiệp.

---

## 3. Chẩn đoán & Nguyên nhân gốc (Root Causes - Chặng 1)

### 3.1 Ứng dụng các Framework chẩn đoán

* **Gartner-Lite (Tổ chức & Mức sẵn sàng):**
  * *Direction (Hướng đi):* **ĐẠT** – Mục tiêu rõ ràng: rút ngắn thời gian tìm kiếm tài liệu từ 15 phút xuống dưới 4 phút/tác vụ.
  * *Readiness (Mức sẵn sàng):* **THIẾU** – Kho tài liệu chưa được gán Metadata (ngày ban hành, người phê duyệt, thời hạn hiệu lực); chưa có Data Owner chịu trách nhiệm cập nhật tài liệu khi có chính sách mới.
  * *Absorption (Khả năng hấp thụ):* **THIẾU** – Thiếu kênh tiếp nhận phản hồi lỗi (Feedback Loop) và quy chế chịu trách nhiệm khi thực hiện theo hướng dẫn của AI.
* **Mollick - Jagged Frontier (Phân chia công việc Người – AI):**
  * *Hiện trạng:* Đẩy toàn bộ việc tra cứu và ra quyết định cho AI mà không định nghĩa ranh giới an toàn.
  * *Chuẩn hóa phân định:* 
    * *Con người giữ quyền:* Quyết định áp dụng quy trình cho khách hàng/đối tác, xử lý các trường hợp ngoại lệ (Edge cases), chịu trách nhiệm pháp lý.
    * *AI hỗ trợ:* Trích xuất đoạn văn bản, tóm tắt nội dung chính, đối chiếu tài liệu và luôn trích dẫn kèm số hiệu văn bản/ngày ban hành.
    * *AI tự động có kiểm soát:* Chỉ áp dụng cho các FAQ chuẩn mực đã qua thẩm định của Lead QA.
* **ADKAR (Điểm nghẽn người dùng):**
  * *Awareness:* Người dùng nhận thức được công cụ hỗ trợ tiết kiệm thời gian.
  * *Desire (ĐIỂM NGHẼN CHÍNH):* Người dùng **sợ rủi ro sai sót** vì câu trả lời của AI không có căn cứ xác thực; họ thà mất thời gian hỏi người thật để an toàn trách nhiệm cá nhân.
  * *Knowledge / Ability:* Chưa được hướng dẫn cách kiểm chứng chéo (verify) nguồn tài liệu khi AI đưa ra câu trả lời.
  * *Reinforcement (ĐIỂM NGHẼN THỨ HAI):* Hệ thống không ghi nhận đóng góp khi nhân viên phát hiện tài liệu hết hạn hoặc lỗi AI.

### 3.2 Hai nguyên nhân gốc & Bằng chứng thực tế

1. **Nguyên nhân gốc 1: Thiếu Kiến trúc Tin cậy (Trust Architecture)**
   * *Bản chất:* AI trả về văn bản tổng hợp không đính kèm trích dẫn văn bản gốc (Citations), số hiệu văn bản và ngày cập nhật hiệu lực; không có cảnh báo khi thiếu ngữ cảnh.
   * *Bằng chứng:* Rà soát log 50 truy vấn gần nhất cho thấy **82% câu trả lời không kèm nguồn trích dẫn cụ thể**, khiến 15/18 nhân viên phỏng vấn khẳng định "không dám tin dùng trong ca làm việc".
2. **Nguyên nhân gốc 2: Quy trình làm việc chưa gắn bước kiểm chứng và thiếu trách nhiệm giải trình**
   * *Bản chất:* Quy trình SOP nội bộ chưa hề cập nhật bước sử dụng AI; không quy định ai chịu trách nhiệm nếu AI đưa thông tin sai lệch, tạo tâm lý phòng thủ ở nhân viên.
   * *Bằng chứng:* SOP hiện tại quy định "Mọi quyết định phải căn cứ văn bản ký duyệt", nhưng AI không đưa ra được số quyết định ký duyệt; bài học từ Morgan Stanley chứng minh áp dụng AI chỉ thành công khi có bước kiểm soát tuân thủ và trách nhiệm con người.

---

## 4. Cách làm mới & Giải pháp Quản trị (Chặng 2 - Phụ trách: Mai Hồng Sơn & Nguyễn Tuấn Vũ)

### 4.1 Chọn giải pháp theo đúng nguyên nhân (Tránh lỗi mặc định đào tạo)

| Nguyên nhân gốc xác định | Giải pháp tương ứng | Hành động cụ thể |
| :--- | :--- | :--- |
| **Độ tin cậy kém & thiếu nguồn** | Thiết kế **Kiến trúc Tin cậy (Trust Architecture)** | Bắt buộc hiển thị Citations (tên file, số trang, ngày hiệu lực); tạo cơ chế QA mẫu 20% mỗi tuần. |
| **Điểm nghẽn Desire (sợ trách nhiệm)** | Tái cấu trúc **Quy trình SOP & Phân bổ Mollick** | Quy định rõ AI chỉ là công cụ hỗ trợ tìm kiếm; con người chịu trách nhiệm kiểm tra nguồn đối chiếu trước khi thực thi. |
| **Tổ chức chưa sẵn sàng (Readiness thiếu)** | Bổ nhiệm **Data Owner & Governance** | Chỉ định Data Owner dọn dẹp tài liệu hết hạn; phân quyền truy cập tài liệu theo cấp bậc. |
| **Đo lường sai (chỉ đo activity)** | Tái cấu trúc **Hệ thống Đo lường theo Giá trị** | Chuyển từ đếm login/prompts sang đo thời gian tác vụ (AHT) và tỷ lệ làm lại (Rework rate). |

### 4.2 Thiết kế Kiến trúc Tin cậy (Trust Architecture)
Chuỗi kiểm soát vận hành khép kín:
$$\text{Nguồn dữ liệu có Data Owner} \longrightarrow \text{Trích nguồn bắt buộc (Citations)} \longrightarrow \text{QA kiểm tra mẫu 20%} \longrightarrow \text{Chuyển giao người khi không chắc (Fallback)} \longrightarrow \text{Vòng phản hồi & Học từ lỗi}$$

* **4 Điều kiện Mức sẵn sàng (Readiness Checklist) bắt buộc:**
  1. *Nguồn dữ liệu:* Có Data Owner phụ trách; cập nhật định kỳ ngày 25 hàng tháng; gắn nhãn trạng thái hiệu lực rõ ràng.
  2. *Quyền truy cập:* Phân quyền theo vai trò/ca trực, không truy cập tài liệu chưa ban hành.
  3. *Phạm vi rõ ràng:* Ban hành bảng phân định Jagged Frontier (việc được dùng AI và việc cấm dùng).
  4. *Chi phí & Vận hành:* Thiết lập trần chi phí Token RAG và quy chế xử lý sự cố.

### 4.3 Lộ trình 30–60–90 ngày: Ba Cổng Quyết định dựa trên Bằng chứng (Gate Decision Process)

```
[0 - 30 ngày: CỔNG 1] --------------> [31 - 60 ngày: CỔNG 2] --------------> [61 - 90 ngày: CỔNG 3]
  Chứng minh độ tin cậy                 Chứng minh chất lượng                  Quyết định mở rộng
```

* **Giai đoạn 0–30 ngày (Cổng 1: Chứng minh độ tin cậy & Chuẩn hóa dữ liệu):**
  * *Hành động:* Nâng cấp giao diện bắt buộc trích nguồn + ngày hiệu lực; Data Owner dọn dẹp các tài liệu cũ; ban hành SOP tạm thời; ghi nhận mốc Baseline.
  * *Owner:* **Mai Hồng Sơn** *(Lead Quản trị)* phối hợp Đội Kỹ thuật AI.
  * *Dấu hiệu hoàn thành (Gate Criteria):* 100% tài liệu có metadata chuẩn; tỷ lệ câu trả lời có nguồn trích dẫn đạt ≥ 90%; Baseline 5 chỉ số được ghi nhận đầy đủ.
* **Giai đoạn 31–60 ngày (Cổng 2: Chứng minh chất lượng & Thử nghiệm có kiểm soát):**
  * *Hành động:* Triển khai Pilot trên nhóm hạt nhân (10 nhân viên vận hành); bật nút báo lỗi; thực hiện QA mẫu 20% truy vấn/tuần; theo dõi tỷ lệ làm lại (Rework rate).
  * *Owner:* **Nguyễn Tuấn Vũ** *(Lead Quy trình)* phối hợp Trưởng ca Vận hành.
  * *Dấu hiệu hoàn thành (Gate Criteria):* Tỷ lệ nhân viên thực hiện xác nhận nguồn ≥ 85%; Tỷ lệ câu hỏi bị báo lỗi/làm lại giảm xuống ≤ 5%; không có sự cố áp dụng nhầm tài liệu cũ.
* **Giai đoạn 61–90 ngày (Cổng 3: Đánh giá giá trị nghiệp vụ & Quyết định mở rộng):**
  * *Hành động:* Đối chiếu chỉ số Dashboard v2 với mục tiêu; thẩm định chi phí - lợi ích (ROI); họp Hội đồng Chuyển đổi số chốt phương án Rollout toàn diện.
  * *Owner:* **Hội đồng Vận hành & Chuyển đổi số** (Báo cáo bởi Sơn & Vũ).
  * *Dấu hiệu hoàn thành (Gate Criteria):* Thời gian tra cứu trung bình giảm ≥ 60% (≤ 4 phút); chi phí rủi ro quy trình giảm ≥ 50%; sẵn sàng mở rộng cho toàn bộ 35 nhân sự.

---

## 5. Hệ thống chỉ số (Dashboard Metrics)

* **Product Metric:** Tỷ lệ câu trả lời AI có đầy đủ trích nguồn hợp lệ và ngày cập nhật (Baseline: 18% $\rightarrow$ Target: ≥ 95% | Nguồn: Log hệ thống RAG | Owner: Mai Hồng Sơn).
* **Workflow Metric:** Thời gian xử lý trung bình một yêu cầu tra cứu - AHT (Baseline: 14.5 phút $\rightarrow$ Target: ≤ 4.0 phút | Nguồn: Ticket duration log | Owner: Trưởng ca Vận hành).
* **Quality Metric:** Tỷ lệ yêu cầu phải làm lại do thông tin sai - Rework Rate (Baseline: 26% $\rightarrow$ Target: ≤ 3% | Nguồn: QA mẫu 20% ca trực | Owner: Trưởng nhóm QA).

---

## 6. Quyết định (Decision)

* **Quyết định đề xuất:** **SỬA (REPAIR / PIVOT)** trước khi tiếp tục mở rộng.
* **Lý do cốt lõi:** Vấn đề không nằm ở việc nhân viên "chống đối" hay "thiếu kỹ năng prompt", mà nằm ở độ tin cậy của hệ thống (Readiness & Trust Architecture) và quy trình giao việc chưa rõ ràng. Cần hoàn thiện kiến trúc tin cậy và chuẩn hóa quy trình SOP trước khi mở rộng rollout toàn công ty.
