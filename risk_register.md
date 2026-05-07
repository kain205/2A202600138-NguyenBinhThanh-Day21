# Risk Register — Synapsy
**Cập nhật:** Day 18 | **Scoring:** Likelihood (1–5) × Impact (1–5 theo tháng runway mất)

---

## RISK-01 — Vendor Risk: OpenAI Rate Limit / Pricing / ToS

**If** OpenAI tăng giá API đột ngột, siết rate limit trong mùa thi cao điểm, hoặc cập nhật ToS loại trừ education use case  
**Then** Diagnostic Loop bị tê liệt hoặc chi phí vượt kiểm soát, không thể serve beta users  
**Leading to** mất **2 tháng runway** để rebuild pipeline trên provider thay thế

*Ref: Replika (2023) — vendor infrastructure + áp lực pháp lý Italy buộc tắt core use case trong 48h. Không có Plan B = không có thời gian phản ứng.*

| Likelihood | Impact (tháng) | Score |
|:---:|:---:|:---:|
| 3 | 4 | **12** |

**Mitigation:** Abstract AI layer sau interface — không hardcode OpenAI SDK vào business logic. Test Anthropic / Gemini làm drop-in provider mỗi 2 tuần.

---

## RISK-02 — Customer-Facing AI Risk: Chatbot Bịa Policy

**If** AI chatbot hallucinate thông tin lâm sàng (dosage, chẩn đoán) nằm ngoài file upload, sinh viên Y tin và dùng để ra quyết định học/lâm sàng  
**Then** Synapsy bị hold liable vì AI output — tòa coi chatbot statement là tuyên bố ràng buộc của công ty  
**Leading to** mất **3+ tháng runway** (legal fees + forced shutdown Y khoa vertical + mất toàn bộ beachhead)

*Ref: Air Canada (2024) — chatbot bịa chính sách hoàn tiền không tồn tại, tòa buộc Air Canada thực hiện. Precedent: AI output = binding company statement, bất kể disclaimer.*

| Likelihood | Impact (tháng) | Score |
|:---:|:---:|:---:|
| 2 | 5 | **10** |

**Mitigation:** System prompt hardlock "chỉ dùng nội dung file đã upload." Mọi output phải có source reference về trang/dòng PDF gốc. Per-session disclaimer hiển thị rõ.

---

## RISK-03 — Founder Bandwidth: Single Point of Failure

**If** Founder bị ốm, burnout, hoặc sự cố cá nhân 3+ ngày  
**Then** critical bug không fix được, investor demo bị miss, user interview bị cancel  
**Leading to** mất **1 tháng runway** từ missed PMF window và delay fundraising

| Likelihood | Impact (tháng) | Score |
|:---:|:---:|:---:|
| 4 | 3 | **12** |

**Mitigation:** `main` luôn shippable — mọi WIP trên branch. Cuối mỗi sprint cập nhật WORKLOG.md với current state + next 3 actions. Buffer 2 ngày trước mọi investor meeting.

---

## 2×2 Risk Matrix

```
Impact        LOW Likelihood (1–2)    HIGH Likelihood (3–5)
(tháng)      ┌──────────────────────┬────────────────────────┐
HIGH (3–5)   │      ESCALATE        │    ☠️  KILL ZONE       │
             │  RISK-02 (L:2, I:5)  │  RISK-01 (L:3, I:4)   │
             │                      │  RISK-03 (L:4, I:3) ★ │
             ├──────────────────────┼────────────────────────┤
LOW (1–2)    │       ACCEPT         │        WATCH           │
             │          —           │          —             │
             └──────────────────────┴────────────────────────┘
```

**KILL ZONE:** RISK-01 + RISK-03 (Score: 12 each)

**★ Priority Block 3: RISK-03 (Founder Bandwidth)**
Likelihood cao nhất (4/5). Xảy ra bất kỳ lúc nào. Mitigation không tốn tiền — chỉ cần discipline. Hiện tại zero protection.
