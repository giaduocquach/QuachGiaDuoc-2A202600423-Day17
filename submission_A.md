# Day 17 Submission - Phiên bản A
**Student:** Quách Gia Được (MSSV: 2A202600423)
**Date:** 24/04/2026
**Product idea:** Hệ thống AI phân tích tương quan văn bản giữa CV và mô tả công việc (JD) để chỉ ra lỗ hổng kỹ năng thực chiến và đề xuất khóa học bù đắp cho sinh viên IT.

## 1. MVP Boundary Sheet
**Riskiest Assumption:**
> [cite_start]Sinh viên IT sẽ thực sự tin tưởng và dành thời gian học các kỹ năng được AI gợi ý, thay vì bỏ qua và tiếp tục rải CV mù quáng[cite: 809, 896].

[cite_start]**In-Scope** (tối đa 3)[cite: 813]:
- [x] Tính năng tải lên file CV (PDF) và ô dán nội dung văn bản JD.
- [x] Module LLM đối chiếu từ khóa và ngữ nghĩa (Semantic matching) giữa CV và JD để trích xuất báo cáo "Skill-gap".
- [x] Tính năng hiển thị danh sách kỹ năng IT còn thiếu kèm text gợi ý tên khóa học.

**Out-of-Scope:**
- Tính năng AI tự động sửa hoặc viết lại nội dung file CV cho người dùng. (Lý do: Không tập trung vào lõi giải quyết skill-gap) [cite_start][cite: 813].
- Tích hợp link mua khóa học trực tiếp (Affiliate). (Lý do: MVP chỉ cần test nhu cầu học, chưa cần test khả năng thanh toán ngay).

**Non-Goals:**
- [cite_start]KHÔNG làm công cụ thiết kế đồ họa template CV (CV Builder)[cite: 813].
- KHÔNG làm sàn giao dịch tuyển dụng (Job Board).

## 2. PRD Skeleton
### Problem Statement
> Sinh viên IT (Web, Mobile, System...) thường rải CV hàng loạt nhưng bị loại ngay vòng lọc hồ sơ do thiếu kỹ năng thực chiến sát với JD. [cite_start]Họ không nhận được phản hồi từ HR nên không biết mình hổng ở đâu, gây lãng phí thời gian và thất nghiệp kéo dài[cite: 832, 833, 834].

### Target User
> [cite_start]Sinh viên IT năm cuối và Fresher (dưới 1 năm kinh nghiệm) đang trong quá trình tìm việc nhưng tỷ lệ pass CV thấp[cite: 835, 836].

### User Stories
**Story 1:**
> [cite_start]As a Sinh viên IT đang tìm việc, I want đối chiếu nhanh CV của tôi với một JD cụ thể, so that tôi biết mình đang thiếu những từ khóa kỹ thuật hay công cụ nào[cite: 839, 840].
**Story 2:**
> As a người dùng đang bị hổng kỹ năng, I want nhận được gợi ý các khóa học ngắn hạn, so that tôi có thể tập trung học bù đúng trọng tâm.

### Al-Specific
**Model Selection:**
- [cite_start]Model: GPT-4o-mini[cite: 855].
- [cite_start]Lý do chọn: Xử lý văn bản nhanh, chi phí rẻ, đủ sức mạnh phân tích ngữ nghĩa các thuật ngữ IT (ví dụ hiểu "React" liên quan đến "Frontend")[cite: 858, 859].
- Trade-offs chấp nhận: Có thể bỏ sót các công nghệ quá ngách hoặc từ viết tắt lạ.

**Data Requirements:**
- [cite_start]Nguồn: File PDF CV và văn bản JD do user chủ động nhập vào[cite: 861, 862].
- Update frequency: Tức thời theo từng session sử dụng.

**Fallback UX:**
- [cite_start]Chiến lược: Expectation Management (Quản trị kỳ vọng)[cite: 871, 872].
- Trigger: Khi AI không thể đọc được văn bản từ file PDF do lỗi font.
- Hành động: Báo lỗi "Hệ thống không thể trích xuất văn bản từ file này".
- User options: Hướng dẫn người dùng copy-paste text CV thủ công vào ô nhập liệu để thử lại.

### Success Metrics
- [cite_start]Primary metric: Tỷ lệ người dùng bấm vào xem chi tiết các kỹ năng được gợi ý[cite: 846, 848].
- Ngưỡng thành công: > 20% user thực hiện trong session đầu tiên.

## 3. Hypothesis Table
### Hypothesis 1 
> "Chúng tôi tin rằng [Báo cáo Skill-gap] sẽ giúp [Sinh viên IT] đạt được [việc xác định đúng kỹ năng thực chiến cần học]. 
> [cite_start]Chúng tôi sẽ biết mình đúng khi thấy [Tỷ lệ click xem chi tiết lộ trình học] đạt [>20%] trong [14 ngày]." [cite: 891, 892, 893, 894]

## 4. PMF Scorecard
**Aha Moment:**
> [cite_start]Khoảnh khắc sinh viên nhận ra CV của họ bị loại vì thiếu các công nghệ "ngoại vi" bắt buộc trong JD (như Docker, Unit Test) mà trường đại học không dạy[cite: 930, 931].
**Actionable Metric:**
> [cite_start]Tỷ lệ người dùng bấm nút "Lưu lộ trình vá kỹ năng"[cite: 940, 941].
**PMF Method:**
> [cite_start]Sean Ellis Test (>40% trả lời "Rất thất vọng" nếu không còn sản phẩm)[cite: 904, 912].