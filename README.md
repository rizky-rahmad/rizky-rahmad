<img src="assets/header.svg?v=2" alt="Rahmad Rizki — Full Stack Web Developer & AI Implementation Specialist" width="100%">

I build web applications end to end — Node.js and Express on the back, React and
Next.js on the front — and I put language models into them where they earn their
place. Full Stack Developer at PT Unicorn, based in Ubud, Bali.

Most of what I enjoy is the part after it works: finding out *why* it is slow, and
being willing to be wrong about the answer.

<img src="assets/divider.svg" alt="" width="100%">

### Selected work

<a href="https://rizky-portfolio.pages.dev"><img src="assets/card-portfolio.svg" alt="Portfolio + AI assistant — answers a visitor's questions from my resume, in English or Indonesian, in about two seconds. Next.js, Gemini, Cloudflare." width="49%"></a>
<a href="https://apply-mate-ai-nine.vercel.app"><img src="assets/card-applymate.svg" alt="ApplyMate AI — turns a resume and a job posting into a match analysis and a cover letter, grounded in real work. Next.js, TypeScript, Gemini." width="49%"></a>
<a href="https://sikemasbpsdm.web.id"><img src="assets/card-sikemas.svg" alt="SIKEMAS — complaint management for BPSDM Aceh, with multi-role RBAC, audit logs and ticket dispatch. React, Node.js, PostgreSQL." width="49%"></a>
<a href="https://barakah-qurban.vercel.app"><img src="assets/card-qurban.svg" alt="Barakah Qurban — landing page for a syariah-certified Qurban cattle provider, built to load fast on mobile. Next.js, React 19, Tailwind." width="49%"></a>

