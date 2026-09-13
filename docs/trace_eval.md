# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Phạm Quang Đạt  
> **Mã Sinh Viên / Mã Học viên:** 2A202602704  
> **Chủ đề Lựa chọn:** *Trợ lý Học vụ VinUni:* Tra cứu hồ sơ, GPA, thông tin cố vấn và hỗ trợ đặt lịch tư vấn học vụ.  

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | **4 / 5** | Bài toán có những yêu cầu cần chia thành nhiều bước, ví dụ tra cứu hồ sơ để xác định cố vấn, sau đó dùng thông tin cố vấn để đặt lịch. Trong phiên bản hiện tại, luồng đơn bước đã hoạt động ổn định; luồng đa bước TC04 mới hoàn thành bước tra cứu đầu tiên. |
| **2. Tool Interaction** | **5 / 5** | Hệ thống cần kết nối MCP Server và nguồn dữ liệu học vụ thay vì chỉ dựa vào kiến thức có sẵn của LLM. Agent phải gọi Tool tra cứu để lấy GPA, lịch thi, thông tin cố vấn và gọi Tool hành động để đặt lịch tư vấn; nhờ đó câu trả lời dựa trên dữ liệu thực tế và hạn chế hallucination. |
| **3. Dynamic Decision** | **4 / 5** | Về thiết kế, bước tiếp theo phụ thuộc vào Observation của bước trước. Phiên bản hiện tại đã xử lý được kết quả SUCCESS và NOT_FOUND, nhưng chưa tiếp tục gọi Tool thứ hai sau Observation trong TC04. |
| **4. Long Horizon Goal** | **4 / 5** | Agent cần duy trì xuyên suốt mục tiêu hỗ trợ đúng sinh viên và giữ nhất quán mã sinh viên, nhu cầu học vụ, lịch thi, thời gian hẹn và tên cố vấn qua nhiều bước ReAct. Tuy nhiên, mỗi yêu cầu thường hoàn tất trong một phiên tương đối ngắn và không cần theo dõi kéo dài nhiều ngày nên chấm 4/5. |
| **TỔNG ĐIỂM AGENTIC FIT** | **17 / 20** | **17/20 > 12/20:** Chủ đề Trợ lý Học vụ rất phù hợp triển khai Agentic System vì cần phối hợp suy luận nhiều bước, tương tác công cụ qua MCP và điều chỉnh hành động dựa trên dữ liệu quan sát. |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Dán 1 đoạn trích xuất log tiêu biểu từ file `docs/trace_waterfall.json` sinh ra từ phản hồi LLM API thật:

```json
[
  {
    "step": 1,
    "query": "Chào bạn, bạn có thể giới thiệu quy chế học vụ cơ bản của Đại học VinUni không?",
    "action_type": "FINAL_ANSWER",
    "thought": "Gemini phản hồi trực tiếp bằng văn bản (không cần gọi công cụ).",
    "output": "Chào bạn! Là Trợ lý Học vụ của Đại học VinUni, tôi xin giới thiệu tóm tắt các quy chế học vụ cơ bản và quan trọng nhất mà sinh viên VinUni cần lưu ý:\n\n### 1. Hệ thống Tín chỉ & Đăng ký Môn học\n* **Đơn vị học trình:** VinUni áp dụng hệ thống tín chỉ (credits). Mỗi môn học thường có thời lượng từ 1 đến 4 tín chỉ.\n* **Tải học tập tiêu chuẩn (Course Load):** \n  * Học kỳ chính (Thu/Xuân): Sinh viên thường đăng ký từ **12 đến 18 tín chỉ** (tối đa có thể lên tới 20-21 tín chỉ nếu có sự chấp thuận của Viện trưởng/Cố vấn học tập và đạt GPA cao).\n  * Học kỳ hè (Summer Term - nếu có): Thường tối đa **6 - 8 tín chỉ**.\n\n### 2. Hệ thống Đánh giá & Thang điểm (Grading System)\nVinUni sử dụng thang điểm chữ (Letter Grade) và quy đổi sang thang điểm 4.0 để tính điểm trung bình tích lũy (GPA):\n* **A / A-**: Xuất sắc (4.0 / 3.7)\n* **B+ / B / B-**: Tốt (3.3 / 3.0 / 2.7)\n* **C+ / C / C-**: Khá/Trung bình khá (2.3 / 2.0 / 1.7)\n* **D+ / D**: Trung bình/Yếu (1.3 / 1.0) - Điểm đạt thấp nhất để được tính tín chỉ.\n* **F**: Không đạt (0.0) - Sinh viên phải học lại môn này.\n\n### 3. Đánh giá Học vụ (Academic Standing)\nCuối mỗi học kỳ chính, kết quả học tập của sinh viên sẽ được đánh giá:\n* **Tình trạng Bình thường (Good Standing):** GPA học kỳ và GPA tích lũy (CGPA) đạt từ **2.0/4.0** trở lên.\n* **Cảnh cáo Học vụ (Academic Probation):** Sinh viên sẽ bị rơi vào tình trạng cảnh cáo học vụ nếu GPA học kỳ hoặc CGPA dưới **2.0**. Sinh viên bị cảnh cáo học vụ sẽ bị giới hạn số tín chỉ đăng ký ở kỳ tiếp theo và phải làm việc chặt chẽ với Cố vấn học tập (Academic Advisor).\n* **Buộc thôi học (Academic Dismissal):** Áp dụng nếu sinh viên không cải thiện được tình trạng cảnh cáo học vụ sau số học kỳ quy định (thường là 2 học kỳ liên tiếp bị cảnh cáo) hoặc vi phạm nghiêm trọng quy chế.\n\n### 4. Liêm chính Học thuật (Academic Integrity)\nVinUni đặt tiêu chuẩn rất cao về liêm chính học thuật. Các hành vi sau đây bị nghiêm cấm:\n* Gian lận trong thi cử, kiểm tra.\n* Đạo văn (Plagiarism) - sử dụng ý tưởng, lời văn của người khác mà không trích dẫn nguồn rõ ràng.\n* Tự đạo văn (gộp một bài làm cho nhiều môn học khác nhau mà không xin phép).\n* Hỗ trợ người khác gian lận.\n*Mọi vi phạm sẽ bị xử lý nghiêm khắc bởi Hội đồng Kỷ luật Học thuật (Academic Integrity Committee).*\n\n### 5. Điều kiện Tốt nghiệp\nĐể được xét tốt nghiệp, sinh viên cần đáp ứng:\n* Hoàn thành đầy đủ số lượng tín chỉ yêu cầu của chương trình đào tạo.\n* Đạt điểm trung bình tích lũy (CGPA) tối thiểu từ **2.0/4.0** trở lên.\n* Đạt chuẩn đầu ra về năng lực ngoại ngữ (Tiếng Anh) theo quy định của trường.\n* Hoàn thành các yêu cầu về thực tập (Internship), đồ án tốt nghiệp (Capston Project) và các hoạt động ngoại khóa/phát triển toàn diện (Co-curricular activities) nếu chương trình yêu cầu.\n\nNếu bạn cần tra cứu thông tin học vụ cá nhân của mình (như điểm số, tiến độ học tập) hoặc muốn đặt lịch hẹn với Cố vấn học tập, vui lòng cung cấp **Mã sinh viên (Student ID)** để tôi có thể hỗ trợ bạn nhanh nhất nhé!",
    "latency_ms": 11173.33
  },
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "TOOL_EXECUTION",
    "tool_name": "academic_query",
    "thought": "Gemini quyết định gọi công cụ 'academic_query' với tham số: {\"student_id\": \"SV2026001\"}",
    "arguments": {
      "student_id": "SV2026001"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026001",
      "data": {
        "full_name": "Nguyễn Văn An",
        "class": "AI-K4",
        "gpa": 3.85,
        "email": "an.nv@vinuni.edu.vn",
        "status": "Đang học",
        "advisor": "PGS.TS Nguyễn Văn A"
      }
    },
    "latency_ms": 1902.06
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin học vụ của sinh viên SV2026001.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tổng hợp kết quả từ MCP Server thành công.",
    "output": "Kết quả tra cứu cho sinh viên SV2026001 (Nguyễn Văn An): Lớp AI-K4, GPA: 3.85, Email: an.nv@vinuni.edu.vn, Trạng thái: Đang học, Cố vấn: PGS.TS Nguyễn Văn A.",
    "latency_ms": 10.0
  }
]
```
### Ghi chú về TC04 — Multi-step Reasoning

TC04 yêu cầu thực hiện hai hành động liên tiếp:

1. Gọi `academic_query` để tra cứu cố vấn của sinh viên `SV2026002`.
2. Gọi `schedule_appointment` với tên cố vấn nhận được từ Observation.

Trong lần nghiệm thu hiện tại, Agent đã gọi thành công `academic_query` và nhận được cố vấn `TS. Lê Thị B`. Tuy nhiên, hàm `run_react_agent()` trong `src/app.py` đang tổng hợp Final Answer và kết thúc vòng lặp ngay sau Tool Call đầu tiên, nên `schedule_appointment` chưa được gọi ở bước tiếp theo.

Do giới hạn trên, TC04 được ghi nhận là **hoàn thành một phần**, không tính là Test Case thành công hoàn toàn.


---

| Test Case | Kết quả | Số Tool Call | Ghi chú |
| :--- | :---: | :---: | :--- |
| TC01 | PASS | 0 | Trả lời trực tiếp, không cần Tool. |
| TC02 | PASS | 1 | Gọi đúng `academic_query` cho `SV2026001`. |
| TC03 | PASS | 1 | Gọi đúng `schedule_appointment`. |
| TC04 | PARTIAL | 1 | Tra cứu đúng cố vấn của `SV2026002`, nhưng chưa gọi Tool đặt lịch ở bước tiếp theo. |
| TC05 | PASS | 1 | Nhận và xử lý đúng kết quả `NOT_FOUND`. |

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [x] Đã cấu hình API Key thật và chạy Test Suite với Gemini.
- **Tổng số Test Cases thành công hoàn toàn:** 4 / 5.
- **Test Case hoàn thành một phần:** TC04.
- **Tổng số lượt gọi Tool qua MCP Server ghi nhận trong trace:** 4 lượt.
- **Tool Call còn thiếu:** `schedule_appointment` ở bước thứ hai của TC04.
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!
