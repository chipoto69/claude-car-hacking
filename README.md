# Claude, a cable, and a car

A dated, sourced list of the wildest things people have actually done by plugging an AI agent (mostly Claude / Claude Code) into a real car: OBD-II dongles, BMW ENET cables, J2534 pass-thru, USB-CAN adapters, and one $3 Arduino.

**Sweep date: 17 November 2026.** Every entry was checked against a primary source during that sweep. Engagement numbers are as observed on that date.

Each entry carries an evidence tag:

| Tag | Meaning |
|---|---|
| `repo` | Public code you can read and run |
| `paper` | Writeup with methodology published |
| `video` | Recorded run with the work shown |
| `thread` | Long public discussion with first-hand detail |
| `claim` | An internet post we could not verify beyond the post itself |

---

## The index

| # | What | Car | Date | Evidence |
|---|---|---|---|---|
| 1 | Claude + Codex reverse-engineer a 1994 Jeep ECU over a $3 Arduino, then pass California smog | 1994 Jeep Cherokee | 10 Nov 2026 | `video` `thread` |
| 2 | iDrive 7 head unit rooted over one ENET cable: app store, Waze on the cluster, Doom 3 on the dash | BMW iDrive 7/8 (MGU) | Oct-Nov 2026 | `repo` |
| 3 | Frontier models drive a real Corolla through a cone course over MCP | 2022 Toyota Corolla | Sep 2026 | `paper` |
| 4 | Claude runs BMW ISTA+ headlessly, reads test plans as text, walks to the faulty part | BMW G20 | Sep 2026 | `repo` |
| 5 | AI decodes BMW CAN and an ESP32 replaces the shifter; same trick for an i8 engine swap | BMW (various) | 2026 | `video` |
| 6 | A $35 Mini VCI cable becomes a J2534 MCP server for Toyota/Lexus | Toyota / Lexus | Sep 2026 | `repo` |
| 7 | AI clones a pickup ECU off an eBay spare and goes hunting for the immobilizer | GM pickup | late 2026 | `video` |
| 8 | Four Claude Code agents rendering on the car's centre screen, in a dealership | BMW, Porsche | Oct 2026 | `claim` |
| 9 | Proprietary CAN signals decoded into DBC files in minutes instead of weeks | any | Jun-Jul 2026 | `video` `repo` |
| 10 | A free macOS app that reads every BMW ECU over a cheap ENET cable, with MCP for your own Claude | BMW F/G series | 14 Nov 2026 | `repo` |

---

## 1. The 1994 Jeep that Claude taught to pass smog
**1994 Jeep Cherokee · 10 Nov 2026 · `video` `thread`**

The car predates OBD-II, so no scan tool talks to it. Muhammad Zulali wired a ~$3 Arduino into the engine computer and pointed Claude Code and Codex at it. Over about a week the agents read the ECU live, logged his drives, copied the entire program, and ran their own code inside it. It ended with the Cherokee passing California's smog test.

The moment everyone quotes: Claude suggested it could run the fuel pump on its own, he said "I don't think you can do that", and it primed the fuel rail for about ten seconds before he asked it to stop. First time an agent had changed something inside the car.

1.46M views, 2.2K comments, 450 points and 239 comments on Hacker News, plus Tom's Hardware coverage. The HN critique is worth reading too: a lean-at-certain-RPM tune raises NOx, and adding fuel moves it back toward lambda, cutting NOx while raising CO and HC. That likely masked a mechanical fault rather than fixing it.

- Video: https://www.youtube.com/watch?v=WPj3aQ-xxpI
- HN thread: https://news.ycombinator.com/item?id=50463399

## 2. iDrive 7 root over one ENET cable
**BMW iDrive 7 / 8 (MGU head unit) · Oct-Nov 2026 · `repo`**

The wildest BMW thread on the internet right now, and it is three weeks old. On 15 November, `f0rgent` posted: "Claude just jailbroke my BMW. Then it put DOOM 3 on the dash." The repo behind it is explicitly written to have Claude as the co-pilot: read the tool READMEs, verify the car read-only first, refuse to flash until a coding backup exists. One ENET cable, one laptop. Head unit only, it does not touch engine, brakes or airbags.

It sits on top of a community ecosystem that detonated in late October:

- `cauliflowermossgrape/idrive7-8-ssh-root`: flashes a debug image over ENET to get root. Open-sourced 22 Oct 2026.
- `robertnisipeanu/mgu-store`: an app store that runs on the car screen, install, update and uninstall from GitHub repos, URLs or USB. Created 27 Oct, past 723 stars in about two weeks.
- `robertnisipeanu/idrive-apps`: Waze / Apple Maps / Google Maps from CarPlay and Android Auto projected onto the instrument cluster, and experimentally the HUD. Created 31 Oct, 377 stars. iDrive 8 has this from the factory, iDrive 7 never did.
- Third-party install: tuning shops in Europe charge €100 to €150 to do the flash for you.

Earlier groundwork: a writeup from 30 Aug 2026 on enabling SSH and running Doom on an MGU18 unit.

- https://github.com/f0rgent/bmw-idrive-claude
- https://github.com/robertnisipeanu/mgu-store
- https://github.com/robertnisipeanu/idrive-apps
- https://rishik.de/blog/bmw_mgu_ssh/

## 3. Claude drove a real Toyota Corolla
**2022 Toyota Corolla · Sep 2026 · `paper`**

DrivingBench retrofitted a Comma 4 running a modified build of openpilot onto a Corolla and handed the car to frontier models through three MCP tools: `observe`, `set_motion`, `stop_now`. Camera frames come back as tool output, the model commands steering and velocity, and the car keeps moving while the model thinks, so inference latency is part of the task.

Claude Fable 5.1, running inside Claude Code, managed 9%, 10%, then 45% of a 127 m cone course. GPT-6 Astra was the only model to finish, at 5:22 on its second attempt. Grok 4.6 peaked at 11%, GPT-5.6 Sol at 6%. A human sat with a foot over the brake the whole time.

- https://drivingbench.com/
- Paper: https://arxiv.org/html/2609.38948v1
- 404 Media: https://www.404media.co/these-tech-workers-made-chatgpt-drive-a-toyota-corolla/

## 4. Claude runs BMW ISTA+ headlessly
**BMW G20 · Sep 2026 · `repo`**

An MCP server that connects Claude (or any MCP client) to the Windows laptop sitting in your car, so the agent reads ISTA's fault memory and test plans as text, looks the problem up, explains it, and clicks through ISTA's screens. You keep the keys for anything that writes to the car.

Proven by a hobbyist on a G20: the agent read a stored charge-pressure fault, pulled BMW's own test plan as text, and walked it to the faulty component. Part replaced, fault gone on the next read. The repo ships no BMW software or data, just the bridge and the documentation.

- https://github.com/chrisbray85/headless-ista

## 5. AI decoded BMW CAN, then an ESP32 took the shifter's job
**BMW, multiple projects · 2026 · `video`**

Zero To 60 used AI to reverse-engineer the BMW shifter's CAN messages, then replaced the shifter with an ESP32. 129K to 131K views. Couch Built went further in two directions: AI reverse-engineering the CAN signals needed for a BMW i8 engine swap, and decoding BMW CAN data to build a touchscreen steering wheel with paddle shifters.

- Zero To 60: "I Used AI to Replace a BMW Shifter" (CAN bus reverse engineering + ESP32)
- Couch Built: BMW i8 swap signals, and the touchscreen steering wheel
- Case study index: https://www.csselectronics.com/pages/can-bus-sniffer-reverse-engineering

## 6. A $35 Toyota cable turned into a J2534 MCP server
**Toyota / Lexus · 3 Sep 2026 · `repo`**

`cyrusdavirusss/j2534-mcp-server` bridges an AI agent straight to J2534 PassThru. Tested on Toyota and Lexus through a Mini VCI (FTDI FT232R) cable, but adapter-agnostic. It does raw device control, ISO15765 CAN channels with ECU flow-control filters, UDS and OBD-II send and receive, DTC read and clear, VIN, live data, manufacturer-aware DTC lookup across 12,000+ codes and 33 brands, and Toyota's enhanced mode `0x21` ReadDataByLocalIdentifier for the parameters OBD-II never exposes.

The repo carries real diagnostic artifacts, not just code: `stall_log_crank.csv`, `stall_log_now.csv`, `sweep_sampler.py`, `toyota_scan.json`. Needs Windows with a 32-bit Python to load `MVCI32.dll`.

- https://github.com/cyrusdavirusss/j2534-mcp-server

## 7. AI cloned a truck's ECU off an eBay spare
**GM pickup · late 2026 · `video`**