Source: [portfolio-v2](https://github.com/rizky-rahmad/portfolio-v2) · [ApplyMateAi](https://github.com/rizky-rahmad/ApplyMateAi) · [barakah-qurban](https://github.com/rizky-rahmad/barakah-qurban)

<img src="assets/divider.svg" alt="" width="100%">

### Shipping now — PT Unicorn, production systems

What I run in production every day: restaurants take bookings through these
systems, and HR runs attendance and hiring on them.

**Channelflow — omnichannel inbox + AI agent + bookings engine.**
WhatsApp, Instagram, Email and TikTok in one inbox. A grounded multilingual AI
answers from the knowledge base and live menu, takes bookings, and hands off
to humans on allergy, VIP and low-confidence cases.

**Channelflow Mobile — the staff app, on Android.** The dashboard's bookings
in a pocket: 14-day list, day timeline, month view, analytics. Same backend,
same data, verified screen by screen on an emulator.

<img src="assets/card-channelflow.svg?v=2" alt="Channelflow — one inbox for WA, IG, Email and TikTok, with a grounded AI agent that answers and books. Next.js, Mastra, Hono.js." width="49%"> <img src="assets/card-channelflow-mobile.svg?v=2" alt="Channelflow Mobile — staff bookings on Android at parity with the dashboard. Expo, React Native, TypeScript." width="49%">

**CMS — multi-tenant page builder.** One deploy serves many brands: free-form
drag-and-drop builder, in-place editing, autosave drafts, design import,
per-site themes and domains. All media served locally.

**PeopleOS — HR / workforce operating system.** The whole employee lifecycle:
GPS time-clock with geofencing, outlet scheduling, hiring ATS with video
answers and AI scoring, training, announcements. Web app plus a React Native
companion.

<img src="assets/card-cms.svg?v=2" alt="CMS plus page builder — multi-tenant CMS with a free-form drag-and-drop builder. Next.js, Prisma, dnd-kit." width="49%"> <img src="assets/card-peopleos.svg?v=2" alt="PeopleOS — HR operating system with GPS time-clock, scheduling, hiring ATS and training. React, Express, PostgreSQL." width="49%">

<details>
<summary><b>How the portfolio chatbot actually works</b> — and the 49s → 2s story</summary>

<br>

My résumé lives in a Google Doc, and the certificates in it are scans — images
only a vision model can read. The obvious implementation sends that document to
the model on every message. That is what it did at first, and it cost about
**40 seconds per reply** to produce a reading that came out identical every time.

The fix was not a faster model or a smaller payload. It was doing the reading at
a different moment:

<img src="assets/pipeline.svg" alt="Pipeline: the Google Doc is hashed hourly by GitHub Actions; only a changed document is transcribed once by Gemini, committed as an 8KB JSON file, and served to a visitor in about two seconds" width="100%">

The document is hashed first, so an unchanged résumé costs nothing at all. I
still update it the same way as before: by pasting into the Doc.

**What I got wrong on the way there:** I was sure the cost was the 2.2MB going
over the wire each message. Measured, that transfer was under 5% of the time. The
expensive part was the model re-reading eight pages of scanned certificates.

</details>

<details>
<summary><b>Making the same site score 98 on mobile</b></summary>

<br>

Mobile PageSpeed sat at 90 with LCP 3.5s while desktop was already perfect —
which is exactly why it went unnoticed for months.

The LCP element was the hero subtitle, and it was fading in:

```css
.animate-fade-up { animation: fade-up 0.7s ease-out both; }
```

`fade-up` starts at `opacity: 0`, and `both` holds it there through the delay.
Chrome does not count a transparent element as painted, so **the metric was
waiting on my own animation**, not on the network. Giving above-the-fold text a
transform-only animation, deferring the chat widget to `requestIdleCallback`, and
inlining the render-blocking CSS took LCP from 1536ms to 508ms locally.

<img src="assets/proof.svg" alt="Mobile PageSpeed performance 98 out of 100, and Largest Contentful Paint improved from 1536ms to 508ms. CLS 0, TBT 20ms, SEO 100." width="100%">

Best Practices stays at 96 because Cloudflare's own analytics beacon fails with
`ERR_BLOCKED_BY_CLIENT`. I left it there — real analytics is worth more than four
points.

</details>

<details>
<summary><b>What I work with</b></summary>

<br>

<img src="assets/skills.svg?v=2" alt="Stack: React, Next.js, TypeScript and Tailwind on the frontend; Node.js, Express, NestJS, REST, JWT and OAuth 2.0 on the backend; PostgreSQL, Redis and Supabase for data; Gemini, OpenAI, prompt design and document retrieval for AI; Cloudflare, Vercel, Azure Functions, GitHub Actions and Docker for infrastructure. Microsoft Certified Azure AI Fundamentals." width="100%">

</details>

<img src="assets/divider.svg" alt="" width="100%">

### How I work

<img src="assets/workflow.svg" alt="A terminal session: measuring which leg is slow before optimising, and finding that Largest Contentful Paint was waiting on a CSS fade" width="100%">

I use **Claude Code** in my daily loop — not to produce code I could not explain,
but to move faster through the mechanical parts and to have something to argue
with while debugging. It is most useful exactly where I am most likely to be
confidently wrong.

Both results on this page came out of that: three plausible explanations for the
chatbot's latency were wrong before measurement settled it, and the mobile LCP
turned out to be waiting on my own CSS rather than on the network. The tool did
not know the answer either — it made it cheap to test each hypothesis until one
survived.

<img src="assets/divider.svg" alt="" width="100%">

### Reach me

<a href="https://rizky-portfolio.pages.dev"><img src="assets/badge-portfolio.svg" alt="Portfolio" height="44"></a>
<a href="https://linkedin.com/in/rahmad-rizki-1728a6186"><img src="assets/badge-linkedin.svg" alt="LinkedIn" height="44"></a>
<a href="mailto:rizky.business7@gmail.com"><img src="assets/badge-email.svg" alt="Email" height="44"></a>
<a href="https://wa.me/6282365434655"><img src="assets/badge-whatsapp.svg" alt="WhatsApp" height="44"></a>

<sub>The assistant on my portfolio will answer questions about my background before you have to ask me directly.</sub>
