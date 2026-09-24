# منصة فهيم — حاسبة الساعات التعليمية (Faheem)

صفحة ثابتة (static) واحدة: شاشة ترحيب تشغّل النشيد الوطني السعودي عند الدخول، وحاسبة
ساعات يختار فيها ولي الأمر المرحلة الدراسية ويعبّئ احتياج الطالب لكل مادة، واختبار
تحديد مستوى، وآراء أولياء الأمور (صور وفيديو)، والاعتمادات وبيانات التواصل.

A single static page in Arabic (RTL): a welcome screen that plays the Saudi national
anthem on entry, an hours calculator per school stage and subject, a free placement
quiz, parent testimonials (screenshots and videos), credentials and contact sections.

## Files

| File | Purpose |
| --- | --- |
| `index.html` | The whole site (originally `fahim.html`) — HTML, CSS, JS; logo, certificate and partners images inline as base64. |
| `assets/` | Testimonial screenshots (`feedback-*.jpg`, phone numbers cropped out), the award photo, and the three videos with their poster frames. |
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
- `HOUR_PRICE` — the calculator's price per hour: **83 SAR**. It is the average price per hour
  across all 13 former packages (4–200 hours) under every former discount tier (5–35%),
  which comes to 83.08. The packages themselves were removed from the site.
- `WEEKS_PER_MONTH` — 4. Hours per subject = minutes ÷ 60 × sessions per week × 4 × months.
- `STAGES` — each school stage and its subjects, shown in the calculator.
- `BANK` — the quiz question bank, grouped by track (`p1`, `p2`, `m`, `h`, `f`, `q`, `t`).
  The quiz now reports a level only; it no longer grants a discount.

The anthem is the US Navy Band recording on Wikimedia Commons (public domain), linked
directly rather than committed. Browsers block audio until the visitor interacts, so it
starts when they press «ادخل إلى المنصة»; «الدخول بدون صوت» skips it, and it is shown once
per browser tab session.

## Notes

- Lead capture (`saveLead`) only writes when a `claude.use("db")` runtime is
  present; on Render it is a no-op and the page works normally.
- Google Fonts are loaded from the CDN; everything else is inlined.
