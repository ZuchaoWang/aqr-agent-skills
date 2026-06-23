# PowerPoint reports

Conventions for PowerPoint reports this project produces. Include this doc only if the project ships `.pptx` outputs (e.g., client deliverables, internal reviews). Otherwise delete it.

## 1. Template and brand

- Master template: {{path to `.pptx` or link}}.
- Color palette: {{primary, secondary, accent — hex or named}}.
- Fonts: {{heading font, body font, fallback}}.
- Logo placement: {{corner, size, when to omit}}.

## 2. Slide structure

- One idea per slide. If a second idea appears, split the slide.
- Title slide and section dividers follow the master template; do not redesign per deck.
- Slide titles are sentence-case, ≤ {{N}} characters.
- Body text bullets: ≤ 6 per slide, ≤ 2 lines each. Dense slides get an appendix sibling.

## 3. Charts and visuals

- Charts use the project palette, not PowerPoint defaults.
- Axis labels, units, and source line are required on every chart.
- Screenshots are dated and cropped to the relevant region; no full-window dumps.
- Diagrams are editable shapes, not flattened images, when the deck is a working artifact.

## 4. Notes and narration

- Speaker notes on every content slide: the talking point, not the slide text repeated.
- Appendix slides for backup data; reference them from the speaker notes of the slide they support.

## 5. File conventions

- Naming: `{{YYYY-MM-DD}}-{{topic}}.pptx`. Date prefix sorts naturally.
- Storage: {{path under `docs/` or external — pick one and record}}.
- Source files (`.key`, `.fig`, design assets) live next to the deck, not in a separate dump.
- Export PDFs for distribution; do not email `.pptx` unless the recipient will edit.

## 6. Review checklist

- [ ] Spell check run.
- [ ] Every chart has source and date.
- [ ] Slide titles consistent in style.
- [ ] Appendix slides referenced from the main deck.
