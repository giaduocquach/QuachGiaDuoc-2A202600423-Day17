# Day 17 Submission - Phiên bản B
**Student:** Quách Gia Được (MSSV: 2A202600423)
**Date:** 24/04/2026
**Product idea:** Hệ thống AI phân tích văn bản CV và mô tả công việc (JD) để chỉ ra các lỗ hổng kỹ năng thực chiến và đề xuất lộ trình học tập tối ưu cho sinh viên IT.

## 1. MVP Boundary Sheet
**Riskiest Assumption:**
> "Tín hiệu 'Lỗ hổng kỹ năng' do AI chỉ ra đủ sức thuyết phục để sinh viên kiên trì đầu tư thời gian vào lộ trình học thay vì tiếp tục rải CV mù quáng."

**In-Scope** (tối đa 3):
- [x] Giao diện tải file CV (PDF) và ô nhập liệu văn bản JD thủ công (Copy-paste).
  *test giả định:* Sinh viên chấp nhận thao tác thủ công để nhận được đánh giá chuyên sâu.
- [x] Module LLM trích xuất kỹ năng và phân tích Semantic Matching để xuất báo cáo "Work-readiness Score" (Điểm sẵn sàng làm việc).
  *test giả định:* Việc chấm điểm số cụ thể tạo ra động lực để người dùng muốn cải thiện CV.
- [x] Lộ trình lấp đầy khoảng trống: Đề xuất 03 kỹ năng ưu tiên kèm danh sách khóa học uy tín lấy từ một CSDL tĩnh (Curated List).
  *test giả định:* Sinh viên tin tưởng vào danh sách đã được kiểm duyệt hơn là tự đi tìm kiếm khóa học trôi nổi.

**Out-of-Scope:**
- Tự động crawl dữ liệu JD từ các link trang tuyển dụng.
  *lý do bỏ:* Rủi ro bị block IP và làm phình to quy mô dự án (Scope creep) không cần thiết.
- Quét mã nguồn từ GitHub.
  *lý do bỏ:* Tốn kém chi phí API và nhiều sinh viên không có GitHub public chuẩn chỉnh, làm giảm tập khách hàng MVP.

**Non-Goals:**
- KHÔNG làm công cụ thiết kế template CV (CV Builder).
- KHÔNG tự sản xuất video bài giảng (Không làm nền tảng E-learning).

## 2. PRD Skeleton
### Problem Statement
> Sinh viên IT hiện nay rải CV mù quáng mà không biết mình bị loại vì thiếu kỹ năng gì, trong khi JD thực tế yêu cầu nhiều kỹ năng thực chiến (Docker, Clean Code, Testing) mà trường học ít dạy. Sự lệch pha văn bản này khiến họ học lan man và kéo dài thời gian thất nghiệp.

### Target User
> Sinh viên IT năm cuối và Fresher đang rải CV nhưng có tỷ lệ phản hồi hồ sơ thấp.

### User Stories
**Story 1:**
> As a Sinh viên IT sắp tốt nghiệp, I want AI đối chiếu những công nghệ trong CV của tôi với JD mục tiêu, so that tôi biết mình cần ưu tiên bổ sung framework nào để không bị loại ở vòng lọc hồ sơ.

**Story 2:**
> As a Fresher đang bị hổng kỹ năng, I want nhận được danh sách khóa học uy tín đã được xác thực, so that tôi yên tâm đăng ký học mà không sợ gặp phải các khóa học kém chất lượng.

### Al-Specific
**Model Selection:**
- Model: GPT-4o-mini.
- Lý do chọn: Phù hợp nhất cho tác vụ phân tích ngôn ngữ tự nhiên (NLP) để bóc tách keyword và hiểu ngữ cảnh kỹ năng IT với chi phí tối ưu (Unit economics) cho tệp người dùng sinh viên.
- Trade-offs chấp nhận: Khả năng hiểu logic kỹ thuật siêu sâu thấp hơn model lớn, nhưng đủ dùng cho việc matching văn bản.

**Data Requirements:**
- Nguồn: PDF CV từ người dùng, Text JD dán vào, và Danh sách 100 khóa học được curate sẵn.
- Owner: Người dùng sở hữu dữ liệu CV; hệ thống không lưu trữ file lâu dài.
- Update frequency: Xử lý tức thời trong từng session.

**Fallback UX:**
- Chiến lược: Expectation Management & Human-in-the-loop.
- Trigger: Khi AI gặp các công nghệ quá mới (Confidence score thấp) hoặc nhận diện sai do CV viết tắt.
- Hành động: Hiển thị cảnh báo "Kết quả so khớp có thể thiếu sót do CV chứa thuật ngữ không phổ biến".
- User options: Cho phép người dùng bấm nút "Bổ sung kỹ năng thủ công" để tự gõ thêm các kỹ năng mình có và yêu cầu AI chấm điểm lại.

