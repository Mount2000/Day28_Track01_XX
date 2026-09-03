# Báo Cáo Áp Dụng AI - Nhóm Day28_Track01_TeamXX

## 1. Danh sách thành viên và phân công công việc

| Họ tên | MSSV | Phần phụ trách chính | Góp ý đã đưa cho nhóm bạn (Nhóm Chicken Plus — Dự án HPTravelAI) |
| :--- | :--- | :--- | :--- |
| **Mai Hồng Sơn** | `2A202601921` | **Lead Chẩn đoán & Quản trị**: Khóa phạm vi, phân tích Gartner-Lite, xác định bằng chứng, thiết kế Kiến trúc tin cậy, xây dựng Lộ trình 30-60-90 ngày theo 3 Cổng quyết định, chủ trì viết `memo_quyet_dinh.md` & tổng hợp README. | **Phản biện trục Phạm vi & Gartner-Lite (Readiness):** Mô hình tổng hợp review từ MXH bên thứ ba (TikTok/Facebook) có rủi ro lớn về tính ổn định của nguồn dữ liệu và bản quyền/scraping. Đề xuất nhóm bổ sung cơ chế dự phòng dữ liệu nếu API bên thứ ba bị chặn hoặc thay đổi cấu trúc, đồng thời cam kết rõ Data Freshness SLA (thời hạn làm mới dữ liệu tối đa 48h) và quy định trách nhiệm Data Owner kiểm duyệt nội dung trước khi nạp vào vector store. |
| **Nguyễn Tuấn Vũ** | `2A202601845` | **Lead Quy trình & Chỉ số**: Phân tích hành vi ADKAR, phân bổ vai trò Mollick, thiết kế quy trình AS-IS / TO-BE, thiết kế & cập nhật `dashboard_hanh_dong_v1` và `v2`. | **Phản biện trục Chỉ số & Hành động (UX vs Compliance):** Ở Dashboard Tầng 2, chỉ số *Tỷ lệ xem bài review gốc* đặt target ≥ 70% và hành động khi chỉ số xấu là cưỡng bức hiển thị Popover Checklist 3 điểm có thể tạo ma sát trải nghiệm (UX friction) quá lớn cho Gen Z. Đề xuất đổi cơ chế phạt sang Gamification (thưởng điểm uy tín/voucher khi đối chiếu nguồn); đồng thời cần công thức hóa rõ ràng cách tính *Trust Score (ngưỡng 8/10)* ở Tầng 4 để tránh đo cảm tính. |

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

### 4.2 Thiết kế Quy trình AS-IS vs TO-BE (So sánh trực quan & 3 thay đổi bắt buộc)

| Bước thực hiện | Quy trình cũ (AS-IS) | Quy trình mới có AI (TO-BE) | Phân định vai trò & Trách nhiệm |
| :--- | :--- | :--- | :--- |
| **Bước 1: Tiếp nhận yêu cầu** | Nhận tình huống nghiệp vụ, mở nhiều thư mục mạng chia sẻ tìm file PDF/Word. | Nhập yêu cầu tra cứu vào DocBot RAG theo template chuẩn hóa. | **Nhân viên vận hành** thực hiện |
| **Bước 2: Tìm kiếm & Tổng hợp** | Đọc lướt nhiều tài liệu cũ, dễ nhầm lẫn phiên bản bãi bỏ. Mất 12–15 phút. | AI tự động trích xuất đoạn văn bản, tóm tắt và **bắt buộc hiển thị Citations (Số hiệu, trang, ngày hiệu lực)**. Mất 10–30 giây. | **AI hỗ trợ (Mollick)** |
| **Bước 3: Kiểm chứng nguồn** *(BẮT BUỘC 1)* | Bỏ qua hoặc chỉ dựa trên trí nhớ; nếu phân vân thì hỏi miệng đồng nghiệp xung quanh. | **Bắt buộc click đối chiếu trực tiếp trích dẫn văn bản gốc** và kiểm tra trạng thái hiệu lực (Metadata). | **Nhân viên vận hành (Chịu trách nhiệm kiểm chứng)** |
| **Bước 4: Xử lý khi AI sai/không chắc** *(BẮT BUỘC 2)* | Không có cơ chế báo lỗi; nhân viên tự đoán hoặc đùn đẩy trách nhiệm. | **Bấm nút Báo lỗi (Feedback)** & **Kích hoạt Fallback chuyển tự động sang Trưởng ca/Chuyên gia** giải đáp. | **Hệ thống AI & Trưởng ca vận hành** |
| **Bước 5: Ký duyệt & Thực thi** *(BẮT BUỘC 3)* | Thực thi nhưng dễ sai sót chính sách mới; không ai chịu trách nhiệm giải trình. | Tick chọn checkbox "Đã kiểm chứng nguồn", ký duyệt kết quả nghiệp vụ và thực thi an toàn. | **Nhân viên vận hành (Chịu trách nhiệm cuối)** |

