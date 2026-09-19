# منصة فهيم — صفحة اختبار التقييم (Faheem Evaluation)

صفحة ثابتة (static) واحدة تحتوي اختبار تقييم قصير يحدّد نسبة الخصم على باقات
الساعات التعليمية، مع الباقات وجدول الأسعار والاعتمادات وبيانات التواصل.

A single self-contained static page: an Arabic (RTL) placement quiz whose score
sets the student's discount tier, plus pricing packages, credentials and contact
sections. No build step, no dependencies — all CSS, JS and images are inlined.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site (originally `fahim.html`) — HTML, CSS, JS and base64 images inline. |
| `render.yaml` | Render Blueprint describing the static site. |

## Local preview

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

Opening `index.html` directly in a browser also works.

## Deploy on Render (static site)

### Option A — Blueprint (uses `render.yaml`)

1. Render Dashboard → **New** → **Blueprint**.
2. Connect this repo (`ayhamhammd/FaheemEvaluation`) and pick branch `main`.
3. Render reads `render.yaml` and creates the static site. Click **Apply**.

### Option B — Manual static site

1. Render Dashboard → **New** → **Static Site**.
2. Connect this repo, branch `main`.
3. Settings:
   - **Build Command:** *(leave empty)*
   - **Publish Directory:** `.`
4. **Create Static Site**.

Every push to `main` redeploys automatically. The free static plan is enough —
there is no server-side code.

## Configuration

Values live at the top of the `<script>` block in `index.html`:

- `WHATSAPP` — WhatsApp number that receives order summaries (currently `966500127846`).
- `EXAM_SECONDS` — quiz time limit (currently 10 minutes).
- `PACKAGES` — package names, hours and base prices in SAR.
- `TIERS` — discount percentages shown in the full price table.
- `BANK` — the question bank, grouped by track (`p1`, `p2`, `m`, `h`, `f`, `q`, `t`).

Discount mapping: 10 correct → 35%, 9 → 30%, 8 → 25%, 7 → 20%, 6 → 15%,
5 → 10%, below that → 5%.

## Notes

- Lead capture (`saveLead`) only writes when a `claude.use("db")` runtime is
  present; on Render it is a no-op and the page works normally.
- Google Fonts are loaded from the CDN; everything else is inlined.
