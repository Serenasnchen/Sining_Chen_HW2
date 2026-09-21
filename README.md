# Homework 2: My Tiny Classifier

Author: Sining Chen

This project starts with the One Pixel ML demo in `one-pixel.html`, which is preserved unchanged.

## Shape Neighbor: first version

`your-new-lab.html` is a small interactive classifier for CPSC 1710 Homework 2.
Visitors adjust a rectangle's numeric width and height using integer sliders from 1 to 100.
The page predicts **Horizontal**, **Vertical**, or **Tie** by comparing the input with four fixed labeled examples.
This is a classroom demonstration using numeric dimensions, not image recognition.

All five shapes use the same scale: one dimension unit equals two CSS pixels.
The shapes keep their proportions, and changes in size remain visible.
The page shows every example's distance and marks every equally nearest example.
Reset restores width 60 and height 30.

## How to open the page

1. Open File Explorer and go to `C:\Users\siningchen\Desktop\课\ai\Sining_Chen_HW2`.
2. Double-click `your-new-lab.html`. If needed, right-click it, choose **Open with**, and select a browser such as Microsoft Edge or Google Chrome.
3. Move the Width and Height sliders. For precise integer changes, click a slider and use the arrow keys.

The file contains its own HTML, CSS, and JavaScript. It works directly from disk with JavaScript enabled; no server, build step, external dependencies, API keys, or paid services are required.

## Vercel hosting

Live page: [Shape Neighbor](https://shape-neighbor.vercel.app).

The Vercel configuration serves `your-new-lab.html` at the website's root URL.
The original demo remains in the local project and is excluded from this deployment.
`.vercelignore` limits the deployment to `your-new-lab.html` and `vercel.json`.
The local Vercel project link in `.vercel/` and environment files are excluded from Git.

To deploy from this folder with Node.js and npm installed:

```powershell
npx.cmd --yes vercel login
npx.cmd --yes vercel --prod
```

This uploads the local files to Vercel directly; it does not push commits to GitHub.
Vercel linked the project to the existing GitHub repository during setup; future pushes can trigger Git-based deployments.
The original double-click opening instructions still work.

## Classification method

| Example | Width | Height | Label |
| --- | ---: | ---: | --- |
| A | 80 | 20 | Horizontal |
| B | 60 | 30 | Horizontal |
| C | 20 | 80 | Vertical |
| D | 30 | 60 | Vertical |

For each example, calculate Euclidean distance:

```text
distance = sqrt((width - example width)^2 + (height - example height)^2)
```

The classifier finds the smallest distance and includes every example at that distance.
If these examples all have the same label, it predicts that label.
If their labels differ, it displays **Tie** and explains that both classes are equally close.
It does not use a direct `width > height` prediction rule.
Classification uses full JavaScript numeric precision; only the displayed distances are rounded to two decimal places.
Distance is not presented as a confidence percentage.
Reference labels are fixed; label editing is outside this first version.

## Inputs to try

| Try | Width | Height | Expected result |
| --- | ---: | ---: | --- |
| Easy: exact reference | 80 | 20 | Horizontal; A is nearest with distance 0.00 |
| Near the boundary | 50 | 49 | Horizontal; B is nearest |
| Exact mixed-label tie | 50 | 50 | Tie; B and D are both nearest, each displayed as 22.36 |
| Other side of the boundary | 49 | 50 | Vertical; D is nearest |
| Unusual: very thin rectangle | 1 | 100 | Vertical; C is nearest |
| Same-label tie | 70 | 25 | Horizontal; A and B are both nearest, each displayed as 11.18 |

## Checks performed and checks still needed

Automated checks were run with Node.js against the actual embedded JavaScript:

- Checked distances and predictions for all 10,000 valid integer input pairs against an independent squared-distance calculation and `Math.hypot` distance calculation.
- Verified the nearest-example sets, including 86 inputs with same-label ties and 100 inputs with mixed-label ties.
- Used a simulated DOM to run the page's initial rendering, both slider input handlers, and the Reset click handler. Checked current values, preview dimensions and caption, reference shapes, displayed distances, nearest highlights, prediction text, and explanations.
- Checked exact references, boundary inputs, both kinds of tie, and extreme dimensions, including 1 by 1, 100 by 100, 100 by 1, and 1 by 100.
- Checked slider limits and integer steps in the HTML and reviewed the file for external dependencies.

Browser automation was unavailable in this session. These checks did **not** include actual browser rendering, physical mouse/keyboard interaction, or screen-reader testing.

Still to do in your browser:

1. Try the inputs above using the sliders and arrow keys, and confirm the values and results update immediately.
2. Check that every nearest example is marked and that shapes keep their proportions as their sizes change.
3. Change both sliders, then click Reset. Confirm width 60, height 30, prediction Horizontal, and only B marked Nearest with distance 0.00.
4. Narrow the browser window and check that the layout is readable and keyboard focus is visible.

## Development log

### 2026-09-21 — First Shape Neighbor version

- Request: build a simple, self-contained nearest-example rectangle classifier, preserve the original One Pixel page and unrelated work, document the method and checks, and commit the relevant changes with `Build first Shape Neighbor classifier` without pushing to GitHub.
- Implementation: added `your-new-lab.html` with two integer sliders, a fixed-scale preview, four fixed reference examples, Euclidean distances, complete nearest-example highlighting, explicit tie handling, prediction explanations, and Reset.
- Verification: completed the automated calculation and simulated-DOM checks listed above. Actual browser checks remain for the student.
- Scope: reference-label editing and further development are deferred until the student opens and tests this version. No student observations, classmate feedback, or personal reflection have been supplied or invented.

### 2026-09-21 — Vercel deployment

- Request: publish the first version to Vercel. The user completed Vercel's account authorization.
- Added static hosting configuration, a homepage route, and deployment/Git exclusions. Created the `shape-neighbor` Vercel project and deployed to https://shape-neighbor.vercel.app.
- The final upload contained only `your-new-lab.html` and `vercel.json`. An earlier attempt that included the original page was rejected by automatic approval review and did not execute; the upload was narrowed before retrying.
- Verified public HTTP 200 responses for `/` and `/your-new-lab.html`, with response bodies exactly matching the local classifier. Verified `/one-pixel.html` returns HTTP 404. No new browser-interaction or visual checks were performed.
- Vercel connected the existing GitHub repository during project setup. This deployment used local files; no Git commits were pushed to GitHub. The classifier code and original demo were unchanged.
