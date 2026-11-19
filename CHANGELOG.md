# Changelog

All changes relative to the **original fonts** (B fonts and IR fonts as originally distributed, and the "2" fonts).
The two entries below are cumulative: together they list every change made to the originals.

Families in this repository:

| Folder | Fonts | Version after this update |
|---|---|---|
| `B fonts/` | 120 | 2.02 – 2.022 |
| `IRfonts/` | 60 | 1.002 – 1.004 |
| `2 fonts/` | 45 | 1.002 |

No glyph shapes were redrawn in either update. PostScript names are unchanged, so existing Word, Pages and Keynote documents still find the fonts.

---

## 2026-11-19 — Second update: Windows/Mac parity, kashida justification, kerning

### Rendering in Word for Windows and Word for Mac is now identical
Every font now has the same values for everything that Word reads differently on Windows and on macOS: the line metrics (hhea = OS/2 typo = OS/2 win, no line gap), Mac and Windows names, Unicode character maps, and bold/italic flags. The only remaining difference is TrueType hinting. Windows applies it on screen at small sizes; it doesn't affect layout, line breaks, printing or PDF.

- **Line spacing (54 IR fonts and B Homa).** Word for Mac adds the font's line gap between lines and Word for Windows doesn't. The gap was 1500 units in 30 IR fonts (for example IRAban: 2.66 em per line on Mac against 1.93 em on Windows), 300 in 24 IR fonts, and 200 in B Homa. It's now 0, so Mac lines are spaced exactly as on Windows. Windows spacing is unchanged.

### Justify Low / Medium / High (kashida justification)
- Word stretches justified Arabic-script text by filling each widened join with copies of the font's kashida glyph, and the last copy overshoots into the neighboring letter. The kashidas in these fonts were 425–1254 units wide, so the overshoot flattened the notches under ن/ی teeth, stuck square blocks out past the base of ل and the curve of ک, and drew stray lines in Outline/Shadow/decorative styles.
- **219 fonts** now have an extra narrow kashida glyph (`uni0640.kashida`, typically 40–110 units) mapped to U+0640, which Word uses for justification. It is the font's own tatweel, shortened so that its end shapes are kept exactly (or, in 6 detailed styles, cut from its middle). This makes Solid, Outline, Shadow, Border and 3D styles all tile seamlessly. Its width is measured per font so that any overshoot stays inside the flat part of the joining stroke.
- **Typed tatweel is unchanged.** A required `ccmp` rule (under both the Arabic and the default script) maps it back to the original glyph, so «بســـم», decorative ـــــ and tatweel + haraka look and measure exactly as before.
- Unchanged, because their tatweel is an ornament that can't be repeated or is already narrow: B Badkonak, B Kidnap, B Masjed, B Moj, B Sooreh, B Tanab.

### Kerning (IR fonts)
- **Repaired corrupted kerning** in IRLotus, IRLotus-Italic, IRBadr-Bold and IRMaryam. Every kerning value had been stored one record off, with the real amount in the vertical-shift field, which pushed letters after ا د ذ ر ز ژ و up or down and tore them out of their words. For example, «زندان» rendered as «زنمدان». Also fixed: one broken pair in IRTerafik, IRTerafik-Bold and IRTerafik-Italic («تامین» → «تا مین»), and 4 pairs with a stray vertical shift in IRCompset-Bold. These fonts now render exactly like the originals.
- **Restored kerning in IRBadr and IRBadr-Italic** (7,826 pairs each), which had been lost in the first update. They now render exactly like the originals.
- An automated test of 3,872 letter combinations per font, run with kerning on and off, finds no letter detached from its word in any of the 225 fonts.

### Characters and shaping
- **ZWNJ (نیم‌فاصله), ZWJ, LRM, RLM** are now empty and zero-width in all fonts. In the B and "2" fonts they had a visible width (23–45 units), and LRM/RLM drew a mark. In the IR fonts they had zero width but still drew a visible stroke.
- **No-break space (U+00A0)** added to all fonts that lacked it (all except IRLotus). Option+Space on a Mac types it, and it previously fell back to another font.
- **Lam-Alef (لا لأ لإ لآ)** moved from the optional `liga` feature to the required `rlig` feature in the B and "2" fonts, so it still joins when ligatures are turned off. The Allah and shadda/sukun ligatures stay in `liga`, in their original order.

