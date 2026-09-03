# MEMO QUYẾT ĐỊNH: ĐÁNH GIÁ & ĐỊNH HƯỚNG ÁP DỤNG TRỢ LÝ AI TRA CỨU TÀI LIỆU NỘI BỘ

**Người gửi:** Nhóm Day28_Track01_Van_Hanh  
- **Mai Hồng Sơn** (`2A202601921`) - Lead Chẩn đoán & Quản trị (Thành viên A)  
- **Nguyễn Tuấn Vũ** (`2A202601845`) - Lead Quy trình & Chỉ số (Thành viên B)  
**Người nhận:** Ban Lãnh đạo Vận hành & Hội đồng Chuyển đổi số  
**Ngày:** 03/09/2026  
**Chủ đề:** Đánh giá hiện trạng triển khai Trợ lý AI và Đề xuất phương án Sửa đổi kiến trúc áp dụng

---

## 1. Vấn đề và Nguyên nhân gốc

### 1.1 Vấn đề quan sát được (Triệu chứng)
Sau 6 tuần triển khai và cấp tài khoản sử dụng Trợ lý AI tra cứu tài liệu nội bộ (DocBot RAG) cho 35 nhân sự phòng Vận hành:
* Tần suất sử dụng giảm sút nghiêm trọng: Từ 120 lượt tra cứu/ngày trong tuần đầu tiên xuống còn dưới 25 lượt/ngày ở tuần thứ 6.
* Nhân viên vận hành có xu hướng quay lại thói quen cũ: Tự tìm kiếm thủ công trong hàng trăm thư mục chia sẻ hoặc gửi tin nhắn hỏi trực tiếp các trưởng nhóm/đồng nghiệp kỳ cựu.
* Dashboard kỹ thuật ban đầu chỉ ghi nhận số lượt đăng nhập và số câu lệnh (Activity metrics), tạo ảo tưởng về tỷ lệ phủ nhưng không phản ánh được giá trị thực tế mang lại cho quy trình nghiệp vụ.

### 1.2 Hai nguyên nhân gốc (Root Causes)
1. **Thiếu Kiến trúc Tin cậy (Trust Architecture):** AI trả lời dưới dạng văn bản tổng hợp nhưng không trích dẫn chính xác mã hiệu tài liệu, trang văn bản, ngày ban hành và thời hạn hiệu lực. Người dùng không có căn cứ để xác minh độ chính xác của thông tin.
2. **Quy trình vận hành thiếu bước kiểm chứng & trách nhiệm giải trình (Workflow & Accountability Gap):** Quy trình thao tác chuẩn (SOP) hiện tại không quy định việc tra cứu AI là bước hợp lệ, không phân định trách nhiệm khi xảy ra sai sót thông tin, dẫn tới rủi ro trách nhiệm đổ dồn lên nhân viên thực thi.

---

## 2. Framework đã dùng và Bằng chứng thực tế

### 2.1 Các Framework áp dụng
* **Gartner-Lite (Đánh giá mức sẵn sàng tổ chức):**
  * *Direction:* Xác định rõ mục tiêu giảm thời gian tra cứu từ 15 phút xuống ≤ 4 phút/yêu cầu.
  * *Readiness (Điểm nghẽn):* Dữ liệu tri thức nội bộ chưa được chuẩn hóa metadata, chưa phân định Data Owner chịu trách nhiệm cập nhật khi chính sách thay đổi.
  * *Absorption (Điểm nghẽn):* Chưa có cơ chế hỗ trợ vận hành (operational support) và quy trình tiếp nhận lỗi/học hỏi từ phản hồi của người dùng.
* **Mollick - Jagged Frontier (Phân chia công việc Người – AI):**
  * Thiết lập lại 3 ranh giới công việc:
    * *Con người giữ quyền:* Phê duyệt ngoại lệ, quyết định áp dụng chính sách cho các trường hợp rủi ro cao, chịu trách nhiệm pháp lý với khách hàng/đối tác.
    * *AI hỗ trợ, người kiểm tra:* AI đóng vai trò tìm kiếm, trích xuất đoạn văn bản, tóm tắt nội dung kèm trích dẫn văn bản; nhân viên vận hành đối chiếu nguồn và ký duyệt kết quả.
    * *AI tự động:* Chỉ áp dụng cho các FAQ chuẩn hóa tuyệt đối có mã hiệu quy định cố định.
