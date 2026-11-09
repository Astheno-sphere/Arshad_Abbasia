# Tool Map — Astheno-sphere, clubbed into systems
Verified 2026-11-10. Sources: `offshore_simulations/catalog/tools.yaml`, `computational_Logistics/knowledge-bank/catalog/skills.json` (3,047 entries) + `mcp-servers.json` (84), `requirements*.txt` in both repos, local `~/workspace/venvs/`, `~/workspace/custom-connectors/`, `~/workspace/custom-skills/`, and the repos themselves. See `~/workspace/github-inventory-2026-11-10.md` for the 7 repos.

## Honesty first — what "we have" actually means
- Offshore catalog: **154 tools listed, 145 parsed cleanly. Tested 25 + wired 13 + installed 8 = 46 actually used. Candidate 52 + watch 43 = 95 listed-only, not yet played with.** Do not present the 154 as "my stack" — present the 46, and the rest as "scouted".
- Knowledge-bank catalogs are **harvested**, not used: skills.json 3,047 entries (davila7 claude-code-templates 1,448; wshobson agents 584; Abhinav only 130), mcp-servers.json 84 entries (harvested location services 41). They are a library to search, not a stack.
- **Anaconda: NOT found on this VM.** No `conda`. Python 3.12.3 + plain venvs (13 venvs: venv-studio, venv-geo, venv-rhino, venv-kepler, moon, urdu-ocr, venv-reelmimic, venv-scout, venv-tdc, venv-weasy, venv-finish, blender-4.2.23, jdk). If Anaconda lives on your laptop, it is not in the evidence here — say so, don't claim it.
- VS Code: no binary verified on this VM. Your verified CLI layer is: `gh` 2.102.0, git, Python 3.12, Node 24.22.1, FFmpeg 8.1.2, npm. VS Code is plausible on your laptop, unverified here.
- Rhino/Grasshopper live drive is **not yet done**: standalone rhino3dm works (244 tools), the bridge plugin + Tailscale IP are still pending your approval. Grasshopper Hops components exist in code (`servers/hops_app.py`, ghhops-server 1.5.5, rhino3dm 8.35.0) — live Grasshopper runs are not yet verified.

## System 1 — WORLD / MAPS / GIS (the ground everything runs on)
Tested/wired: OpenStreetMap + Overpass POST (kumi mirror, 300 s timeout — bare GET 406 is not "dead"), Geofabrik Norway PBF, pyrosm, OSMnx, pyproj, shapely, geopandas, NetworkX, scikit-image, pydelatin, Kartverket stedsnavn API, Sodir FactMaps (layer 304, 376 facilities), EMODnet depth, BBBike Oslo extract (104.8 MB GeoParquet).
Scouted (candidate/watch): Kartverket DTM 10 m, seabed DTM 5/25/50 m, GEBCO, Copernicus DEM GLO-30, PMTiles, Protomaps, Planetiler, Overture Maps, QGIS MCP.
Local proof: Molde bbox 1,568 buildings; Oslo Sentrum 5,141 buildings / 9,644 roads; Bergen world 35,968 coastline ways → 15,417 faces, 361,428 buildings, 32,411 roads.
Renderers: Kepler.gl, deck.gl + MapLibre, lonboard (watch).

## System 2 — TRAFFIC / AIS / LIVE NORWAY
Wired/tested: Entur GTFS (national, NLOD-2.0), Entur Journey Planner API, Entur Vehicle Positions API, MovingPandas, GTFS Kit, r5py, Datashader.
Manual: Kystverket Historic AIS (HAIS) — order/download by hand.
Scouted: pyais, AISdb, BarentsWatch AIS (wired in catalog, not yet run here), Danish AIS datasets.
Live proof: Molde GTFS 41 routes / 2,323 trips; 289 trips, 5,963 km, peak 67 at 07:35 in Molde morning v2.