### Success Metrics
- Primary metric: Tỷ lệ người dùng bấm nút "Lưu lộ trình vá kỹ năng".
- Ngưỡng thành công: > 25% user thực hiện hành động này.
- Timeframe đo lường: 30 ngày sau launch.

## 3. Hypothesis Table
### Hypothesis 1 
> "Chúng tôi tin rằng [Việc giới hạn AI chỉ đề xuất đúng 3 kỹ năng cốt lõi kèm khóa học từ danh sách đã kiểm duyệt] sẽ giúp [Sinh viên IT] đạt được [sự tập trung để bắt đầu học ngay]. 
> Chúng tôi sẽ biết mình đúng khi thấy [Tỷ lệ click chuột (CTR) vào link khóa học] đạt [>20%] trong [4 tuần]."

Riskiest assumption: Sinh viên có động lực click đi học ngay lập tức sau khi xem báo cáo.
Cách test cheapest: Khảo sát 50 sinh viên bằng báo cáo Skill-gap tĩnh làm thủ công, đo lường tỷ lệ họ thực sự click đăng ký khóa học.

## 4. PMF Scorecard
**Aha Moment:**
> Khoảnh khắc sinh viên nhận ra: "Hóa ra mình trượt CV vì JD yêu cầu cả CI/CD và Unit Test mà CV mình chỉ ghi mỗi Java Spring Boot".

**Actionable Metric:**
> Tỷ lệ quay lại hệ thống (Retention Rate D30) để upload CV phiên bản 2 sau khi đã update kỹ năng mới.

**PMF Method:**
> Sean Ellis Test (40% Rule) & Retention Curve.

**Vanity Metrics tôi sẽ không dùng:**
- Số lượng tài khoản đăng ký mới; Số lượt tạo báo cáo.

## 5. Al Critique Log
**Điểm Al chỉ ra khi Stress-Test:**
1. **[AI Fallback Holes]** AI ở Ver A có nguy cơ Hallucinate ra các khóa học ảo hoặc gợi ý các khóa học kém chất lượng trên mạng.
   *Action: Accept.* Bổ sung rào cản In-scope trong Ver B: AI không được tự chế khóa học, chỉ được phép match kỹ năng thiếu với một CSDL tĩnh (Curated List 100 khóa học) do chuyên gia chọn lọc.
2. **[Scope Creep]** Nếu JD yêu cầu quá nhiều kỹ năng, AI sẽ liệt kê một danh sách dài dằng dặc khiến sinh viên nản chí.
   *Action: Accept.* Giới hạn tính năng trong Ver B: Ép AI chỉ được xuất tối đa 03 kỹ năng ưu tiên nhất để lấp đầy khoảng trống.
3. **[Vanity Metric Trap]** "Số lượng báo cáo tạo ra" và "Số người đăng ký" (Ver A) không chứng minh sinh viên nhận được giá trị thực.
   *Action: Accept.* Thay đổi Actionable Metric và PMF sang "Tỷ lệ quay lại upload CV phiên bản 2 (D30 Retention)", bởi vì người dùng chỉ quay lại upload CV mới khi họ đã thực sự tiếp thu lời khuyên và vá được lỗi.

**Thay đổi lớn nhất giữa Version A và Version B:**
> Chuyển đổi từ tư duy "công cụ liệt kê mọi lỗi sai" sang "công cụ định hướng tập trung", khắc phục hoàn toàn rủi ro Hallucinate của LLM bằng việc sử dụng CSDL khóa học tĩnh, và định hình lại thước đo thành công dựa trên sự tiến bộ của người dùng (Retention) thay vì các chỉ số tăng trưởng ảo.

## 6. Self-assessment
**Mắt xích nào trong [MVP Boundary | PRD | Hypothesis | PMF] bạn đang yếu nhất?**
> **PMF**. Hành vi học tập để bổ sung kỹ năng thường kéo dài (2-4 tuần), nên việc thu thập đủ dữ liệu để chứng minh PMF qua Retention Curve trong ngắn hạn là một thử thách lớn.

**Open questions bạn muốn giải đáp tiếp:**
1. Làm thế nào để tạo ra các "Micro-value" trong 7 ngày đầu để giữ chân user trong lúc họ đang tham gia các khóa học dài hạn?
2. Có thể tích hợp hệ thống này với các nền tảng LMS của trường đại học để tự động hóa việc vá kỹ năng ngay trong chương trình học không?