Gearshift Garage, 526K views. The stack: Claude, Grok and Codex, a cheap SH-C31A adapter, a homemade harness, and Ghidra Pro installed via Homebrew with the agent given permission to drive it. They did a full read and write of a spare ECU bought on eBay, VIN programming included, looking for a way past the immobilizer.

The honest part is in the transcript: Claude "kind of gave up", Codex kept pushing, and roughly 15 KB of the binary could not be copied. The agent narrowed it to nine non-erased bytes in three regularly spaced slots near the end of the bank and said plainly that the purpose was not yet proven.

- "I gave AI access to my trucks computer", 526K views
- "I Let AI Tune My Truck", and the clone follow-up

## 8. Four Claude Code agents rendering on the car's dash, in a dealership
**BMW, Porsche · Oct 2026 · `claim`**

`0xCodez` claims Claude Opus 5.5 reverse-engineered CarPlay through an Android AI box (Ottocast / Carlinkit Tbox class) plugged into the car's USB port, then ran four Claude Code agents rendered live on the centre screen. Posted from a BMW dealership. A second account reports the same playbook on a Taycan at a Porsche dealer in 14 minutes.

Treat the numbers as marketing. The method is real and cheap: the head unit treats the box as a phone, so the screen becomes a normal Android surface, and the cable is the USB port you already have.

## 9. Proprietary CAN signals decoded in minutes
**Any car · Jun-Jul 2026 · `video` `repo`**

You normally spend weeks staring at hex with SavvyCAN to learn which bits carry speed, RPM or gauge position. CSS Electronics published a Claude Code skill plus CANsub.2 interface workflow that takes raw proprietary frames to a DBC file, with OBD-II values, vision OCR or a human pressing buttons as the reference signal. Their claim is 95%+ faster than the classical approach; the demo video has 308K views.

Jordans Garage did the same thing with a ~$20 DSH CAT SH-C30A USB adapter and Claude Code, 211K views, aimed at engine swaps and retrofits. The lesson repeated in both: the AI is not the hard part, the ground-truth reference signal is.

- https://github.com/CSS-Electronics/can-bus-reverse-engineering-skills
- https://www.youtube.com/watch?v=jQDqWuL5-nQ
- https://www.youtube.com/watch?v=GlQO8fLgHi4

## 10. A free macOS app that reads every BMW ECU over ENET
**BMW F/G series · 14 Nov 2026 · `repo`**

`sembsa/bmw-enet-diag` is three days old at the time of this sweep and does what should have existed for a decade: plug a cheap OBD-to-Ethernet cable into the OBD port and your Mac, switch ignition on, click connect. You get every control unit's faults with plain-language hints, live engine data judged against normal ranges, a guided engine check that separates a vacuum leak from a dirty air-mass meter, and an MCP server so your own Claude can explain the rest.

The design choice that matters: it cannot change anything in the car.

- https://github.com/sembsa/bmw-enet-diag

---

## Honorable mentions