## System 3 — WEATHER / SEA / METOCEAN
Used: NORA3 wind+wave hindcast (one winter Dec 2024–Feb 2025, 2,160 h, wind max 30.9 m/s, Hs 0.02–11.74 m, mean 2.63 m), NorKyst v3 800 m, xarray + netCDF4.
Scouted: MET Norway metocean-api / oceanforecast / WAM 800 m, Copernicus Marine Toolbox, ERA5, OpenDrift, cfgrib/herbie.
Note: "24 winters" is NOT done — one winter only, ~8–9 min per winter to extend.

## System 4 — SOLVE / OPTIMISE
Used: AMPL (amplpy, academic licence — Gurobi/CPLEX/Xpress/COPT/Mosek 12 mo, cloud lease needs academic IP; this VM is not one) + HiGHS/CBC allowlisted, Pyomo + HiGHS, PyVRP, Biogeme 3.3.5 (DCM, Swissmetro LL −5214.9, N=6,768), EMA Workbench 3.0 + PRIM, OR-Tools 9.15, highspy.
Scouted: ALNS, VROOM, Timefold, jsprit, RL4CO.
MCP: `offshore_simulations/tools/ampl_mcp/server.py` — 5 tools (solve_model, get_variable, get_duals, run_ampl, list_models), verified loads under mcp 2.3.0. Actions route: `.github/workflows/solve.yml`, secret AMPL_LICENSE_UUID (never in chat/file/commit).

## System 5 — SIMULATE
Used: SimPy, openclsim 1.6.3, WOMBAT 0.13.5 + ORBIT 1.3.1 (NREL offshore wind), Taichi 1.7.4, PhiFlow 3.4.0, PyVista, trimesh.
Scouted: Mesa, OpenTNSim, Capytaine (GPL-3.0), salabim, Fossen PythonVehicleSimulator, port-simulation.
Gates (offshore_simulations): contract.validate + corrector gates 0–4; no render until gates pass; straight-line sea legs never acceptable — tracks follow real OSM ferry ways.
Reproductions: Nafstad 2021 trip-based PASSES 0.00% on 6 instances; Ji/Liu/Yu 2026 FAILS (8-order 0/5, gaps 94–97%, headline −43.33% reproduces as −0.014%) — do not build on it. Next: Gribkovskaia & Slettemark 2025, lean 4-heli vs actual 5-heli through NORA3 winters, pre-registered P90 unmet ≤ 1/week.

## System 6 — DESIGN / CAD / PHYSICS (the Abhinav spine)
Rhino: rhino3dm-mcp v0.6.0, venv-rhino, client `custom-connectors/custom.rhino/bin/client.py`, **244 tools verified standalone**, one server per document, args wrapped `{"args":{...}}`. Bridge: plugin `rhino-bridge/rhino-mcp.rhp`, loopback-only, no auth — never expose; needs Tailscale join + plugin on the Rhino machine + its Tailscale IP.
Grasshopper: Hops app `computational_Logistics/servers/hops_app.py` — /cl/route, /cl/vrp etc. via ghhops-server; Ladybug / Honeybee / Butterfly→OpenFOAM, Kangaroo 2 form-finding, Karamba3D (from Abhinav DNA measurement, not yet run live here).
Blender: 4.2 headless + bpy 5.2 smoke tests PASS; BlenderGIS candidate; Freestyle+SVG candidate.
CAD in Python: CadQuery 2.8.0 + build123d 0.12.0 + cadquery-ocp, trimesh, PyVista.
OpenFOAM 13 smoke test PASS (2D buoyant NS at reduced fidelity, Landsat anchor) — full CFD is not yet run here.
Design skills (local): abhinav-poster-style (1,246 lines, 135 posters reverse-engineered), anatomy (isometric SVG figures), canvas-design, god-tier-designer (NGHS-), whipster_visual_gate, teardown.

## System 7 — RENDER / VIDEO / NARRATIVE
Used/tested: skia-python 144.1 (current renderer, v7/v10), FFmpeg 8.1.2, Remotion, Manim, HyperFrames, GSAP, Matplotlib, deck.gl, Pillow, reportlab.
Films (real, in `~/workspace/docs/linkedin/`): ferry-multiplier v2-final + v5, Nafstad trip-based v2, Oslofjord gale v10 (24 s, 1080×1350, 720 frames, 13.1 MB, whipster PASS, your verdict "its better"), Molde morning v2 (24 s, 289 trips), Oslo flood A/B/C v5, START Molde Painted Scenic Drift (30 s, 1080×1920).
Rules: no colour legend, denotation by naming; numbers from the gated run only; assumptions printed on screen.

