# Shape Neighbor 2.1 — Design QA

**Final result: passed**

## Scope and evidence

- Source visual truth: user-supplied `../cpsc1710-labs/lab-02/one-pixel.html`, rendered in its initial state. Source credits Xiuye Chen, CC BY 4.0.
- Implementation: `your-new-lab.html`, initial default samples, input 60 by 30, k = 1, optional guide off.
- Intent: adapt the reference's visual language and experiment affordances to the existing rectangle classifier. This is not a pixel-for-pixel recreation of the one-pixel lesson or its training workflow.
- Capture browser: installed Microsoft Edge, headless, with the existing temporary Playwright harness. The in-app browser was unavailable. The context-preflight Python script could not run because Python is not installed; a direct read check found no saved Product Design context.
- Desktop viewport: 1440 by 1100 CSS pixels, device scale factor 1. Source and implementation viewport captures use the same 1440 by 1100 pixel dimensions. The comparison sheet displays them at equal 50% scale; no unequal density adjustment was used.
- Local capture directory: `%TEMP%/shape-neighbor-v2-checks/`. These QA captures are temporary local evidence, not runtime assets or files uploaded to Vercel.
- Full-view comparison: `source-v3.png` and `v3-default.png`, combined in `v3-comparison.png`. The full implementation is also captured in `v3-full.png`.
- Focused comparison: source input card `source-input-detail.png` and implementation card `v3-input-detail.png`, combined in `v3-detail-comparison.png`. Their card content intentionally differs; the comparison checks display type, border/shadow, spacing, shape treatment, controls, and callouts.
- Responsive evidence: `v3-mobile.png`, captured at 390 by 844 CSS pixels as a full-page screenshot, plus programmatic layout checks at 1440, 1100, 1024, 820, 560, 390, and 320 pixel widths.

## Findings

No actionable P0, P1, or P2 findings remain in the inspected states.

- **Fonts and typography:** matches the source's system sans-serif body, heavy display headings with tight tracking, and monospace labels. Headline wrapping is intentional for the narrower two-column experiment layout. Inputs and prediction have a clear hierarchy; long empty/tie explanations remain readable.
- **Spacing and layout rhythm:** uses square cards, two-pixel black borders, hard offset shadows, approximately 22-pixel card padding/gaps, and a header divider. The input and map share the desktop workspace. Data editing and experiments extend below it because Shape Neighbor has more controls than the source. Navigation makes those sections directly reachable.
- **Colors and tokens:** uses the reference's `#101114`, `#f4f1e8`, `#dbff4a`, `#ff6b35`, and `#4f74ff`. Lighter map-region colors preserve plot legibility. Point shapes, outlines, labels, and vote text supplement color. The current-input diamond is lime with a dark outline.
- **Image/asset fidelity:** the reference's main experience uses code-rendered numeric stimuli rather than photography. Shape Neighbor retains its functional rectangle previews and SVG data visualization, as required by the existing product. No decorative raster substitutes, generated art, external fonts, icon libraries, or image dependencies were added.
- **Copy/content:** simple English explains the rectangle experiment. Quick tries explicitly preserve samples and k. The map's grid approximation, exact input calculation, non-probabilistic distances/votes, and geometric-guide distinction are retained. Map instructions are expandable; no fake training process was introduced.
- **Interaction and accessibility:** native labeled controls, visible keyboard focus, selected preset states, live result/status text, semantic tables, input validation, and post-deletion focus are retained. Reduced-motion preferences disable smooth anchor scrolling. Narrow tables scroll within their own containers without causing page overflow.

## Comparison history

1. Captured and opened the supplied One Pixel reference and the prior Shape Neighbor implementation before editing. Identified the intended source palette, typography, borders/shadows, compact experiment controls, and dark insight panel.
2. Implemented the reference-based interface adaptation while retaining the classifier and data operations.
3. Opened the combined full-view reference/implementation sheet and the narrow-screen implementation; then opened a combined focused input-card comparison. The implementation preserves the source language. Differences in plot, data table, feature count, and page height are intentional product adaptations. No visual repair cycle was required after this comparison.

## Verification

- Reran 20,000 default input/mode comparisons against an independent oracle.
- Reran real Edge `file://` offline tests for data editing, add/delete, duplicates, empty/insufficient samples, both tie rules, resets, exact versus displayed distances, map/table consistency, and numeric/keyboard controls.
- Tested every Quick tries button, preserving edited samples and k, selected-state updates, keyboard activation, help expansion/collapse, and section navigation.
- Checked seven responsive widths and inspected desktop/mobile screenshots. Browser regression checks reported no page JavaScript errors or external requests.
- Confirmed `one-pixel.html` is unchanged. The standalone HTML still requires no build or external runtime dependency.

## Remaining manual checks

- Screen readers, other browsers, real touch devices, and the student's subjective classroom usability review.
- Temporary screenshots are not bundled in Git; the captured source and implementation can be rendered again from their HTML files.

## Implementation checklist

- [x] Reference captured before changes; combined visual comparisons inspected.
- [x] Core classifier and data operations preserved and retested.
- [x] New input shortcuts and navigation tested.
- [x] Original source files preserved; attribution included.
- [x] Responsive layout and keyboard behavior checked.
- [x] Ready for the user-authorized GitHub and Vercel publication.

final result: passed
