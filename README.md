> 🚧 **Status: active development.** It works for the author; interfaces may change.

# karincaai-lightpanda-rehberi

[Türkçe](README.tr.md)

A Turkish-language beginner's guide (a single static web page) to [Lightpanda](https://lightpanda.io/), a lightweight headless browser engine: what it is, which tasks it suits, and when to keep the Chrome / automatic path instead. It ends with a small, controlled benchmark of Lightpanda against Chrome on one VPS.

The guide exists because "is the lighter engine good enough for my agent's browsing tasks?" is easy to answer wrongly from marketing numbers; this one tells you how to decide with your own URLs and success criteria, and when to stop.

## What is in the repository

- `index.html` — the guide (Turkish). Sections: a 5-question decision path, a Lightpanda vs Chrome comparison, a safe-experiment boundary, success / stop criteria, sources and a change log.
- `benchmark-report.md` — method, results and limits of the benchmark (Turkish).
- `benchmark-data.json` — the raw benchmark data.
- `assets/` — images used by the page (signature, benchmark chart, social preview card).

To read it locally open `index.html` in a browser, or serve the folder: `python -m http.server 8000 --bind 127.0.0.1`.

## The measurement (30 August 2026)

One Ubuntu x86_64 VPS (15 GiB RAM), Chrome 146.0.7680.164 and Lightpanda `1.0.0-nightly.9002+1bd957d07`, both running as persistent CDP engines on localhost, 20 deterministic local pages that each build 200 DOM items with JavaScript.

| Measure | Lightpanda | Chrome |
|---|---:|---:|
| Median navigation + DOM-ready time | 4,992 ms | 25,129 ms |
| p95 time | 7,688 ms | 28,709 ms |
| Start-up process-tree RSS | about 24 MB | about 1,001 MB |
| Tasks completed | 20/20 | 20/20 |

## Limits (please read)

- Local pages, no internet latency; controlled and simple pages that do not represent the JavaScript/API complexity of real sites.
- Engines were already running: this is **not** a cold-start benchmark.
- A single nightly build of Lightpanda against one Chrome version; screenshots, PDF, file upload, persistent profiles, login flows and bot protection were not tested.
- It does **not** show that Lightpanda is "5x faster and uses 41x less RAM" on every site. It only supports the verdict that a controlled pilot is reasonable for repetitive text/DOM work.
- The scripts that generated the pages and ran the benchmark are not included, so the numbers cannot be reproduced from this repository alone. Page checks at 320 / 390 / 1440 px and with JavaScript disabled were done by hand at publication time and are not re-run in CI.
- Measured values are dated; re-check them before relying on them.

## Turkey-specific notes

The guide itself is written in Turkish. The Turkish README is `README.tr.md`. This repository and the site do not post anything automatically.

## Licence

The written guide and benchmark report: CC-BY-4.0 (`LICENSE`). Code (the page script and any code snippets): Apache-2.0 (`LICENSE-APACHE-2.0`). Copyright (c) 2026 Kemal (KarıncaAI). Report problems privately, see `SECURITY.md`.