* **ADKAR (Chẩn đoán điểm nghẽn người dùng):**
  * Xác định điểm nghẽn cốt lõi nằm ở **Desire (Động lực)**: Nhân viên từ chối dùng không phải vì "ngại học công nghệ mới" mà vì "sợ chịu trách nhiệm cá nhân nếu AI trả lời sai". Mở các lớp đào tạo kỹ năng prompt đơn thuần hoàn toàn không giải quyết được gốc rễ vấn đề này.

### 2.2 Bằng chứng thực tế thu thập được
* **Bằng chứng dữ liệu log:** Rà soát ngẫu nhiên 50 truy vấn gần nhất của hệ thống cho thấy **82% câu trả lời không kèm nguồn trích dẫn cụ thể hoặc ngày hiệu lực**, gây khó khăn cho việc tra cứu chéo.
* **Bằng chứng phỏng vấn người dùng:** 15/18 nhân viên vận hành được hỏi trả lời rằng họ gặp tình trạng AI trích dẫn các quy chế đã bị bãi bỏ từ năm trước, khiến họ mất niềm tin và quay lại hỏi đồng nghiệp.
* **Bài học đối sánh (Case Morgan Stanley & DWP/GDS):** Bài học từ Morgan Stanley cho thấy để chuyên viên tài chính tin dùng trợ lý tri thức, toàn bộ câu trả lời bắt buộc phải kèm đường dẫn trực tiếp tới văn bản nghiên cứu gốc đã qua kiểm duyệt. Triển khai AI mà thiếu kiến trúc tin cậy sẽ dẫn đến sự đào thải tự nhiên của người dùng.

---

## 3. Các thay đổi sau phản biện chéo từ Nhóm Chicken Plus (Changes in v2)

### 3.1 Góp ý nhận được từ Nhóm bạn (Chicken Plus)
* **Ưu điểm được ghi nhận:** Nhóm khóa phạm vi rõ ràng (DocBot RAG cho 35 nhân sự vận hành với 3 quy trình cụ thể); sử dụng chuẩn xác Gartner-Lite, Mollick, ADKAR để bóc tách triệu chứng "ít dùng" khỏi nguyên nhân gốc (thiếu kiến trúc tin cậy & thiếu bước kiểm chứng trong quy trình); quy trình AS-IS/TO-BE đủ 3 thay đổi bắt buộc; lộ trình 30-60-90 thiết kế theo cổng quyết định có owner rõ ràng; quyết định "Sửa trước khi mở rộng" hợp lý với bằng chứng hiện có.
* **Khuyết điểm cần khắc phục:**
  1. Sản phẩm còn lỗ hổng lớn về độ tin cậy do câu trả lời thiếu trích dẫn nguồn và ngày hiệu lực văn bản, khiến nhân viên không dám tin dùng.
  2. Quy trình chưa gắn bước kiểm chứng bắt buộc, chưa có nút báo lỗi và thiếu cơ chế chuyển giao (Fallback) khi AI không chắc chắn.
  3. Kho tài liệu thiếu metadata chuẩn hóa và chưa có Data Owner chịu trách nhiệm cập nhật.
  4. Hệ thống đo lường v1 chủ yếu đo thời gian ước tính, cần tăng cường đo lường kiểm soát rủi ro thực tế từ log hệ thống.

### 3.2 Ba thay đổi cải tiến cốt lõi trong Bản v2 (Đối chiếu với v1)
1. **Thay đổi 1 — Bổ sung cơ chế và chỉ số Rủi ro & Fallback (Tầng 6):** Bổ sung chỉ số *Tỷ lệ yêu cầu kích hoạt Fallback chuyển giao tự động sang Trưởng ca khi AI không chắc chắn (độ tin cậy < 80%)* với mục tiêu $\le 6\%$ từ log hệ thống `Fallback & Handoff Tickets`. Bổ sung nút báo lỗi (Report/Flag inaccurate info) trực tiếp trên giao diện bot.
2. **Thay đổi 2 — Chuyển từ khuyến khích sang chốt chặn kiểm chứng nguồn bắt buộc (Source Verification Gate ở Tầng 2):** Nâng mục tiêu *Tỷ lệ nhân viên thực hiện đối chiếu nguồn* từ $85\%$ lên $\ge 90\%$. Hệ thống UI bắt buộc người dùng click mở văn bản gốc để xem mã hiệu và ngày hiệu lực trước khi nút "Xác nhận hoàn thành SOP" được mở.
3. **Thay đổi 3 — Siết chặt chỉ số Năng suất và Chất lượng (Tầng 3 & Tầng 4):** 
   - Tối ưu hóa pipeline RAG để rút ngắn thời gian xử lý trung bình (AHT) từ mục tiêu 4.0 phút xuống $\le 3.5$ phút/yêu cầu.
   - Siết chặt ngưỡng Tỷ lệ làm lại (Rework Rate) $\le 3\%$; nếu chỉ số xấu, lập tức kích hoạt quy trình rà soát khẩn với Data Owner để bóc tách tài liệu hết hạn khỏi cơ sở dữ liệu vector.

