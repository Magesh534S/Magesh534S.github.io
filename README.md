# Portfolio Website

A single-file, dark-themed developer portfolio with a "design-tool selection" scroll animation.

## Transparency First

- **The design is not an original idea of mine.** I saw this style of portfolio online, liked it, and wanted to recreate it.
- **The build is mine.** I did not copy the code. I recreated the site by writing a detailed prompt and directing an AI assistant to generate it.
- **This whole repository was created with AI.** The website code, this README and the prompt itself were all produced with AI assistance, under my direction.

I would rather say that openly than hide it. Knowing how to get good results from AI is a real skill, and this repo is a demonstration of it.

## Prompt Engineering: How This Prompt Works

A vague request like "make me a portfolio website" gives a generic result. A good prompt behaves more like a design brief and a technical spec combined. The prompt below uses these ideas:

1. **Role.** It tells the AI who to be (a senior front-end engineer and visual designer), which sets the quality bar.
2. **Deliverable and constraints.** One self-contained file, no frameworks, output only the code. Hard limits stop the AI from wandering.
3. **Design tokens.** Exact fonts, colors and component rules are given up front, so the look is specified, not guessed.
4. **Structure.** Every section is described in order, with its content slots, so the layout is predictable.
5. **Behavior spec.** The signature scroll animation is written as step-by-step rules (triggers, timing, reset logic, edge cases) instead of "make it animated".
6. **Quality requirements.** Accessibility, responsiveness, reduced-motion support and error-free JavaScript are stated explicitly.
7. **Guardrails against invention.** The content rules tell the AI to use only the content provided and to leave out anything missing, so it does not make things up.
8. **Reusable placeholders.** Everything personal is a `[BRACKETED]` field, so the same prompt works for anyone.

The prompt is below. Replace every `[BRACKETED]` item with your own content before using it.
## The Prompt

Use the prompt below with an AI assistant to generate the site. Replace every `[BRACKETED]` item with your own content first.