> **Ba thay đổi cốt lõi trong quy trình TO-BE:**
> 1. **Nguồn kiểm chứng:** Mọi câu trả lời AI đều đính kèm trích dẫn văn bản gốc và ngày hiệu lực để đối chiếu.
> 2. **Người chịu trách nhiệm:** Nhân viên vận hành giữ quyền phê duyệt và chịu trách nhiệm cho kết quả nghiệp vụ cuối cùng.
> 3. **Xử lý khi AI không chắc:** Cơ chế nút Báo lỗi kèm chuyển giao tự động (Fallback) cho Trưởng ca khi độ tin cậy thấp.

### 4.3 Thiết kế Kiến trúc Tin cậy (Trust Architecture) & 4 Điều kiện Mức sẵn sàng
Chuỗi kiểm soát vận hành khép kín:
$$\text{Nguồn dữ liệu có Data Owner} \longrightarrow \text{Trích nguồn bắt buộc (Citations)} \longrightarrow \text{QA kiểm tra mẫu 20%} \longrightarrow \text{Chuyển giao người khi không chắc (Fallback)} \longrightarrow \text{Vòng phản hồi & Học từ lỗi}$$

* **4 Điều kiện Mức sẵn sàng (Readiness Checklist) bắt buộc:**
  1. *Nguồn dữ liệu:* Có Data Owner phụ trách; cập nhật định kỳ ngày 25 hàng tháng; gắn nhãn trạng thái hiệu lực rõ ràng.
  2. *Quyền truy cập:* Phân quyền theo vai trò/ca trực, không truy cập tài liệu chưa ban hành.
  3. *Phạm vi rõ ràng:* Ban hành bảng phân định Jagged Frontier (việc được dùng AI và việc cấm dùng).
  4. *Chi phí & Vận hành:* Thiết lập trần chi phí Token RAG và quy chế xử lý sự cố.

### 4.4 Lộ trình 30–60–90 ngày: Ba Cổng Quyết định dựa trên Bằng chứng (Gate Decision Process)

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

## 5. Hệ thống chỉ số (Action Dashboard v2)

*Chi tiết đối chiếu: [`dashboard/dashboard_hanh_dong_v2.csv`](dashboard/dashboard_hanh_dong_v2.csv) (bản v2) và [`v1/dashboard_hanh_dong_v1.csv`](v1/dashboard_hanh_dong_v1.csv) (bản v1).*