---

## 4. Quyết định đề xuất: SỬA (REPAIR / PIVOT)

Nhóm đề xuất quyết định chính thức: **TẠM DỪNG MỞ RỘNG (PAUSE EXPANSION) VÀ SỬA ĐỔI TOÀN DIỆN HỆ THỐNG VẬN HÀNH TRƯỚC KHI TIẾP TỤC.**

* **Lý do cốt lõi:** Mức độ sử dụng thấp chỉ là triệu chứng; nguyên nhân gốc rễ nằm ở việc thiếu kiến trúc tin cậy và thiếu sự tích hợp trong quy trình SOP. Nếu tiếp tục cố gắng "ép chỉ tiêu sử dụng" hoặc "mở thêm lớp đào tạo", doanh nghiệp chỉ lãng phí ngân sách mà không tạo ra giá trị nghiệp vụ.

---

## 5. Giải pháp Quản trị & Lộ trình Cổng Quyết định 30–60–90 ngày (Phụ trách: Mai Hồng Sơn)

### 5.1 Thiết kế Kiến trúc Tin cậy (Trust Architecture) & 4 Điều kiện Mức sẵn sàng
Để giải quyết tận gốc nguyên nhân thiếu độ tin cậy và thiếu quy trình kiểm chứng, nhóm thiết lập chuỗi cơ chế kiểm soát khép kín:
$$\text{Nguồn dữ liệu chuẩn hóa} \rightarrow \text{Trích nguồn bắt buộc (Citations)} \rightarrow \text{QA kiểm tra mẫu} \rightarrow \text{Chuyển giao chuyên gia (Fallback)} \rightarrow \text{Vòng phản hồi (Feedback Loop)}$$

* **4 Điều kiện Mức sẵn sàng (Readiness Checklist) bắt buộc:**
  1. *Data Owner & Lịch cập nhật:* Bổ nhiệm Trưởng bộ phận Nghiệp vụ làm Data Owner; định kỳ rà soát kho văn bản vào ngày 25 hàng tháng; gán nhãn trạng thái hiệu lực (Có hiệu lực / Hết hiệu lực / Đang sửa đổi).
  2. *Quyền truy cập đúng đối tượng:* Phân quyền truy cập tài liệu theo cấp bậc ca trực và phân hệ nghiệp vụ, tránh lộ lọt văn bản nội bộ mật.
  3. *Phạm vi sử dụng rõ ràng:* Ban hành "Quy chế phạm vi Jagged Frontier" – chỉ định rõ những nghiệp vụ được dùng AI hỗ trợ và những tác vụ cấm dùng AI hoàn toàn.
  4. *Cơ chế quản trị & Chi phí:* Thiết lập hạn mức chi phí truy vấn Token RAG hàng tháng; thành lập Hội đồng QA mẫu gồm các Senior Operator.

---

### 5.2 Lộ trình 30–60–90 ngày: 3 Cổng Quyết định dựa trên Bằng chứng (Gate Decision Criteria)

Lộ trình được thiết kế theo nguyên tắc: **Chỉ chuyển giai đoạn khi đạt đủ bằng chứng tại Cổng nghiệm thu (Gate Criteria), không phụ thuộc vào tiến độ thời gian cơ học.**

