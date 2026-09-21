# Homework 2: My Tiny Classifier

Author: Sining Chen

This project started with the One Pixel ML demo. The original `one-pixel.html` is preserved unchanged.

## Shape Neighbor: version 2.3

`your-new-lab.html` is a self-contained classroom experiment in nearest-neighbor classification.
It uses numeric width and height, both whole numbers from 1 to 100, and the labels **Horizontal** and **Vertical**.
It is not image recognition and does not need neural-network training, a training animation, or training epochs.

Version 2 extends the same project with:

- 24 fixed default samples: 12 Horizontal and 12 Vertical. They cover small, medium, and large shapes, including thin and almost-square rectangles. All default labels match geometric direction, none is square, and the set is not entirely made of mirrored pairs.
- Width and height sliders synchronized with numeric inputs, a fixed-scale rectangle preview, and the current prediction and vote counts.
- Editable sample labels, adding the current rectangle with a chosen label, deleting individual samples, and restoring the defaults.
- Duplicate-coordinate prevention: if the same width and height already exist, a message identifies that sample and focuses its label control instead of adding another copy.
- A two-dimensional map with Width on the horizontal axis and Height increasing upward. Horizontal samples are blue circles; Vertical samples are mauve squares. A rose diamond with a dark outline marks the current input. Lines and outlines identify every participating neighbor.
- Pale model-prediction regions, k = 1 / k = 3 controls, and a nearest-sample table with IDs, dimensions, labels, distances, and voting status.
- An optional dashed width = height line, explicitly labeled as a geometric guide rather than a model boundary.
- Short experiments for changing a label, adding a sample, and comparing k values.

## Open the current version

1. Open File Explorer and go to `C:\Users\siningchen\Desktop\课\ai\Sining_Chen_HW2`.
2. Double-click **your-new-lab.html**. If needed, right-click it and choose **Open with → Microsoft Edge** or another browser.
3. Enter width and height in the number boxes, or use the sliders. A focused slider supports arrow keys.
4. Scroll to the training-data table to change labels or delete samples. On narrow screens, the tables scroll horizontally as well as vertically.

The page includes all HTML, CSS, and JavaScript. It works directly from disk with JavaScript enabled and makes no network requests. There are no external libraries, APIs, paid services, application dependencies, or build steps.

