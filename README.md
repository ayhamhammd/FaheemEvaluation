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
| `assets/` | Testimonial screenshots (`feedback-*.jpg`, phone numbers cropped out), the award photo, the founder's story video (`faheem-story`, its own section after the steps), and four testimonial videos with their poster frames. |
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
- `HOUR_BANDS` — the former packages' hours and prices, used only to derive a price **per
  hour** for each band (615 ÷ 4 = 153.75 … 17661 ÷ 200 = 88.31). Package totals are never
  shown. The calculator adds up the hours of every subject, rounds that total to the
  **nearest** band (a tie goes to the larger band, which is cheaper), and prices every hour
  at that band's rate. Below 4 hours uses the 4-hour rate, above 200 the 200-hour rate.
- `TIERS` — the quiz discount on the band's hour price: 10 correct → 35%, 9 → 30%, 8 → 25%,
  7 → 20%, 6 → 15%, 5 → 10%, below that → 5%. The calculator shows a table of every band's
  hour price before and after each tier, highlighting the visitor's band and discount.
- `EXAM_SECONDS` — 5 minutes. The quiz can be taken **once**: progress and the result are
  kept in `localStorage` (`faheem-exam`), so a reload resumes the exam against the same
  deadline, or shows the saved result. Clearing site data is the only way round it.
- **Prices stay hidden until the quiz is done.** Before that the calculator section shows a
  lock card pointing to the quiz, the hero card shows only the discount range, and the
  assistant and FAQ explain the rule without quoting a figure. `renderPrice()` flips all of
  it on `state.done`, and the saved result keeps it open across reloads.
- `WEEKS_PER_MONTH` — 4. Hours per subject = minutes ÷ 60 × sessions per week × 4 × months.
- `STAGES` — each school stage and its subjects, shown in the calculator.
- `BANK` — the quiz question bank, grouped by track (`p1`, `p2`, `m`, `h`, `f`, `q`, `t`).

### مساعد فهيم (the assistant)

A floating chat that answers parents from `KB` in the page script — no server and no AI
model, so it only knows what is written there. Each entry has `keys` (Arabic keywords,
matched after normalising hamza, taa marbuta, alef maqsura and diacritics), an answer, and
follow-up suggestions. A question that matches nothing is handed to the team on WhatsApp
with the question pre-filled. To teach it something new, add an entry to `KB` and a label
to `CHIP_LABELS` if it should appear as a suggestion.

The anthem is the US Navy Band recording on Wikimedia Commons (public domain), linked
directly rather than committed. Browsers block audio until the visitor interacts, so it
starts when they press «ادخل إلى المنصة»; «الدخول بدون صوت» skips it, and it is shown once
per browser tab session.

## Notes

- Lead capture (`saveLead`) only writes when a `claude.use("db")` runtime is
  present; on Render it is a no-op and the page works normally.
- Google Fonts are loaded from the CDN; everything else is inlined.