| Tầng đo lường | Loại chỉ số | Tên chỉ số ra quyết định | Mốc ban đầu (Baseline) | Mục tiêu (Target) | Nguồn dữ liệu (Source) | Người phụ trách (Owner) | Hành động cụ thể khi chỉ số xấu |
| :--- | :--- | :--- | :---: | :---: | :--- | :---: | :--- |
| **1. Sử dụng** | **Product Metric** | Tỷ lệ câu trả lời AI có đầy đủ trích nguồn văn bản và ngày hiệu lực | 18% | ≥ 95% | Log RAG & Citation parser | Mai Hồng Sơn | Tạm dừng sinh tự do; kiểm tra lại pipeline metadata và semantic chunking. |
| **2. Hành vi** | **Workflow Metric** | Tỷ lệ nhân viên thực hiện đối chiếu nguồn và tick xác nhận trước khi áp dụng SOP | 12% | ≥ 90% | Telemetry click Verify Citation | Nguyễn Tuấn Vũ | Bật pop-up bắt buộc click link văn bản gốc trước khi kích hoạt nút 'Xác nhận'. |
| **3. Năng suất** | **Workflow Metric** | Thời gian trung bình hoàn thành một yêu cầu tra cứu SOP (AHT) | 14.5 phút | ≤ 3.5 phút | Ticket duration log hệ thống | Trưởng ca Vận hành | Tinh chỉnh độ trễ RAG (< 5s); bổ sung danh mục query template chuẩn hóa. |
| **4. Chất lượng & Tin cậy** | **Workflow Metric** | Tỷ lệ yêu cầu phải làm lại do thông tin sai/hết hạn (Rework Rate) | 26% | ≤ 3% | Báo cáo QA mẫu 20% ca trực | Trưởng nhóm QA | Rà soát khẩn với Data Owner; tạm đình chỉ các tài liệu bị gắn cờ báo lỗi. |
| **5. Giá trị** | **Business Metric** | Chi phí xử lý khiếu nại phát sinh từ áp dụng sai quy trình | 35 tr VNĐ / tháng | Giảm ≥ 60% (≤ 14 tr) | Báo cáo tài chính & bồi hoàn | Giám đốc Vận hành | Đánh giá lại mức độ sẵn sàng; dừng tính năng tự động và siết chặt 100% duyệt người. |
| **6. Rủi ro & Fallback** | **Workflow Metric** | Tỷ lệ kích hoạt Fallback chuyển tự động sang Trưởng ca khi AI không chắc (< 80%) | 0% (chưa có) | ≤ 6% | Log Fallback & Handoff Tickets | Mai Hồng Sơn & Trưởng ca | Nếu > 8%: Họp kỹ thuật bổ sung dữ liệu vào vùng khuyết; cập nhật prompt lọc câu hỏi. |

---

## 6. Quyết định & Thay đổi sau phản biện chéo (Decision & Changes in v2)

### 6.1 Quyết định đề xuất: 🟡 SỬA TRƯỚC KHI MỞ RỘNG (REPAIR / PIVOT)
* **Lý do cốt lõi:** Vấn đề sụt giảm sử dụng không bắt nguồn từ việc nhân viên "chống đối" hay "thiếu kỹ năng prompt", mà do lỗ hổng nghiêm trọng về **Kiến trúc tin cậy (câu trả lời thiếu trích dẫn nguồn/ngày hiệu lực)** và **Quy trình chưa tích hợp cơ chế kiểm chứng & chuyển giao rủi ro**. Cần hoàn thiện bản vá v2 và vượt qua Cổng 1 (Gate 30) trước khi mở rộng quy mô.

### 6.2 Các thay đổi cốt lõi so với bản v1 (Dựa trên góp ý của nhóm Chicken Plus):
1. **Bổ sung cơ chế Fallback và nút Báo lỗi (Tầng 6):** Khắc phục trực tiếp khuyết điểm *"chưa có nút báo lỗi và cơ chế chuyển giao khi AI không chắc"* mà nhóm Chicken Plus chỉ ra; thiết lập chỉ số Fallback chuyển giao Trưởng ca với target $\le 6\%$.
2. **Thiết lập chốt chặn kiểm chứng nguồn bắt buộc (Source Verification Gate ở Tầng 2):** Khắc phục khuyết điểm *"nhân viên không dám dùng do sợ sai"* bằng cách bắt buộc đối chiếu văn bản gốc trên giao diện trước khi hoàn tất tác vụ, nâng target lên $\ge 90\%$.
3. **Chuẩn hóa đo lường Năng suất & Chất lượng thực tế (Tầng 3 & Tầng 4):** Thay vì đo thời gian ước lượng, đo AHT trực tiếp từ log vé tác vụ (giảm mục tiêu xuống $\le 3.5$ phút) và kiểm soát Rework Rate $\le 3\%$ gắn với trách nhiệm dọn dẹp kho văn bản của Data Owner.
