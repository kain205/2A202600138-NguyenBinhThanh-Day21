# Incident Playbook — AI Nói Sai Thông Tin
> Dành cho tình huống: AI của Synapsy output sai → viral trên mạng xã hội
> Dùng được lúc 3h sáng, mắt díu, tay run. Làm theo thứ tự. Không bỏ bước.

---

## Bước 1 — Verify (3 phút)

**Mục tiêu: Xác nhận thật/giả trước khi làm bất cứ điều gì.**

### Check log ở đâu?

1. **Helicone dashboard** → filter theo `timestamp` ± 30 phút quanh giờ tweet
   - URL: `helicone.ai/dashboard` → tab Requests → search `user_id` nếu biết
   - Tìm: `prompt`, `completion`, `model`, `latency`, `status`

2. **Nếu không có Helicone:** Firestore → collection `ai_logs` → filter `createdAt` → export JSON

3. **OpenAI dashboard** (last resort): `platform.openai.com` → Usage → không có full content, chỉ có token count → **không đủ để verify**

### Verify thật vs Photoshop — checklist 60 giây:

- [ ] Screenshot có `timestamp` khớp với log không? (sai ±2 phút = suspicious)
- [ ] `session_id` hoặc `user_id` trong screenshot có trong database không?
- [ ] Font, bubble chat, spacing có đúng với UI hiện tại của Synapsy không?
- [ ] Check IP / device log của account đó có active lúc đó không?

**Nếu không verify được trong 3 phút:** Assume thật và tiếp tục. Sai thì xin lỗi sau — còn hơn im lặng khi thật.

---

## Bước 2 — Stop the Bleeding (5 phút)

### 4 options:

| Option | Hành động | Khi nào dùng |
|--------|-----------|--------------|
| **Hard kill** | Tắt toàn bộ AI response, thay bằng "Tính năng đang bảo trì" | Lỗi nghiêm trọng, có thể gây hại sức khỏe (sai liều thuốc, chống chỉ định) |
| **Soft limit** | Giữ AI nhưng thêm disclaimer bắt buộc: "Nội dung do AI tạo — hãy đối chiếu với tài liệu gốc" | Lỗi nhỏ, factual error nhưng không nguy hiểm |
| **Block** | Block tính năng cụ thể bị lỗi (vd: chỉ tắt Diagnostic Quiz, giữ Flashcard) | Lỗi cô lập trong 1 feature, feature khác không ảnh hưởng |
| **Tighten prompt** | Deploy hotfix prompt: thêm constraint "Chỉ dùng thông tin từ tài liệu đã upload, không suy luận ngoài" | Lỗi do AI hallucinate ngoài file — phổ biến nhất |

### Tình huống cụ thể → Lựa chọn:

**Nếu AI nói sai thông tin y khoa có thể gây nguy hiểm** (sai liều, sai chống chỉ định):
→ **Hard kill** ngay lập tức. Code:
```bash
# Set env var, redeploy takes < 2 min on Vercel
NEXT_PUBLIC_AI_DISABLED=true
```

**Nếu AI nói sai factual nhưng không nguy hiểm** (sai tên thuốc generic, sai năm nghiên cứu):
→ **Tighten prompt** + **Soft limit** song song.
```
# Thêm vào system prompt ngay lập tức:
"QUAN TRỌNG: Chỉ trả lời dựa trên nội dung trong file PDF đã được upload.
Nếu không tìm thấy thông tin trong file, trả lời: 'Tôi không tìm thấy thông tin này trong tài liệu của bạn.'
Tuyệt đối không suy luận hoặc thêm thông tin từ nguồn bên ngoài."
```

**Quyết định mặc định cho Synapsy (sandboxed chatbot model):**
→ **Tighten prompt** vì đây là lỗi kiến trúc (AI dùng knowledge ngoài file) — không phải bug UI.

---

## Bước 3 — Customer Comm (5 phút)

**DM trực tiếp cho người đã tweet. Gửi trong vòng 15 phút kể từ khi verify.**

> Subject: [Không cần subject — DM thẳng]

---

Chào [tên],

Mình là [Tên], founder của Synapsy. Mình vừa thấy tweet của bạn và đọc ngay.

Bạn nói đúng — AI đã đưa ra thông tin "[quote lại đúng nội dung sai]" và điều đó không chính xác. Mình xin lỗi. Đây là lỗi của sản phẩm, không phải lỗi của bạn.

Mình đã [tắt tính năng X / cập nhật prompt / deploy fix] lúc [giờ] sáng nay. Mọi output mới từ bây giờ đã được kiểm soát chặt hơn.

Cụ thể với bạn:
- Tài khoản của bạn được **gia hạn miễn phí 3 tháng** — không cần làm gì thêm.
- Nếu bạn muốn, mình có thể review trực tiếp bộ thẻ bạn đang dùng để đảm bảo không có thông tin sai nào khác.

Một lần nữa — cảm ơn bạn đã chỉ ra. Startup giai đoạn này cần những phản hồi thẳng thắn như vậy hơn bất cứ thứ gì khác.

[Tên]
Founder, Synapsy

---

**Lưu ý khi gửi:**
- Không dùng "chúng tôi" — dùng "mình"
- Không dùng corporate template — viết như người thật
- Quote lại đúng nội dung sai trong DM — thể hiện bạn đã đọc kỹ
- Compensation cụ thể (3 tháng free) — không phải "chúng tôi sẽ xem xét"

---

## Bước 4 — Public Response (2 phút)

**1 tweet từ account founder (không phải account @Synapsy). Dưới 280 ký tự.**

---

> AI của Synapsy vừa đưa ra thông tin sai về [X]. Đã fix lúc [giờ] sáng nay.
>
> Cảm ơn @[username] đã chỉ ra — đây đúng là lý do sản phẩm còn cần thời gian.
>
> Với ai đang dùng: nếu thấy output nào đáng ngờ, DM mình trực tiếp.

---

**Nguyên tắc của tweet này:**
- Thừa nhận cụ thể (không "một số vấn đề kỹ thuật")
- Mention trực tiếp người report
- Call-to-action rõ ràng (DM mình)
- Không xin lỗi quá mức — ngắn, thật, có hành động

---

## Checklist Sau Incident

Làm trong vòng 24h sau khi tình huống ổn:

- [ ] Viết post-mortem ngắn (5 gạch đầu dòng): root cause, timeline, fix, learning, prevention
- [ ] Lưu conversation log của incident vào `ai_logs/incidents/YYYY-MM-DD.md`
- [ ] Update system prompt chính thức trên production
- [ ] Xem xét thêm output validation layer (regex hoặc classifier check trước khi render)
- [ ] Tặng compensation cho user bị ảnh hưởng (đã làm ở Bước 3 — xác nhận lại)

---

*Synapsy · Internal · Không chia sẻ ngoài team*
