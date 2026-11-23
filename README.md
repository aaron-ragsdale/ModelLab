# Model Lab

A browser-based computer vision development sandbox from **OpenSportSense**. Model Lab helps you see what pose models are doing, measure their performance on the hardware you're using, and compare devices without needing an API key or a local server.

**Live site (after GitHub Pages is enabled):** https://aaron-ragsdale.github.io/ModelLab/

## Getting started

1. Open the GitHub repository's **Settings → Pages**.
2. Under **Build and deployment**, select **Deploy from a branch**.
3. Select the **main** branch and **/(root)** folder, then save.
4. Open the published site using **HTTPS** and select **Start camera + model**.
5. Allow camera permission and stand in view of the camera.

The entire app is in `index.html`. It runs on GitHub Pages without a backend, Node installation, API key, or Python server.

## Experiments

- **Modes:** Body pose only (33 landmarks per person), hands only (21 landmarks per hand, up to two hands), or body pose + hands concurrently.
- **Models:** Google MediaPipe Pose Landmarker Lite, Full, and Heavy when pose is enabled; MediaPipe Hand Landmarker when hands are enabled.
- **Delegates:** GPU (WebGL) and CPU (WebAssembly). If GPU initialization fails, the app attempts a CPU fallback and labels that result.
- **Input requests:** 320 × 240, 640 × 480, and 1280 × 720. The page reports the resolution actually delivered by the camera, which may differ.
- **Multiple poses:** 1, 2, or 4.
- **Processing targets:** 5–60 inferences per second. A target is a maximum attempt rate, not a guaranteed achieved rate.
- **Inspection:** Live skeleton/landmark overlay and device/browser capabilities.
- **Performance:** Processed FPS, mean inference time, P95 inference time, model initialization time, and inference history. In combined mode, separate Pose and Hand inference timings are also shown.
- **History:** Save benchmark results to browser local storage and export a CSV to compare machines.

## Interactive walkthrough

Select **Walk me through the lab** near the top of the page. Each of the eight steps explains a section, what its controls do, and why the settings affect inference performance. The walkthrough highlights the relevant part of the page.

## How to run a fair comparison

1. Use the same browser, position, lighting, and camera scene.
2. Run a configuration for at least 15 seconds before saving.
3. Use **Compare Lite, Full & Heavy** for an automatic guided pose-only comparison. Then test **Hands only** and **Body pose + hands** manually with matching settings.
4. Repeat with **CPU vs. GPU**, keeping all other settings the same.
5. Repeat on an iPad or an 8 GB laptop and export CSV files from each device.
6. Record unusual cases such as multiple people, rapid motion, and low lighting.

Changes to model, delegate, camera resolution, and pose count take effect when you stop and restart. The target FPS slider works live.

## One-click 8-second benchmarks

Under **Experiment Controls**, choose **Body pose only**, **Hands only**, or **Body pose + hands**. Then, under the camera, select **Run 8-second test + auto-save**. Model Lab loads the selected pipeline, measures eight seconds, saves the run automatically, and updates the comparison chart. You do not need to manually press Save for this workflow.

Use **Compare all 3 modes** to automatically test Body Pose, Hands Only, and Body + Hands sequentially, using the selected pose model and other settings. The top **Compare Pose Lite / Full / Heavy** button remains available for pose-model size comparisons.

For longer unscripted measurements, choose **Start continuous camera**, then **Save current run** after sufficient inference samples. **Stop / cancel** stops a manual camera run or cancels an automated sequence. Benchmark data stays local to the browser.

## Compare any saved benchmarks

The **Compare saved benchmarks** chart can compare Body Pose, Hands Only, Body + Hands, and runs using different CPU/GPU backends.

1. Run a configuration for 15–20 seconds and select **Save benchmark**. Stop and repeat with another mode or setting.
2. Scroll to **Compare saved benchmarks**. Choose a quick selection (latest tests, pose-only, hand/combined, or CPU/GPU) or select individual saved runs.
3. Choose **Mean inference time**, **P95 inference time**, or **Processed FPS**.
4. Check the interpretation and comparison caution. The chart flags mismatched camera resolution, target FPS, max poses, backend, or browser/device.

The chart loads the latest 12 saved runs into its selection list. Run data stays in browser local storage until cleared or exported as CSV. A shorter latency bar is better; a longer processed-FPS bar is better. These are timing comparisons, not accuracy measurements.

## Understanding the metrics

| Measurement | What it means |
| --- | --- |
| Mean / P95 inference time | Elapsed time for the synchronous MediaPipe `detectForVideo` call. Includes more than neural network computation alone. |
| Processed FPS | Completed inference calls per second, not the camera capture or display frame rate. |
| Model initialization | Time to create the model instance. This can include loading cached/downloaded model data and initializing the runtime. |
| Device memory | Optional, coarse browser-reported estimate, **not** memory used by the app. |
| JS heap used | Optional JavaScript heap reading, **not** total RAM, WASM allocations, or GPU memory usage. |
| Resolution | Video stream dimensions requested and received, **not** necessarily the neural network's internal tensor size. |

Measurement is affected by device load, thermal throttling, browser, camera FPS, model caching, and GPU delegate support. On some Apple browsers, hardware diagnostics and memory APIs are not exposed.

## Privacy and network access

The camera is processed in the browser; Model Lab does not upload footage or landmark recordings. The browser must initially fetch MediaPipe JavaScript/WASM and model files from public CDNs/storage. Results saved with **Save benchmark** are stored only in the current browser's local storage until exported or cleared. This initial version does not save camera frames.

## Stack

- Static HTML, CSS, and JavaScript
- MediaPipe Tasks Vision (`@mediapipe/tasks-vision`)
- GitHub Pages hosting
- Local Storage and CSV export

## Roadmap

- Face/object detector benchmarks and automated cross-mode comparisons
- Comparison charts across saved runs
- Custom ONNX movement classifiers and stage-by-stage timing
- A data collection tab for recording labeled landmark sequences
- Hardware capability presets and reproducible benchmark scenes
- More detailed memory instrumentation where browser APIs permit

## License / contributions

This is an experimental OpenSportSense development tool. Check the repository for licensing information before redistributing code.
