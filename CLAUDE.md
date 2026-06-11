# Scent.M — Project Rules (persistent)

## Design language source of truth
**ALWAYS follow the design language of: `index.html` / `about.html` / `workshops.html` / `journal.html`.**
Font sizes, typography, layout, art direction, decoration — all defer to these four pages. Never invent new scales or decoration outside what these pages establish.

## Typography scale (from index/about/workshop/journal — do NOT default to tiny)
- Fullscreen menu links: `text-[2.75rem] sm:text-5xl md:text-7xl font-serif italic`
- Section eyebrow: `text-xs sm:text-sm md:text-base tracking-[0.2em] uppercase text-brand-terracotta` (12→16px — NOT 11px)
- Section H2: `fluid-h2` (40→60px)
- Body / readable nav / clickable text: `text-base md:text-lg` (16→18px) or larger
- `text-[11px]` is ONLY for true micro-labels (captions/metadata), never for navigation or primary readable text
- Bilingual heading pattern: 中文 + `<br>` + `<span class="italic text-brand-terracotta font-light">English</span>`

## Colors
light #FDFBF9 · beige #F3EAE1 · terracotta #A64B2A · brown #3B261D

## Fonts
serif: Baskervville + Cactus Classical Serif · sans: Montserrat + Noto Sans TC · script: Mea Culpa (sparingly)

## Absolute obedience
- Execute the user's instructions EXACTLY as stated. No substitutions, no shortcuts, no "better" alternatives unless asked.
- When told to check line-by-line, dump EVERY relevant line (not headings only, not sampling, not memory). Verify before acting.

## Hard rules
- NEVER copy homepage layout onto other pages — innovate WITHIN the design system.
- NEVER add redundant "contact" CTAs — floating WhatsApp is the primary channel.
- NO two-colour/morph flip effects outside homepage hero + nav menu/logo.
- CTA wording unified: 「聯絡我們」(inquiry), 「立即預約」(workshop booking), Explore Hibi Lab (external).
- Maintain consistent `max-w-screen-2xl` gutter per page; never narrow→wide→narrow.
- Confirm direction before large rebuilds; revert cleanly when asked.