- **CANExplorer MCP** (`akj7`), 1 Nov 2026. Prompt used: "You're connected to an unknown car's CAN bus over MCP. Find the vehicle speed signal. I have a GPS speed log. Don't transmit anything." Claude decoded it live and promoted the signal into a database entry. https://akj7.github.io
- **shadetree-ai**, read-only OBD-II MCP built for a mechanic friend. Author reports it worked "almost perfectly on a 2015 Toyota 4Runner and a 2024 Toyota Highlander". https://github.com/iamneilroberts/shadetree-ai
- **shengjia**, 47 ECU/TCU calibration skills for Claude Code and Codex: A2L/DAMOS/WinOLS parsing, Stage-1 matrices, checksum engines, blind map finder, Bosch MG1/MD1/EDC17/DENSO platforms, and a literal `toyota-pedal-tuner`. https://github.com/liushengjia-AAL/shengjia
- **zepgram/toyota-mcp**, MyToyota Europe account into Claude: fuel, range, location, trips, health, climate, charging, remote commands. https://github.com/zepgram/toyota-mcp
- **tars-car-mcp**, exposes OBD-II, UDS, GM bidirectional actuator tests and a CAN sidecar to Claude, and labels `car_clear_dtcs` as destructive in the tool description. https://github.com/RobertSloan22/tars-car-mcp
- **q50-obd-scanner**, KWP2000 + UDS + OBD-II with an embedded Claude agent behind a read-only gate and 844 tests that need no adapter. https://github.com/darblinko/q50-obd-scanner
- **car-use**, "let your agent use your car", including a built-in virtual car so you can develop without hardware. https://github.com/october-dev/car-use
- **obd2-mcp-server**, a $15 Bluetooth adapter and a broken convertible roof. https://github.com/petrpatek/obd2-mcp-server
- **open-mechanic**, OBD-II plus Claude API into plain-English diagnosis and repair steps. https://github.com/speed785/open-mechanic
- **RealOEM Searcher**, a Claude plugin with a local MCP server for BMW Group part numbers, supersession chains, VIN decoding and fitment checks. https://github.com/Cadtastic/RealOEM-Searcher
- **e30-rag**, local multimodal RAG over BMW E30 repair manuals. https://github.com/rublev/e30-rag
- **Cheap Android head unit teardown** (r/ClaudeCode, 6.7K upvotes). Claude Code found the "8 cores, Android 15" listing was 4 cores and Android 9, an always-listening wake-word assistant burning ~30% CPU in a hidden WebView, Baidu location services, a preinstalled call recorder, and a vendor firmware push channel. https://www.reddit.com/r/ClaudeCode/comments/1xf2lur/
- **Range Rover**, one prompt found the previous owner's app still linked with live location and unlock, plus a GPS tracker under the dash. `claim`
- **Iamcarhacker**, "How I replaced OBD2 scan tool with Claude" (320K views) and "AI is better than 99% Mechanics" (128K), where AI found a €300 Laguna's fault from a 20 minute CSV drive log.
- **Yotty Labs**, "Watch Claude Code hack my dad's car in 5 minutes", one prompt, live dashboard, reskinned as Knight Rider.
- **Daihatsu Mira e:S**, a Japanese writeup of a zero-hand-written-code dashboard on a USB ELM327 in about five days. https://note.com/wise_bee4925/n/n7860794ba328
- **Bitvea**, a deliberately read-only OBD-II MCP for a 2010 Ford Focus CC, written up as much about "knowing when not to let the model act" as about the AI. https://bitvea.com/en/blog/claude-mcp-car-diagnostics-obd2

---

## Patterns worth stealing

1. **Read-only first, always.** Every project that survived a public comment section enforced read-only in code, not in the prompt: `bmw-enet-diag`, `shadetree-ai`, `q50-obd-scanner`, TARS's destructive-tool labels.
2. **Backup before flash, and refuse without one.** The iDrive work will not flash until a coding backup exists. The Civic head unit saved every original file with a matching undo command.
3. **Ground truth beats model quality.** CAN reverse engineering only works when you can validate a candidate signal against OBD-II, a GPS log, an OCR read of the speedo, or a human pressing the button. No reference, no result.
4. **The cable is the moat, not the model.** ENET, J2534, USB-CAN, OBD-II: the protocols are documented enough for an agent to be useful, and the vendor lockout is what people are actually attacking.
5. **The Supra crossover.** BMW ENET is native on the A90/A91 GR Supra, which runs BMW electronics in a Toyota suit, and BimmerCode codes it as a G29 Z4. Every BMW tooling entry above works on a Supra.

## Safety and legality

Reading your own car is one thing. Flashing, writing VINs, defeating immobilizers and jailbreaking head units can brick a car, void a warranty, and in many jurisdictions breach emissions or roadworthiness law. Several projects above are explicitly head-unit-only or read-only for that reason. Nothing here is advice; it is a record of what other people did.

## How this list was made

- X: 18 search passes, `Top` and `Latest`, roughly 640 unique posts, then a strict filter for posts that mention Claude *and* a car or diagnostic term.
- Reddit: r/ClaudeCode, r/CarHacking, r/BimmerCode, r/BMW, r/ClaudeAI, r/ClaudeWorkflows, r/BmwTech.
- Hacker News: Algolia full-text plus the comment trees on the big threads.
- GitHub: repository and README inspection, star counts and creation dates.
- YouTube: watch pages and transcripts, including the DIY ECU-clone series.
- Community case-study indexes from CSS Electronics, plus vendor and forum cross-references for BMW ENET and Toyota Techstream.

Corrections welcome. If an entry is wrong, open an issue with the primary source.

## License

CC0 for the compilation and the data files. Linked material belongs to its authors.
