# Homework 2: My Tiny Classifier

Author: Sining Chen

This project started with the One Pixel ML demo. The original `one-pixel.html` is preserved unchanged.

## Shape Neighbor: version 2

`your-new-lab.html` is a self-contained classroom experiment in nearest-neighbor classification.
It uses numeric width and height, both whole numbers from 1 to 100, and the labels **Horizontal** and **Vertical**.
It is not image recognition and does not need neural-network training, a training animation, or training epochs.

Version 2 extends the same project with:

- 24 fixed default samples: 12 Horizontal and 12 Vertical. They cover small, medium, and large shapes, including thin and almost-square rectangles. All default labels match geometric direction, none is square, and the set is not entirely made of mirrored pairs.
- Width and height sliders synchronized with numeric inputs, a fixed-scale rectangle preview, and the current prediction and vote counts.
- Editable sample labels, adding the current rectangle with a chosen label, deleting individual samples, and restoring the defaults.
- Duplicate-coordinate prevention: if the same width and height already exist, a message identifies that sample and focuses its label control instead of adding another copy.
- A two-dimensional map with Width on the horizontal axis and Height increasing upward. Horizontal samples are blue circles; Vertical samples are orange squares. A dark-outlined diamond marks the current input. Lines and outlines identify every participating neighbor.
- Pale model-prediction regions, k = 1 / k = 3 controls, and a nearest-sample table with IDs, dimensions, labels, distances, and voting status.
- An optional dashed width = height line, explicitly labeled as a geometric guide rather than a model boundary.
- Short experiments for changing a label, adding a sample, and comparing k values.

## Open version 2 locally

1. Open File Explorer and go to `C:\Users\siningchen\Desktop\课\ai\Sining_Chen_HW2`.
2. Double-click **your-new-lab.html**. If needed, right-click it and choose **Open with → Microsoft Edge** or another browser.
3. Enter width and height in the number boxes, or use the sliders. A focused slider supports arrow keys.
4. Scroll to the training-data table to change labels or delete samples. On narrow screens, the tables scroll horizontally as well as vertically.

The page includes all HTML, CSS, and JavaScript. It works directly from disk with JavaScript enabled and makes no network requests. There are no external libraries, APIs, paid services, application dependencies, or build steps.

**Version 2 has not been deployed or pushed to GitHub.** The existing [Vercel site](https://shape-neighbor.vercel.app) still serves version 1 from the earlier deployment. Open the local file to try version 2.

## Separate resets and input validation

| Action | What changes | What stays |
| --- | --- | --- |
| Reset input | Width 60, height 30 | Edited samples and current k |
| Restore default samples | Original 24 samples and labels; removes added samples | Current input and k |
| Refresh the page | Defaults, input 60 by 30, k = 1, guide off | No edits are saved between page loads |

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

No deployment was made for version 2. After a future request to publish, the existing project can be updated from this folder with `npx.cmd --yes vercel --prod`. Vercel is also connected to the GitHub repository, so future pushes can trigger deployments. No GitHub push was performed for this update.

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
