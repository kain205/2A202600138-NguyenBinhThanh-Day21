# Output: rules_rails_ritual_v2.md
**Startup:** Synapsy
**Risk lớn nhất:** Rò rỉ dữ liệu nhạy cảm (tài liệu ôn thi của sinh viên, khóa API, mã nguồn) do thành viên team sử dụng các công cụ LLM public không an toàn, vi phạm chính sách bảo mật và làm mất lòng tin của người dùng.

## R1 — RULES
* **Cấm cụ thể:** KHÔNG được paste customer chat, source code dự án, dữ liệu file gốc của sinh viên, hoặc API keys vào các nền tảng AI public (như ChatGPT bản miễn phí).
* **Allowed alternative:** Bắt buộc DÙNG API/OpenAI Enterprise (đã setup chính sách data không train) cho các tác vụ xử lý nội dung và DÙNG Cursor + Copilot trực tiếp trong IDE để hỗ trợ code.
* **Hậu quả vi phạm:** Vi phạm lần 1 sẽ bị nhắc nhở trực tiếp (Founder talk 1-1). Tái phạm lần 2 sẽ buộc phải rời dự án (let go).
* **Update mechanism:** Quy định "AI Safety Rules — v1" được Founder viết ngắn gọn trên 1 trang Notion (team đọc xong trong 2 phút). Cập nhật ngay khi có thay đổi công nghệ thay vì đợi review hàng quý.

## R2 — RAILS
* **Tool 1: Chặn API key vào git.** Sử dụng `git-secrets` OSS hoặc cấu hình pre-commit hooks để tự động chặn việc commit nhầm khóa API (MinerU, OpenAI, Firebase) vào kho lưu trữ mã nguồn. Cost: $0.
* **Tool 2: Log mọi LLM prompt/response.** Tích hợp Helicone (gói Free tier hỗ trợ 100K requests/tháng) để giám sát và audit toàn bộ luồng dữ liệu vào/ra giữa pipeline của Synapsy và các mô hình LLM. Cost: $0–50/tháng.
* *Self-check:* Tổng chi phí Rails duy trì ở mức $0/tháng (Hoàn toàn thỏa mãn yêu cầu Stack startup-friendly <$500/tháng).

## R3 — RITUAL
* **Ritual (Hàng tuần):** "Friday 30' Risk Review" kết hợp "Customer Friday". Chiều Thứ Sáu, Founder dành 30 phút rà soát lại hệ thống (có hallucination không, vendor thay đổi gì không) và nhấc máy gọi cho 1 khách hàng thực tế.
* **Question (Founder hỏi customer):** "Tuần qua bạn dùng, AI có nói sai chỗ nào không?".
* *Self-check:* Không công cụ nào thay được việc Founder trực tiếp hỏi khách hàng. Ritual này chi phí bằng không và có thể đưa vào vận hành ngay Thứ Sáu tuần này.