```
[Ngày 0-30: Cổng 1] -------------> [Ngày 31-60: Cổng 2] -------------> [Ngày 61-90: Cổng 3]
  Chứng minh độ tin cậy &             Chứng minh chất lượng &              Đánh giá giá trị & 
    Chuẩn hóa dữ liệu                   Thử nghiệm nhóm nhỏ                 Quyết định mở rộng
```

| Giai đoạn & Tên Cổng | Hành động trọng tâm | Phụ trách chính (Owner) | Điều kiện nghiệm thu Cổng (Gate Exit Criteria) |
| :--- | :--- | :--- | :--- |
| **0 – 30 ngày**<br>*(Cổng 1: Chứng minh độ tin cậy & Chuẩn hóa)* | - Bổ sung tính năng trích xuất nguồn văn bản, số trang và ngày hiệu lực trên giao diện AI.<br>- Phân loại dữ liệu tri thức, dọn dẹp các tài liệu SOP cũ.<br>- Ban hành quy chế SOP v1 (bổ sung bước kiểm tra nguồn).<br>- Đo lường mốc ban đầu (Baseline) từ log thực tế. | **Mai Hồng Sơn** *(Lead Quản trị)* phối hợp cùng Đội AI / IT | - 100% tài liệu đưa vào RAG có metadata hợp lệ.<br>- Tỷ lệ câu trả lời có trích nguồn đạt ≥ 90% trên môi trường Staging.<br>- Baseline của 5 chỉ số được ghi nhận đầy đủ vào Dashboard. |
| **31 – 60 ngày**<br>*(Cổng 2: Chứng minh chất lượng & Thử nghiệm Pilot)* | - Triển khai Pilot trên nhóm hạt nhân (10 nhân viên vận hành).<br>- Kích hoạt nút báo lỗi (Report inaccurate citation) và cơ chế Fallback sang Trưởng ca khi AI không chắc chắn.<br>- Thực hiện QA mẫu 20% truy vấn ngẫu nhiên mỗi tuần.<br>- Theo dõi thời gian xử lý và tỷ lệ phải làm lại (Rework rate). | **Nguyễn Tuấn Vũ** *(Lead Quy trình)* phối hợp Trưởng ca Vận hành | - Tỷ lệ nhân viên thực hiện bước xác nhận nguồn đạt ≥ 85%.<br>- Tỷ lệ câu hỏi bị người dùng báo lỗi hoặc làm lại giảm xuống dưới 5%.<br>- Không phát sinh bất kỳ sự cố áp dụng nhầm văn bản hết hiệu lực. |
| **61 – 90 ngày**<br>*(Cổng 3: Quyết định mở rộng hay dừng lại)* | - Đối chiếu toàn bộ chỉ số thực tế trên Dashboard v2 với Target ban đầu.<br>- Thẩm định chi phí vận hành RAG so với giá trị thời gian và chi phí sai sót tiết kiệm được.<br>- Họp Hội đồng Chuyển đổi số & Lãnh đạo Vận hành để ra quyết định: Rollout chính thức 100% nhân sự / Tiếp tục sửa / Dừng. | **Hội đồng Vận hành & Lãnh đạo Chuyển đổi số** (Báo cáo bởi Sơn & Vũ) | - Thời gian trung bình tra cứu (AHT) giảm ≥ 60% (≤ 4 phút).<br>- Chi phí thiệt hại do sai lệch quy trình giảm ≥ 50%.<br>- Đạt 100% tiêu chí tuân thủ và sẵn sàng mở rộng cho toàn bộ 35 nhân sự. |

---

### 5.3 Kịch bản ứng phó khi không vượt qua Cổng (Fail-safe Rule)
* **Nếu không qua Cổng 1 (sau 30 ngày):** Tỷ lệ trích nguồn không đạt ≥ 90% $\rightarrow$ **DỪNG PILOT**, trả công cụ về cho Đội Kỹ thuật AI nâng cấp pipeline Chunking/Embedding và bộ lọc Metadata; duy trì tra cứu thủ công an toàn.
* **Nếu không qua Cổng 2 (sau 60 ngày):** Tỷ lệ làm lại vẫn > 10% hoặc nhân viên tiếp tục bỏ qua việc kiểm chứng $\rightarrow$ **SỬA ĐỔI QUY TRÌNH**, tích hợp chặn cưỡng bức trên phần mềm (bắt buộc mở link kiểm tra nguồn mới kích hoạt nút Submit tác vụ).
