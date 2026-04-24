# Day 17 Submission - Phiên bản A
**Student:** Quách Gia Được (MSSV: 2A202600423)
**Date:** 24/04/2026
**Product idea:** Hệ thống AI phân tích văn bản CV và mô tả công việc (JD) để chỉ ra các lỗ hổng kỹ năng thực chiến và đề xuất khóa học tương ứng cho sinh viên IT.

## 1. MVP Boundary Sheet
**Riskiest Assumption:**
> Sinh viên IT sẽ thực sự tin tưởng và dành thời gian học theo các kỹ năng mà AI chỉ ra thay vì tiếp tục rải CV mù quáng.

**In-Scope** (tối đa 3):
- [x] Tính năng tải lên file CV (PDF) và ô dán nội dung văn bản JD.
  *Test giả định:* Sinh viên sẵn sàng cung cấp dữ liệu cá nhân và thông tin công việc để được phân tích.
- [x] Module LLM đối chiếu từ khóa và ngữ nghĩa giữa CV và JD để trích xuất báo cáo "Skill-gap".
  *Test giả định:* LLM có thể phát hiện chính xác các từ khóa kỹ năng IT bị thiếu dựa trên văn bản.
- [x] Hiển thị danh sách kỹ năng còn thiếu kèm tên các đầu mục kiến thức/khóa học cần bổ sung.
  *Test giả định:* Người dùng quan tâm đến việc nhận được chỉ dẫn cụ thể để cải thiện hồ sơ.

**Out-of-Scope:**
- Tính năng AI tự động sửa hoặc viết lại nội dung CV.
  *Lý do bỏ:* Làm mất đi trọng tâm là chỉ ra lỗ hổng để học bù, biến sản phẩm thành một công cụ xào nấu văn bản.
- Tích hợp link mua khóa học trực tiếp (Affiliate).
  *Lý do bỏ:* Quá phức tạp cho MVP, ưu tiên kiểm chứng việc người dùng có muốn học hay không trước.

**Non-Goals:**
- Không làm công cụ thiết kế đồ họa hay cung cấp template CV (CV Builder).
- Không làm sàn giao dịch tuyển dụng cho doanh nghiệp (Job Board).

## 2. PRD Skeleton
### Problem Statement
> Sinh viên IT (Web, Mobile, System...) thường rải CV hàng loạt nhưng bị loại ngay từ vòng lọc hồ sơ vì không biết cách đối chiếu năng lực bản thân với yêu cầu thực tế của JD, gây lãng phí thời gian và thất nghiệp kéo dài.

### Target User
> Sinh viên IT năm cuối và Fresher đang trong quá trình tìm việc nhưng tỷ lệ phản hồi hồ sơ thấp.

### User Stories
**Story 1:**
> As a Sinh viên IT đang tìm việc, I want đối chiếu nhanh CV của tôi với một JD cụ thể, so that tôi biết mình đang thiếu những từ khóa kỹ thuật hay kỹ năng thực chiến nào.

**Story 2:**
> As a người dùng đang bị hổng kỹ năng, I want nhận được gợi ý các kỹ năng cần học bù, so that tôi có thể tập trung nâng cấp Portfolio đúng trọng tâm.

### Al-Specific
**Model Selection:**
- Model: GPT-4o-mini
- Lý do chọn: Xử lý văn bản nhanh, chi phí rẻ, đủ mạnh để hiểu sự tương đồng ngữ nghĩa giữa các thuật ngữ IT (ví dụ hiểu ReactJS thuộc Frontend).
- Trade-offs chấp nhận: Đôi khi có thể bỏ sót các công nghệ quá ngách hoặc tên framework viết tắt lạ.
- Trade-offs không chấp nhận: Tự bịa (hallucinate) ra các kỹ năng ảo không có thật.

**Data Requirements:**
- Nguồn: File PDF CV và văn bản JD do user nhập vào.
- Owner: User sở hữu dữ liệu cá nhân.
- Update frequency: Tức thời theo yêu cầu.

**Fallback UX:**
- Chiến lược: Expectation Management (Quản trị kỳ vọng).
- Trigger: Khi AI không thể trích xuất văn bản từ file PDF do lỗi font hoặc file ảnh.
- Hành động: Hiển thị thông báo "Hệ thống không thể đọc được định dạng file này".
- User options: Yêu cầu người dùng copy-paste text CV trực tiếp vào ô nhập liệu để thử lại.

### Success Metrics
- Primary metric: Tỷ lệ người dùng bấm vào xem danh sách kỹ năng gợi ý.
- Ngưỡng thành công: > 20% user thực hiện.
- Timeframe đo lường: 7 ngày đầu.

### Dependencies & Constraints
- Phụ thuộc vào chất lượng trích xuất văn bản từ các thư viện đọc PDF mã nguồn mở.

## 3. Hypothesis Table
### Hypothesis 1 (cho tính năng In-Scope #2)
> "Chúng tôi tin rằng [Module LLM đối chiếu từ khóa] sẽ giúp [Sinh viên IT] đạt được [việc xác định đúng kỹ năng thực chiến cần học].
> Chúng tôi sẽ biết mình đúng khi thấy [Tỷ lệ user click xem chi tiết kỹ năng hổng] đạt [>20%] trong [14 ngày]."

Riskiest assumption: Kết quả AI chỉ ra đủ chính xác và có ý nghĩa đối với sinh viên.
Cách test cheapest: Dùng Wizard of Oz, cho sinh viên nộp CV và JD, team tự chấm bằng mắt và trả kết quả để xem phản ứng.

## 4. PMF Scorecard
**Aha Moment:**
> Khoảnh khắc sinh viên nhận ra CV của họ trượt vì thiếu các kỹ năng "ngoại vi" bắt buộc trong JD (như Docker, Unit Test) mà họ chưa từng để ý.

**Actionable Metric:**
> Tỷ lệ người dùng thực hiện hành động "Lưu lộ trình vá kỹ năng".

**PMF Method:**
> Sean Ellis Test
> Ngưỡng thành công: >40% trả lời "Very disappointed" (Rất thất vọng nếu không còn sản phẩm).

**Vanity Metrics tôi sẽ không dùng:**
- Số lượng lượt truy cập vào trang web; Tổng số CV được tải lên nền tảng.