Live app: [Shape Neighbor](https://shape-neighbor.vercel.app). Source and version history: [GitHub repository](https://github.com/Serenasnchen/Sining_Chen_HW2). You can also open the local HTML directly.

## Guided experiments and live comparison: version 2.3

Use **Experiments** in the top navigation, or scroll to **What changes the answer?** Each experiment button replaces the current dataset, sets the input, and selects k = 1. This is stated beside the buttons; no modal or separate page is involved.

| Experiment | Starting change | What to observe |
| --- | --- | --- |
| Normal labels | Original 24 samples; input 78 × 22 | This is not a training coordinate. The computed nearest voter is H09 (80 × 20), distance 2.83, predicting Horizontal. |
| One odd label | Defaults with only H09 changed to Vertical; input 80 × 20 | k = 1 predicts Vertical. At the same input, k = 3 predicts Horizontal because the other two voters are Horizontal. More neighbors can offset an odd label, but k = 3 is not always more accurate. |
| Reversed labels | Defaults with every label exchanged; input 78 × 22 | Coordinates stay fixed. At the same input and k, predictions swap between Horizontal and Vertical; Tie stays Tie. The model follows the labels rather than a geometric rule. |

Results in this table are calculated expectations and verified test results, not student observations. On the page, predictions, voters, distances, and votes come from the existing `classify` function, never hardcoded experiment answers.

**Before** uses an immutable copy of the default data saved when the experiment starts. **After** uses the current editable samples. Both sides recompute for the same current width, height, and k. The comparison shows each prediction, actual votes, all voting neighbors and distances, data changes, and two small model-region maps. The main map and nearest-distance table continue to show After. Backgrounds use the same 50 × 50 grid helper and classifier; exact input predictions do not use grid approximations.

Manual label changes, additions, and deletions update After while Before stays fixed. Moving the input, switching k, Quick tries, and Reset input update both results without replacing either dataset. Equal-distance cutoff ties, tied votes, insufficient data, and empty data use the existing classifier rules.

- **Exit comparison** clears the experiment state and guidance, preserving current samples, input, and k.
- **Restore defaults** also exits comparison while keeping its existing input/k preservation behavior.
- **Start another experiment** always begins from a fresh default set, regardless of earlier edits, additions, removals, or a previously empty dataset.
- **Refresh** clears the experiment and restores the usual initial page state.

Actual checks for this addition used offline `file://` headless Edge: all three experiments started from edited, empty, and reversed datasets (nine combinations); expected Normal/One odd label results; 20,000 default input/k combinations for the reversal property, including 13 tied-vote cases; independent Euclidean checks of comparison votes, voters, and distances; and all 2,500 cells in both comparison maps after representative changes. Checked input/k/label/add/delete synchronization, duplicate rejection, input-only reset, empty/insufficient data, k = 1 and k = 3 distance ties, exit/restore/reload cleanup, and keyboard activation. The existing 20,000-case classifier and browser regression suite also passed. No network requests or JavaScript errors occurred.

Checked layout widths 1440, 1024, 820, 620, 390, and 320 pixels without overall overflow, and inspected desktop/mobile screenshots of the new section. Real touch devices, screen readers, other browsers, and your own classroom usability test remain manual checks.

## Soft pink Y2K interface: version 2.2

The user selected the attached pink/chrome layout and asked to use the gentler pink from a second texture image. This replaces the earlier One Pixel-inspired appearance while keeping the same project and classifier.

- Soft blush background based on the actual supplied texture (average color approximately `#fed8df`), pale translucent panels, rose controls, lavender/mauve map colors, and a generated chrome wordmark. Both images are embedded in the HTML, so opening the page offline still works.
- Desktop layout: input and k controls on the left; a wide map on the right; prediction and nearest-neighbor evidence together below it. Samples and experiments follow on the same page.
- Experiment / Samples / About anchors, quick test inputs, and Add current input / Edit labels shortcuts. These shortcuts move keyboard focus to the existing form or sample label; they do not create an extra editing mode.
- Voter IDs and dimensions appear beside up to six neighbors on wider plots. The nearest table always lists every voter, including larger tied groups. Map labels are omitted on small screens to avoid crowding.
- Map axes adapt to screen width. Their physical lengths may differ; distance is always calculated from numeric width and height, not screen pixels. The rectangle preview still uses the original fixed scale.
- Retained all 24 defaults, both k modes, full-precision voting, editable labels, add/delete, validation, empty states, and separate resets. The classifier and default data were checked against the previous commit and are unchanged.

The original `one-pixel.html` is untouched. The footer retains attribution to the course lab by [Xiuye Chen](https://github.com/xiuyechen), CC BY 4.0, and identifies this interface as redesigned for Shape Neighbor.

Checks actually completed: all 20,000 input/mode cases against an independent oracle; offline direct-file Edge tests for edits, duplicates, empty/insufficient data, ties, resets, input synchronization, keyboard controls, and map/table consistency; rounded-distance collision case; new focus shortcuts, embedded image loading, and responsive map changes. Checked page overflow at 1660, 1487, 1250, 1100, 1024, 900, 820, 621, 620, 560, 390, and 320 pixels. Inspected combined reference/implementation screenshots and narrow layouts; see [design QA](design-qa.md).

These were automated headless Edge checks and visual inspection by the assistant. Screen readers, other browsers, real touch devices, and the student's usability impressions remain to be tested; no student observations are claimed.

## Separate resets and input validation

| Action | What changes | What stays |
| --- | --- | --- |
| Reset input | Width 60, height 30 | Edited samples and current k |
| Restore default samples | Original 24 samples and labels; removes added samples | Current input and k |
| Refresh the page | Defaults, input 60 by 30, k = 1, guide off | No edits are saved between page loads |

Restore default samples and refresh also clear guided comparison state. Reset input keeps an active comparison and recomputes both sides at 60 × 30.

Invalid number-box entries (empty, fractional, or outside 1–100) show a message and leave the model at the last valid input. Leaving the invalid field restores that valid value. The preview caption and Add panel always show the dimensions actually being used.

## Prediction method

For every sample, compute Euclidean distance:

```text
distance = sqrt((width - sample width)^2 + (height - sample height)^2)
```

The implementation orders and compares **unrounded squared distances**, which preserve Euclidean ordering and exact ties for integer inputs. Square roots are used for displayed distances, rounded to two decimals. No rounded display value is used to select voters.

1. Order all samples by distance to the current input.
2. **k = 1:** include every sample at the nearest distance.
3. **k = 3:** find the third-nearest distance and include every sample at or inside that distance. Ties at the third distance can produce more than three voters.
4. Each included sample gives one vote to its current label. The greater vote count wins; equal counts produce **Tie**. The page reports the actual number of voters, both vote counts, and any cutoff tie.
5. With fewer than k samples, use all available samples and display a shortage message. With none, display **Please add samples first**, clear the neighbors and model regions, and show no stale prediction.

Sorting equal-distance rows by ID only makes the table order stable; it never excludes tied voters. A direct comparison such as `width > height` does not replace the model. Editing a sample to a geometrically incorrect label affects the prediction naturally through the same correct algorithm.

The exact input prediction and the map use the same classifier. The map evaluates a **50 by 50 grid at cell centers**, so its background is an approximation of the prediction regions, not an exact boundary. Moving the input updates its exact prediction, map position, links, and nearest table. Changing data or k also redraws the background. The table shows the five nearest samples, or more when needed to include every voter.

Distances and vote counts are **not confidence probabilities**. The optional geometric guide never participates in classification.

## Default data

These dimensions are fixed and reproducible. Labels can be changed in the page.

| Horizontal ID | Width × Height | Vertical ID | Width × Height |
| --- | --- | --- | --- |
| H01 | 16 × 7 | V01 | 6 × 19 |
| H02 | 23 × 18 | V02 | 17 × 25 |
| H03 | 34 × 12 | V03 | 14 × 39 |
| H04 | 38 × 32 | V04 | 29 × 36 |
| H05 | 49 × 22 | V05 | 24 × 53 |
| H06 | 55 × 46 | V06 | 43 × 51 |
| H07 | 60 × 30 | V07 | 30 × 60 |
| H08 | 68 × 57 | V08 | 54 × 67 |
| H09 | 80 × 20 | V09 | 20 × 80 |
| H10 | 83 × 70 | V10 | 66 × 78 |
| H11 | 95 × 48 | V11 | 45 × 96 |
| H12 | 96 × 89 | V12 | 87 × 97 |

The main preview uses 2 CSS pixels per unit; table thumbnails use 0.45 CSS pixels per unit. Each preview area uses its own fixed scale, preserving proportions and visible size differences.

## Suggested experiments

Restore defaults before comparing these expected results. They are calculated expectations, not claimed student observations.

| Input (width × height) | k = 1 | k = 3 | What to explore |
| --- | --- | --- | --- |
| Easy: 80 × 20 | Horizontal; H09 | Horizontal; H09, H07, H05 | Edit H09 to Vertical, then compare modes |
| Near the boundary: 50 × 50 | Horizontal; H06 | Vertical; H06, V06, V08 | Count votes and compare the map with the geometric guide |
| Unusual: 1 × 100 | Vertical; V09 | Vertical; V09, V11, V07 | A very thin shape at the edge of the map |
| Unusual: 1 × 1 | Horizontal; H01 | Horizontal; H01, V01, H02 | A square still gets a sample-based prediction |
| Unusual: 100 × 100 | Horizontal; H12 | Horizontal; H12, V12, H10 | Limited examples need not give a geometric square a Tie |

For an explicit tie experiment, delete the samples and add (49, 50) as Horizontal and (51, 50) as Vertical. At input (50, 50), k = 1 includes both and predicts Tie. Add (50, 48) as Horizontal and (50, 52) as Vertical: k = 3 now includes four voters and also predicts Tie. Restore defaults afterward.

## Checks actually performed for version 2

Tests used the actual embedded JavaScript and the installed Microsoft Edge browser through a temporary Playwright test harness outside the project. The website itself has no test-tool dependency.

- Verified 24 unique default coordinates, 12 labels of each class, integer values in 1–100, geometrically consistent labels, no default squares, and a set that is not entirely mirrored.
- Compared both modes over all 10,000 valid inputs each (**20,000 cases**) with an independent Euclidean-distance oracle. Checked distances, participating IDs, vote counts, and predictions. The default-data sweep included 118 cases with more voters than k and 13 tied-vote cases across the two modes.
- Opened the HTML directly with a `file://` URL in **headless Edge with network access disabled**. No external requests or JavaScript errors occurred.
- Used real browser controls to check label edits, add/delete operations, duplicate rejection and focus, default restoration, input-only reset, and deterministic defaults after reload.
- Deleted every sample through the page, checked the empty state, then built test datasets with the Add form. Checked fewer-than-k cases, k = 1 equal-distance ties, same-label ties, k = 3 cutoff ties, majority voting, and six participating neighbors being fully listed.
- Verified that map lines, point locations, nearest-table rows, displayed distances, predictions, and vote counts agree. Checked all 2,500 background cells against independent distance/vote calculations after representative data and mode changes.
- Verified full-precision selection even when two distances display as **110.02**: at input (100, 100), sample (2, 50) is closer than (1, 52). Reversing array order does not change the result.
- Checked slider/number synchronization, arrow-key input, keyboard radio switching, keyboard label editing and deletion, focus after deletion, invalid numeric entries, and separate reset behavior.
- Inspected desktop and narrow-screen screenshots. Checked that 1440-, 820-, 390-, and 320-pixel layouts have no overall page overflow; narrow tables support their own horizontal scrolling.
- Confirmed the original `one-pixel.html` content is unchanged and reviewed the Git diff for whitespace errors.

These are automated browser checks and screenshot inspection, not a claim that the student has tested version 2. Screen-reader testing, other browsers, real touch interaction, and subjective classroom usability still need manual review.

### Still to try yourself

Open the local page, run the suggested experiments, and decide whether the controls, map, vote explanation, and table make the changes easy to understand. Try your own incorrect label, add/delete a sample, and verify both reset buttons do what you expect. Check your usual browser, zoom level, and narrow-window or touch experience.

## Existing Vercel setup

The earlier hosting configuration serves `your-new-lab.html` at `/`. `.vercelignore` includes only that page and `vercel.json`; the original demo stays local. `.gitignore` excludes local Vercel metadata and environment files.

Version 2 was initially committed locally. The user subsequently authorized publishing each completed version to GitHub and Vercel. This interface refresh includes the version 2 functionality and uses the existing repository and Vercel project. Continue publishing checked versions unless the user changes that instruction.

Vercel is connected to the GitHub repository; a push can trigger a deployment. Direct deployment is also available with `npx.cmd --yes vercel --prod`. The deployment allowlist excludes the original demo, README, design QA, Git metadata, and environment files.

## Development log

### 2026-09-21 — First Shape Neighbor version

- Request: build a simple, self-contained nearest-example rectangle classifier, preserve the original One Pixel page and unrelated work, document the method and checks, and commit the relevant changes with `Build first Shape Neighbor classifier` without pushing to GitHub.
- Implementation: added `your-new-lab.html` with two integer sliders, a fixed-scale preview, four fixed reference examples, Euclidean distances, complete nearest-example highlighting, explicit tie handling, prediction explanations, and Reset.
- Verification at that time: checked all 10,000 valid inputs with Node.js and checked page handlers with a simulated DOM. Actual browser checks were not performed for version 1.
- Scope: reference-label editing and further development are deferred until the student opens and tests this version. No student observations, classmate feedback, or personal reflection have been supplied or invented.

### 2026-09-21 — Vercel deployment

- Request: publish the first version to Vercel. The user completed Vercel's account authorization.
- Added static hosting configuration, a homepage route, and deployment/Git exclusions. Created the `shape-neighbor` Vercel project and deployed to https://shape-neighbor.vercel.app.
- The final upload contained only `your-new-lab.html` and `vercel.json`. An earlier attempt that included the original page was rejected by automatic approval review and did not execute; the upload was narrowed before retrying.
- Verified public HTTP 200 responses for `/` and `/your-new-lab.html`, with response bodies exactly matching the local classifier. Verified `/one-pixel.html` returns HTTP 404. No new browser-interaction or visual checks were performed.
- Vercel connected the existing GitHub repository during project setup. This deployment used local files; no Git commits were pushed to GitHub. The classifier code and original demo were unchanged.

### 2026-09-21 — Version 2: editable data and decision map

- Actual user feedback: the first version had only four fixed samples and offered too few experiments. The user requested more samples, editable data, and visible classification regions in the same project.
- Added 24 reproducible default examples, editable labels, add/delete controls, duplicate-coordinate prevention, separate input reset and data restoration, numeric input boxes, k = 1 / k = 3 voting, and a shared-model decision map with sample symbols, the current input, and all participating-neighbor links.
- Made cutoff ties, equal votes, insufficient samples, and empty datasets explicit. Kept full-precision calculations; no direct geometry rule, fake training process, or confidence probabilities were added.
- Completed the version 2 automated calculations, real headless Edge interaction checks, map consistency checks, and screenshot inspection listed above. Manual student usability testing is still pending.
- Preserved `one-pixel.html` and the existing deployment configuration. Requested commit: `Expand Shape Neighbor with editable samples and decision map`. No GitHub push or Vercel deployment was made for version 2.
- No classmate feedback, personal reflection, or additional guidance records were supplied or invented.

### 2026-09-21 — Version 2.1: One Pixel-inspired interface

- Actual user feedback: the existing UI/UX was unattractive. The user provided the course One Pixel HTML as the visual and interaction reference and authorized publishing each version to GitHub and Vercel.
- Adapted the supplied design language, made the prediction more prominent, shortened the always-visible explanations, and added quick input examples and page-section navigation. Preserved all model/data features and the original demo.
- Reran the calculation and real-browser regression checks, tested the new controls and seven viewport widths, and compared reference/implementation screenshots. The model itself was not changed.
- Publication uses the existing GitHub repository and Vercel site. Only the classifier page and routing configuration are included in the Vercel payload.
- No classmate feedback, personal reflection, or student usability observations were invented.

### 2026-09-21 — Version 2.2: selected soft pink Y2K design

- Actual user feedback: the previous interface resembled the teacher's example too closely. The user wanted an individual pink Y2K style, then said the pink was too saturated. After comparing directions, the user supplied a selected pink/chrome layout and a second pale pink texture and asked to implement that combination.
- Rebuilt the interface around those attachments, embedded the supplied texture and a generated chrome title asset, widened the map, grouped prediction with neighbor evidence, and added direct shortcuts to sample editing. This remains the same self-contained HTML project.
- Preserved the original demo, default training set, exact classifier, all data operations, and attribution. No reference labels or predictions were changed to imitate the mockup's illustrative plot.
- Reran the model/browser checks documented above. Compared full and focused screenshots with the selected target; tightened desktop spacing after the first comparison and checked the revised layout. Manual student testing is still pending.
- The user's earlier authorization to commit, push to GitHub, and deploy checked versions to the existing Vercel project still applies. Only the classifier and routing configuration are included in the Vercel deployment allowlist.
- No classmate feedback, personal reflection, or additional guidance records have been invented.

### 2026-09-21 — Version 2.3: guided label experiments

- Source of this change: the user's project-review request for one-click experiments and fair Before / After comparisons, borrowing the teaching approach of the course One Pixel demo. This is project review feedback, not classmate feedback.
- Added Normal labels, One odd label, and Reversed labels buttons in the existing experiment section. Each starts reproducibly from the default 24 samples and sets input/k. Kept the pink interface, main map, nearest-distance table, manual data tools, and classifier.
- Added an immutable default snapshot and live current-data comparison: shared current input/k, real votes and distances, data-change summary, and paired region maps. Restoring defaults or exiting clears comparison state; input-only reset preserves it.
- Verified the nine dirty-state starts, expected H09/2.83 result, odd-label k comparison, all 20,000 reversal cases including Tie, independent map/vote checks, data editing, reset/cleanup behavior, offline operation, keyboard controls, and six responsive widths. Reran the existing classifier/browser regression suite and inspected new desktop/mobile screenshots.
- Latest user instruction: create a meaningful commit and push directly to the existing GitHub repository. The connected Vercel project may deploy that push automatically; no separate deployment command is required for this request.
- Original one-pixel.html was unchanged. No student observations, classmate feedback, personal reflection, or accuracy claims were invented.