## System 8 — AGENT / CLAUDE / MCP / CODE
Claude Code plugins (2, in-repo): `computational_Logistics/.claude-plugin` (Asthenosphere v0.7.0) + `offshore_simulations/.claude-plugin` (v0.1.0). MCP servers in-repo: asthenosphere MCP (`servers/mcp_server.py`, mcp 2.2.0), AMPL MCP (above), Rhino MCP (above).
Skills in-repo: computational_Logistics 11 skills, offshore_simulations 9 skills (see github-inventory). Local skills: `~/workspace/custom-skills/` 16 (abhinav-all-skills 81 skills, karpathy-guidelines, linkedin_voice_gate, opengeos-mapping, reelmimic, rhino-mcp, verify_citations_primary_source, verify_exact_label_figures, headless_render_integrity, instrument-design, etc.).
CI / infra: GitHub Actions (tests, fetch-world, solve, papers), gh CLI 2.102.0 (not authenticated — use the GitHub connector, not `gh auth` with a pasted token), git, Node 24, Python venvs.
Connectors available in Muse (79 system + custom.rhino): GitHub, Gmail, Google Calendar/Docs/Drive/Sheets, Figma, Canva, Duffel, Instagram, etc. — list is in the connector catalog, not all used.

## System 9 — DATA / VERIFICATION / THESIS
Z3 (bounded check), AES-256-GCM crypto-erasure, Merkle log, Streamlit (TRF live demo), Synthea synthetic health data, FHIR, pytest (offshore suite 393 passed / 3 skipped at v10; Molde suite 379 passed), checksums + CHANGE_TRAIL in TRF-thesis.
Urdu/OCR (local, not on GitHub): Tesseract 5.3.4 + EasyOCR 1.7.2 + TrOCR, venv urdu-ocr; measured CER Nastaliq EasyOCR 26.3% best; wafq grid 0 digits right — human transcription + wafq_check is the route.
Moon (local): pyswisseph ephemeris verified, 28 mansions dataset, orrery (Schlyter moon worst err 0.021° over 5 yrs).

## System 10 — JOB / CV / VOICE
norsk-job-hunt-: ANIMA v5.0, outreach engine, red-team agent, CV HTML (master + trainee, Schibsted Grotesk, oxblood #7a1c2a, 1-page A4), applications log.
Local CV: cv-abhinav/ Documenting Journey v6 (5 pp, 24 photos, 3 Molde plates), cv-content.md, cv-abhinav-spec.md.
Voice: linkedin_voice_gate (first voice, short posts, plain words, no paper language), cross-system post (Norwegian, "En optimal rute er ikke nødvendigvis en god offshoreoperasjon" — nothing posted yet).

## Visual assets for the profile (real, verified)
- `profile-readme/assets/molde-morning-v2-still.jpg` (1080×1350, frame at 12 s of 24 s film)
- `profile-readme/assets/gale-v10-still.jpg` (1080×1350, frame at 12 s)
- `profile-readme/assets/ferry-multiplier-still.jpg` (1080×1350, frame at 12 s)
- `profile-readme/assets/molde-pinch-points.jpg` / `molde-vrp-loop.jpg` / `molde-depot.jpg` (1440×990, graded plates from cv-abhinav/plates/)
- PhD plate: `phd-proposal/plates/p5-robust-front-v1.html` — "robust is not optimal", 1,000 agents · 60 futures · 25 packages · 1,500 runs (HTML, link it — do not screenshot-claim it as an image)
- Films live at: `docs/linkedin/03-oslofjord-demo/v10/oslofjord-demo-hq-v10.mp4`, `docs/linkedin/05-molde/v2/molde-morning-v2.mp4`, `docs/linkedin/01-ferry/ferry-multiplier-v2-final.mp4`
