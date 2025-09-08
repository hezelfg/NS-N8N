# 🤖 AI_NOTES

## Model  
- **OpenAI GPT (ChatCompletion)** used via n8n “OpenAI Chat Model” node.  
- Config:  
  - `temperature`: 0.9  
  - `top_p`: 0.9  
  - `max_tokens`: 220  
- Example system prompt ensures tone: *“warm, professional, upbeat”*, 2–3 sentences grouped into paragraphs, natural Slack style.  
- Config (temperature, top_p, max_tokens) → These are parameters that control the “personality” of the output: temperature: 0.9 → more creativity/variety to the messages - top_p: 0.9 → diversity filter (often used in conjunction with temperature) - max_tokens: 220 → Limit on the number of words each message can generate

## Prompts / Tools  
- **Prepare AI Prompt (Code Node)** builds context:  
  - Inputs: name, eventType, title, yearsAtCompany, cake/pizza prefs, sweet/savory, allergies.  
  - Rules:  
    - Exclude cake/pizza from anniversaries.  
    - Translate food names to English.  
    - Use 2–4 emojis, varied and relevant.  
    - No em-dashes, avoid robotic tone.  
- **Pollinations Image API** generates geometric corporate cards with correct labels.  
- **Slack API** posts messages with both text + image blocks.  

## Eval Set Results  
- **Test Cases**:  
  - Birthday on Tue → ✅ public post, ✅ HR summary.  
  - Work anniversary on Sat → ❌ no weekend post, ✅ retro on Monday.  
  - Birthday + Anniversary (same day) on Mon → ✅ combined post, ✅ HR summary.  
  - Anniversary of 0 years → ❌ excluded.  
- **Edge Cases**:  
  - Multiple employees same day → ✅ grouped correctly in HR summary.  
  - Employees missing Preferred Name → fallback to FirstName LastName.  

## Cost / Latency Snapshot  
- **Cost**:  
  - ~220 tokens / person, 3–4 prompts per day average.  
  - At $0.002 / 1K tokens → negligible (< $0.05/month for ~100 employees).  
- **Latency**:  
  - Calendar filter + code: <100ms.  
  - OpenAI Chat call: ~1–2s.  
  - Pollinations image fetch: ~2–5s.  
  - Slack post: ~0.5s.  
  - **Total**: 3–7s per celebrant, parallelized by n8n.  