```text
ROLE
You are a senior front-end engineer and visual designer. Build a single-file personal portfolio website.

DELIVERABLE
One self-contained index.html (inline CSS and JS, no frameworks, no build step, no external images). The only external resource allowed is Google Fonts. Output the complete file in one code block, with no commentary.

DESIGN DIRECTION
Bold editorial style on a dark background, warm coral plus violet accents. Big condensed uppercase display type mixed with italic serif accents.
- Fonts: Barlow Condensed (700/800) for display, Instrument Serif italic for accents, Manrope (400/500/700/800) for body. Provide system fallbacks.
- CSS variables on :root: --bg #0b0810, --bg2 #14101c, --text #f3eef7, --muted #9a93a8, --line #2a2335, --c1 #ff7a59 (coral), --c2 #b58cff (violet), --card #15111d.
- Background: body color plus two soft radial gradients (coral top-left, violet right).
- Light theme via :root[data-theme="light"] (bg #faf6f1, text #1a1420, card #fff). Dark is the default. A "Theme" toggle button in the nav, persisted in localStorage inside try/catch.
- Pill-shaped buttons with a coral gradient (primary) and an outlined ghost variant. Cards have a 20px radius, 1px border, and lift on hover with a coral border.

PAGE STRUCTURE
1. Sticky blurred nav: [FIRST NAME]. wordmark; links Work, About, Journey, Contact; theme toggle. On mobile (<760px) hide all but the first two links.
2. Hero (two columns, stacks on mobile): italic serif "Hello, I'm", a huge uppercase [FIRST NAME]. (font-size clamp(64px,13vw,150px), the period in a coral-to-violet gradient text), a small spaced roles line "[ROLE 1] · [ROLE 2] · [ROLE 3]", a short lead paragraph ([ONE-SENTENCE INTRO + LOCATION]), buttons "See my work →" and "Say hello". Right column: a 4:5 portrait card with a gradient fill, giant faint [INITIALS] monogram, a stats stack top-right ([STAT 1 VALUE/LABEL], [STAT 2 VALUE/LABEL], [STAT 3 VALUE/LABEL]) and the caption "[SHORT TAGLINE IN CAPS]".
3. Marquee band: full-width gradient strip rotated -1deg, infinite horizontal scroll of [6-8 SKILL OR TECH KEYWORDS], each followed by " +" (list duplicated for a seamless loop).
4. 01 / Selected work: heading "[SECTION HEADLINE]." Filter tabs All / [CATEGORY A] / [CATEGORY B] that show or hide cards. [3-6] cards with gradient thumbnails (rotate coral, violet, teal), each with: number + context line, title, one-sentence description, category. Below the cards, a "tools behind the work" chip row: [LIST OF TOOLS].
5. 02 / About: heading "[ABOUT HEADLINE]." Three alternating rows, each with an italic serif statement plus a short paragraph on the left, and on the right a stacked typographic lockup (outlined violet word, italic coral serif line, solid white line). Rows: [THEME 1], [THEME 2], [THEME 3], each with its own statement, paragraph and three-line lockup.
6. 03 / Journey: heading "[JOURNEY HEADLINE]." Vertical timeline with coral dots, one entry per item (role/title, organization, dates, optional one-line description): [TIMELINE ENTRIES, most recent first]. Then certification or award cards ([NAME / FULL TITLE / DATE] each) and an optional "[VERIFY LINK TEXT] ↗" link to [URL].
7. 04 / Contact: centered heading "[CONTACT HEADLINE]", a one-line availability statement ([AVAILABILITY]), and pill links: Email ([EMAIL]), [PLATFORM 1] ([URL]), [PLATFORM 2] ([URL]). Links to other sites open in a new tab with rel="noopener".
8. Footer: "© [YEAR] [FULL NAME] · [CITY, COUNTRY]".

SIGNATURE ANIMATION: "DESIGN-TOOL SELECTION BUILD"
Every major element (hero text, portrait, section eyebrows and headings, tabs, cards, tool chips, side rows, timeline items, certs, contact items) starts hidden (opacity 0, scale .94) and is revealed by an animation that imitates a design tool:
- When an element enters the viewport (IntersectionObserver, threshold .15), a colored cursor with a name tag (short labels such as [LABEL 1]..[LABEL 5]; cycling colors blue, red, yellow, pink, cyan) flies in from a random offset, above the element when scrolling down and below it when scrolling up.
- The cursor then drags out a selection rectangle (1px colored border, 9% tinted fill, four white corner handles, a live "W × H" size label under it) from the element's top-left to its bottom-right, using ease-out-cubic timing.
- On completion the element pops in (opacity and scale spring), the rectangle fades out, and the cursor drifts away and fades.
- Reveals are staggered by about 170ms so simultaneous elements do not fire together.
- When an element leaves the viewport it is reset (any leftover overlay nodes removed, hidden state restored) so the build replays when scrolling up or down. Guard every async step with a per-element token so stale timeouts and animation frames never act after a reset.
- Draw all overlay rectangles and cursors in one absolutely positioned layer appended to the body.
- Add 3 "ambient" cursors (labels [A], [B], [C]) fixed to the viewport that continuously glide toward random points on already-revealed elements, easing with a .07 lerp and re-picking targets every 1.4 to 3.2 seconds.
- Respect prefers-reduced-motion: no marquee, no cursors, no hidden initial state; show everything immediately.

TECHNICAL AND QUALITY REQUIREMENTS
- Semantic HTML (nav, main, section, footer), real headings in order, good contrast in both themes, visible focus states.
- Responsive from 360px to 1440px with no horizontal scroll (overflow-x hidden on body). Use viewport-fit=cover and safe-area insets on the root and sticky nav.
- Smooth anchor scrolling with scroll-padding-top so the sticky nav does not cover headings.
- Vanilla JS in a single IIFE, no globals, no console errors, no dependencies.
- Typography detail: use a true apostrophe (’) and curly quotes in visible text.
- Keep it under roughly 20 KB.

CONTENT RULES
Use only the content I provide in the brackets above. Do not invent employers, metrics, testimonials, awards or project links. Do not add placeholder lorem ipsum. If any bracketed field is missing, omit that element rather than making something up.
```

## About Me and How I Use AI

I'm Magesh, a Dynamics 365 and Power Platform developer based in Chennai.

I use AI wisely and keep myself updated with it. For me it is a tool, not a shortcut: I decide what to build, give it clear constraints, review what it produces, and make sure I understand the result well enough to explain it. AI also helps me learn new things faster, from front-end techniques to areas outside my day-to-day work, and this repo is one example of that.

- GitHub: [Magesh534S](https://github.com/Magesh534S)
- LinkedIn: [magesh0503](https://www.linkedin.com/in/magesh0503)