### Names and styles
- **IRLotus Bold:** family name `IRlotus` → `IRLotus`. Windows ignores letter case in family names and already grouped it with IRLotus, but macOS treated it as a separate family, so Bold in IRLotus used a computer-generated bold on Mac. The PostScript name `IRlotus-Bold` is unchanged.
- **B / 2 Kaj, Sahar, Zaman:** these were already Italic for Windows in the originals and were made Italic on Mac in the first update. This update removes the contradictory "Regular" typographic subfamily name (nameID 17) and sets the full names to "… Italic".
- **B Mashhad Bold Italic:** removed nameID 17 "BoldItalic"; full name is now "B Mashhad Bold Italic".
- **B / 2 Niki Border:** removed trailing spaces from the full name. 2 Niki Border's blank version string is filled in.
- **Italic angles** (38 italic fonts) set from each font's measured slant. Most of these italics lean left (Arabic back-slant) but declared a right lean (B, "2") or 0° (IR). Only the angle of the text cursor is affected.
- **Files renamed** to match the fonts inside: `B Elm Bold.ttf` → `B Elm Border.ttf`, `B Niki Bold.ttf` → `B Niki Border.ttf`, `IRlotus-Bold.ttf` → `IRLotus-Bold.ttf`.

### Metadata
- OS/2 Unicode and code-page ranges recomputed from the actual character maps. Three IR italics had claimed 14 code pages they don't cover.
- OS/2 x-height and cap-height set from the glyphs in 11 IR fonts (they were −1).
- Empty GPOS tables removed from the B and "2" fonts.
- B fonts: `head.fontRevision` (1.0) now agrees with the version string. All version numbers were raised so macOS replaces cached copies.

---

## Initial release — First update: Mac compatibility

- **Line spacing on Mac:** in the original B, "2", IRNazanin and IRCompset fonts, the line metrics macOS uses (hhea) didn't match the ones Windows uses (usWin). Mac got as little as 1.00 em per line, with a shifted baseline. They now match.
- **Rebuilt with FontLab 8:** standard glyph names (for example `u0627`/`gidNNN` → `uni0627`), which improves copy/paste and search in PDFs. OS/2 table upgraded to version 4, with x-height, cap height, default/break characters and average width filled in (it was 0 in the B fonts). A Unicode (platform 0) cmap subtable was added, and the Mac Roman subtable rebuilt.
- **"2" fonts:** the لا and الله ligatures now form; they didn't in the originals.
- **Tables:** GDEF (mark classes), GPOS (empty) and gasp added to the B and "2" fonts, and a DSIG placeholder added to all fonts. Removed outdated Windows-only tables: PCLT and hdmx (B, "2"); LTSH, VDMX, hdmx and the legacy `kern` table (IR); embedded bitmaps EBDT/EBLC (30 IR fonts).
- **Hinting** regenerated (fpgm/prep/cvt). This affects Windows screens only: B and "2" fonts render practically pixel-identical to the originals, and IR fonts differ only by small pixel shifts.
- **Names:**
  - Mac and Windows full names made identical (22 IR fonts).
  - Redundant typographic names (16/17) removed from the IR fonts.
  - Unicode-platform name records removed from the B and "2" fonts.
  - "2" fonts got PostScript names without spaces (`2 Aseman` → `2Aseman`) and version strings.
  - B Mashhad Bold Italic's style name fixed ("BoldItalic" → "Bold Italic").
  - B / 2 Niki Border got a style name (it was blank).
- **B / 2 Kaj, Sahar, Zaman:** style changed from Regular to Italic on Mac, which now matches the italic flag Windows already used.
- **Added 2 Nazanin.**
- *Side effects corrected in the second update:* the rebuild added large line gaps to 54 IR fonts, stored the kerning of 8 IR fonts one record off, and dropped the kerning of IRBadr and IRBadr-Italic.

---

## Known limitations (unchanged from the originals)
- B and "2" fonts: Latin digits 0–9 show Persian digit shapes, and ی/ي and ک/ك share glyphs. This is the original design, and existing documents rely on it.
- B and "2" fonts have no glyphs for hamza above (ٔ), superscript alef (ٰ), the ellipsis or the euro sign; these fall back to another font.
- "2" fonts keep their original family names with two spaces (for example "2  Nazanin"), because documents store these names.
- Some glyphs in 27 IR fonts and 32 B fonts are taller than the line metrics. Windows clips them on screen; macOS draws them overlapping the line above.
