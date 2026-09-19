<div align="center">

# VulnClaw 🦞

> *AI-powered penetration testing CLI tool — bas natural language me bolo, vulns khud hi test ho jayengi.*

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python 3.10+](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/)
[![OpenAI Compatible](https://img.shields.io/badge/API-OpenAI_Compatible-green)](https://platform.openai.com/)
[![MCP](https://img.shields.io/badge/Toolchain-MCP-orange)](https://modelcontextprotocol.io/)
[![PyPI](https://img.shields.io/badge/PyPI-v0.4.0-blueviolet)](https://pypi.org/project/vulnclaw/)
[![codecov](https://codecov.io/gh/Netw0rkNoob/VulnClaw/branch/main/graph/badge.svg)](https://codecov.io/gh/Netw0rkNoob/VulnClaw)
[![Security](https://img.shields.io/badge/Scope-Authorized_Only-red)](#security-disclaimer)
[![Discord](https://img.shields.io/badge/Discord-Join_Community-5865F2?logo=discord&logoColor=white)](https://discord.gg/q5nrZpe6S)
[![AtomGitStars](https://atomgit.com/Unclecheng-li/VulnClaw/star/badge.svg)](https://atomgit.com/Unclecheng-li/VulnClaw)
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://kimi-file.moonshot.cn/prod-chat-kimi/kfs/4/1/2026-06-05/1d8h69mt3v89kkekg24gg">
  <img alt="Kimi Open Source Friends" src="https://kimi-file.moonshot.cn/prod-chat-kimi/kfs/4/1/2026-06-05/1d8h69fudcmosb3pipls0">
</picture>
<br>

🌐 **English version**: [`README_EN.md`](README_EN.md)

**Ye project ek standalone AI penetration testing Agent hai.**
<br>
Project ki official website: https://unclecheng-li.github.io/vulnclaw.com/
<br>

LLM Agent + MCP toolchain + optional Skill reference material par based,
OpenAI / Anthropic / MiniMax / DeepSeek jaise compatible models ke saath —
natural language input → poora flow automatically complete: 「info collection → vuln discovery → exploitation → report generation」.

[Quick start](#quick-start) · [Architecture](#architecture) · [Built-in Skills](#built-in-skills)

</div>

---

## Ye kya kar sakta hai

Natural language me input karo, AI poora penetration test flow khud-ba-khud execute karega:

```
User input: http://target.example.com ka penetration test kar do

VulnClaw automatically execute karega:
  Round 1:  Info collection → fingerprinting, port scanning, directory enumeration
  Round 2:  Vuln discovery → injection points, known CVEs, config flaws detect karna
  Round 3:  Exploitation → PoC verification, access gain karna
  Round 4:  Report generation → structured report + Python PoC script
```

<img width="1148" height="642" alt="image" src="https://github.com/user-attachments/assets/576e1cf6-25da-4969-864b-40e77d020dbf" />

<img width="2521" height="1300" alt="image" src="https://github.com/user-attachments/assets/9f8d62c1-8e19-4b25-a2c9-651338329e88" />

Authorized penetration tests, CTF competitions, security teaching aur red-team drills jaise scenarios ke liye suitable.

---

## Features

- **Model-led solving engine (default)** — Claude Code/Codex jaisa autonomous loop: model khud decide karta hai ki agla step kya hai, tool kab call karna hai, kab finish karna hai / kab user se poochhna hai / kab no-path declare karna hai
- **AgentState evidence memory** — saare tool results `AgentState.evidence` me write hote hain, raw output poora preserve rehta hai; active context me by default sirf high-signal preview inject hota hai, aur original evidence on-demand wapas dekhne ke liye `evidence_search` / `evidence_view` hain
- **Lightweight correction layer** — tool call se pehle aur baad me repeat calls, failure fallback, timing aur naye findings jaise signals record hote hain; same evidence range ko baar-baar padhna suppress hota hai, aur continuous evidence idle-spinning par stall guard trigger hota hai — lekin purana stage planner wapas nahi aata
- **Evidence-level anti-hallucination gate** — claim kiya gaya flag/conclusion tabhi accept hota hai jab wo real tool output me character-by-character mile; isse bina basis ke flag ghadne wali jhoothi jeet poori tarah rukti hai
- **Natural language driven** — pentest ka intent normal bhasha me likho, stages aur tools automatically identify ho jate hain
- **14 LLM Providers** — OpenAI / Anthropic / MiniMax / DeepSeek / Zhipu / Moonshot / Qwen / SiliconFlow / Doubao / Baichuan / StepFun / SenseTime / 01.AI / local Ollama, ek command me switch
- **MCP toolchain** — 4 MCP services: `fetch` / `memory` local implementations out-of-the-box, aur `chrome-devtools` / `burp` external MCP services se browser automation aur HTTP capture-replay ke liye
- **Enhanced fetch request tool** — default me direct GET karke poora response body return karta hai; HTTP/HTTPS, custom method/headers/params/cookies/body/data/form/json, timeout/redirect/TLS control supported; CTF/range HTTPS par by default certificate verify nahi hota
- **Native traffic evidence storage** — run-scope filtering ke baad append-style JSONL index + har request ka raw packet `evidence/traffic/` me save hota hai; built-in `traffic_list` / `traffic_view` / `traffic_repeat` / `traffic_sitemap` tools seedha read-write karte hain
- **AI Agent core** — OpenAI compatible protocol + Tool Calling + autonomous pentest loop
- **Structured reasoning + adaptive reflection** — known facts/constraints/attack chains structured tarike se store hote hain; failures automatically categorize hokar L0-L4 gradual payload bypass strategies me escalate hote hain
- **Vuln detection plugin system** — low-coupling plugin runtime + built-in read-only Web plugins, results automatically report pipeline me merge hote hain (`vulnclaw plugins`)
- **50 specialized Skills** — CTF, Web, intranet, reverse engineering, vuln verification aur authorized red-team knowledge base cover karte hain; Skills model ko sirf reference index ke roop me dikhte hain, asli content model ko khud `load_skill_reference` call karke on-demand padhna hota hai, zabardasti context me inject nahi hota
- **Encode/decode & crypto tools** — 29 operations (Base64/Hex/URL/AES/JWT/Morse etc.), LLM precisely call kar sakta hai, ab andaza lagane ki zaroorat nahi
- **Automatic source code restoration** — jab `fetch` / `http_probe_batch` ko `highlight_file`, HTML-highlighted source ya mixed HTML/JS body milti hai, to raw body se pehle clean source append ho jata hai; `http_probe_batch` by default TLS verification off rakhta hai aur poore response headers record karta hai taaki `X-Powered-By` jaisi runtime evidence na chhoote; built-in `source_extract` se purani evidence on-demand dobara padhi ja sakti hai, aur dangerous sinks, forms aur endpoint signals pin rehte hain
- **Local command verification** — built-in `shell_command`, jo `php -r` deserialization verify, `curl` precise requests, `rg`/`Select-String` file search jaise Codex-style local debugging scenarios ke liye hai; raw stdout/stderr poora evidence me write hota hai, bada output model tak high-signal preview ke roop me jata hai
- **Runtime differential probing** — built-in `runtime_diff_probe`, regex/string filters aur runtime parser ke bechas inconsistency wale scenarios ke liye; model ko batch me "filter se chhootne wale, parser ke accepted" candidates generate aur verify karne me madad karta hai; PHP serialization mode target/local runtime version difference batata hai aur PHP5 signed length candidates ko remote-verification-required mark karta hai, taaki local naya PHP unhe galat na maare
- **Python code execution** — built-in `python_execute` tool, payload construction aur response parsing ke liye perfect; abhi bhi high-risk experimental capability hai, ise strong isolated sandbox mat samjho
- **Batch HTTP probing** — built-in `http_probe_batch`, ek saath kai URL/params/header/body/raw URL variants compare karne ke liye; by default har response ka poora body return karta hai aur model-visible output me actual request surface (method, URL, params, headers, cookies, body/json) dikhata hai, jisse repeat LLM rounds aur hand-written request code kam hota hai
- **Near-success anti-stop** — solve me evidence gate bana rehta hai aur naya generic `NO_PATH` gate add hua hai: jab source code sinks, forms/params, request surface, local proof ya response diffs jaise high-signal anchors abhi khatam nahi hue hain, to model ko single payload me koi echo na milne ya remote same-body response par jaldi haar maanne nahi diya jata
- **Continuous pentesting** — periodic loops (default 100 rounds/cycle × 10 cycles = 1000 rounds), har cycle ke baad automatic report
- **Reasoning display control** — `think on/off` se LLM ka thinking process dikhana/chhupana ek toggle me
- **Sandbox mode prompt** — AI security testing capabilities unlock karta hai, sirf CTF / authorized pentest scenarios ke liye
- **Auto reports & PoC** — structured Markdown report aur runnable Python PoC script generate hoti hai
- **Web UI mode** — `vulnclaw web` se local web interface chalta hai, default `127.0.0.1:7788`
- **Security knowledge base** — knowledge base module aur basic seed data built-in hai, retrieval augmentation dheere-dheere main flow me aa rahi hai

---

## Quick start

### Install

```bash
# PyPI se install (recommended)
pip install vulnclaw

# Source se install
git clone https://github.com/Netw0rkNoob/VulnClaw.git
cd VulnClaw
pip install -e .
```

### Docker se run (optional)

Image me Web UI aur default MCP services ke liye zaroori runtimes (`npx` / `uvx`) pehle se built-in hain, saara state `/data` volume me persist hota hai.

```bash
cp .env.example .env          # VULNCLAW_LLM_API_KEY etc. bharo
docker compose up --build      # image build karke Web UI start karo
# http://127.0.0.1:7788 kholo
```

Pure docker se koi ek CLI command bhi chala sakte ho:

```bash
docker run --rm -it \
  -e VULNCLAW_LLM_API_KEY=sk-your-key-here \
  -v vulnclaw-data:/data \
  vulnclaw:latest scan <target>
```

> ⚠️ Container ke andar `localhost` ka matlab container khud hota hai. Host machine ki services scan karne ke liye `host.docker.internal` use karo, aur doosre containers scan karne ke liye network share karke container name se access karo. Details ke liye [DOCKER.md](DOCKER.md) dekho.

### Char steps me start

```bash
# 1. Provider chuno (Base URL aur model name auto-fill ho jayega)
vulnclaw config provider minimax   (ya openai/anthropic/deepseek/zhipu/moonshot/qwen/siliconflow/ollama)

# 1.2 (optional) Base URL ya model name customize karo
vulnclaw config set llm.base_url https://your-own-api.example.com/v1 
vulnclaw config set llm.model your-model-name

# 2. API Key set karo
vulnclaw config set llm.api_key sk-your-key-here
#    — ya ChatGPT subscription login use karo (API Key ki zaroorat nahi):
#      vulnclaw login   (browser login; ToS risk ka dhyan rakho)

# 3. Default: original CLI / REPL kholo
vulnclaw

# 4. Optional: TUI workbench kholo
vulnclaw tui
```

### Environment check

```bash
vulnclaw doctor
```

Output example:

```
🦞 VulnClaw environment check

  Python: 3.14.4
  Node.js: v24.14.1
  npx: installed
  nmap: installed

LLM config:
  Provider: openai
  Auth Mode: static
  Credentials: configured
  Base URL: https://api.openai.com/v1
  Model: gpt-4o

MCP services:
  fetch: enabled [P0]
  memory: enabled [P0]
  ...

✅ Environment ready, vulnclaw chala ke shuru karo
```

---

## CLI commands quick reference

```bash
$ vulnclaw --help

🦞 VulnClaw — AI-powered penetration testing CLI

 Usage: vulnclaw [OPTIONS] COMMAND [ARGS]...

 Commands:
   run           🚀 Ek command me poora pentest flow (default solve engine)
   solve         🧩 Target-driven solving (model-led, fixed rounds nahi)
   persistent    🔄 Continuous pentest (100 rounds/cycle)
   recon         🔍 Sirf info collection stage
   scan          🔎 Vuln scanning stage execute karo
   exploit       💥 Exploitation stage execute karo
   report        📝 Session records se report banao
   repl          💬 Classic REPL interactive interface kholo
   config        ⚙️  Config manage karo (set/get/list/provider)
   plugins       🧩 Vuln detection plugins manage karo (list/info/run)
   init          🔧 Config initialize karo
   doctor        🏥  Runtime environment check karo
   tui           🖥️ Terminal graphical workbench kholo
   web           🌐 Local Web UI start karo
   code          🧬 Local source code security scan (network target ki zaroorat nahi)
```

| Command | Kya karta hai | Example |
|------|------|------|
| `vulnclaw` | Default original CLI / REPL kholta hai | `vulnclaw` |
| `vulnclaw tui` | Terminal graphical workbench | `vulnclaw tui --target target.com` |
| `vulnclaw repl` | Classic REPL interactive interface | `vulnclaw repl` |
| `vulnclaw solve <target>` | Target-driven solving (fixed rounds nahi, goal milte hi rukta hai) | `vulnclaw solve target.com --goal "flag lao"` |
| `vulnclaw run <target>` | Ek command me poora pentest (default solve engine) | `vulnclaw run 192.168.1.1` |
| `vulnclaw persistent <target>` | Continuous pentest (100 rounds/cycle) | `vulnclaw persistent 192.168.1.1` |
| `vulnclaw recon <target>` | Sirf info collection (exploit nahi karta) | `vulnclaw recon target.com` |
| `vulnclaw scan <target>` | Vuln scanning stage | `vulnclaw scan target.com --ports 80,443` |
| `vulnclaw exploit <target>` | Exploitation stage | `vulnclaw exploit target.com --cve CVE-2024-1234` |
| `vulnclaw report <session>` | Session JSON se report banao | `vulnclaw report session_xxx.json` |
| `vulnclaw config set <key> <value>` | Config item set karo | `vulnclaw config set llm.api_key sk-xxx` |
| `vulnclaw config provider <name>` | LLM provider switch karo | `vulnclaw config provider minimax` |
| `vulnclaw plugins list` | Vuln detection plugins list karo | `vulnclaw plugins list --stage discovery` |
| `vulnclaw plugins info <id>` | Plugin ki metadata dekho | `vulnclaw plugins info builtin.web.headers` |
| `vulnclaw plugins run <id>` | Plugin chalao (sirf diya gaya data analyze karta hai) | `vulnclaw plugins run builtin.web.headers --input headers.json` |
| `vulnclaw code scan <path>` | Local source code security scan (L1 regex / L2 structural / L3 LLM optional) | `vulnclaw code scan ./src --format sarif` |

---

## Use karne ke tareeke

### Tareeka 1: CLI / REPL (default)

```bash
vulnclaw
```

Bina arguments start karne par 🦞 interactive interface khulta hai, natural language me baat karo:

```
🦞 vulnclaw> 192.168.1.100 ka penetration test karo, ye meri authorized range hai

[*] Autonomous pentest mode me ja rahe ho, Ctrl+C se kabhi bhi rok sakte ho
── Round 1 ──
  [+] Target: 192.168.1.100
  [+] Open ports: 22, 80, 443, 8080
  [+] Web fingerprint: Apache/2.4.62
── Round 2 ──
  [+] /manager/html mila (Tomcat Manager)
  [+] CVE-202X-XXXX hit: Apache Tomcat auth bypass
── Round 3 ──
  [+] Vuln verification successful

🦞 192.168.1.100 | report> pentest report banao
[+] Report save ho gayi: ./reports/192.168.1.100_20260418.md
[+] PoC script save ho gayi: ./pocs/CVE-202X-XXXX.py
```

**REPL built-in commands:**

| Command | Kya karta hai |
|------|------|
| `target <host>` | Pentest target set karo |
| `status` | Current status dekho |
| `tools` | Abhi available MCP tools list karo |
| `think on/off` | Reasoning process display toggle karo |
| `mode [mode]` | Execution approval mode dekho ya badlo (ask / auto_review / full_access) |
| `persistent` | Continuous pentest start karo |
| `clear` | Current session clear karo |
| `help` | Help info dikhao |
| `exit` / `quit` / `q` | Bahar niklo |

**Auto pentest trigger:** jab input me 「pentest」, 「flag dhundo」, 「brute force」 jaise keywords + target address hote hain, to automatic multi-round autonomous pentest loop shuru ho jata hai. `Ctrl+C` se kabhi bhi rok sakte ho.

### Tareeka 2: TUI workbench

Optional terminal graphical workbench jo authorized targets, check mode, run overview aur security boundaries dikhata hai — taaki user task start karne se pehle scope confirm kar le.

```bash
vulnclaw tui
vulnclaw tui --target https://target.example --mode quick --only-port 443
vulnclaw tui --dry-run --target https://target.example --mode deep --only-path /admin
```

Rust TUI workbench configurable container layout support karta hai: left side me default status, right side me discoveries aur sub-agents saath-saath; beech me agent output, neeche input box. Sub-agent view abhi ke liye "no data" message dikhata hai.

- View titles ko drag karke do sidebars ke beech move karo ya usi column me reorder karo; orange preview box batata hai ki chhodne par module kahan aur kitna bada hoga. Title ke left me `v` / `>` click karke collapse/expand karo.
- Containers ya views ke beech ki divider drag karke resize karo; collapsed views ko pehle expand karo phir height badhao. Drag ke dauran Esc dabao to cancel.
- Scroll wheel se jis view ke upar mouse hai uska content scroll karo; click karke focus ke baad arrow keys se scroll, `Ctrl+←/→` se view switch, `Ctrl+Y` se current view copy.
- Auxiliary sidebar khaali hote hi auto-collapse ho jata hai aur beech wala area poori bachhi space le leta hai. Module ko workspace ke right edge par drag karne par orange docking preview dikhta hai, chhodne par wo dobara expand hota hai; main sidebar me hamesha kam se kam ek module rehta hai.
- Layout automatically local `VULNCLAW_HOME/tui/layout.json` (default `~/.vulnclaw/tui/layout.json`) me save hota hai aur agli baar start par restore ho jata hai. Bottom bar command panel ke saath auto-grow hoti hai; terminal bahut chhota hone par required size ka hint milta hai, bada karne par layout wapas aa jata hai.

Mouse capture on hone par terminal ki native text selection terminal ke modifier keys par depend karti hai; single view copy ke liye `Ctrl+Y` use karo.

Common menus:
- **Menu 3** — test scope set karo (hosts/ports/paths/allowed actions/forbidden actions)
- **Menu 7** — environment diagnostics entry (poori details ke liye `vulnclaw doctor`)
- **Menu 8** — model/API config (Provider, Base URL, Model, API Key switch)

### Tareeka 3: Single-command mode

```bash
vulnclaw run 192.168.1.100                    # ek command me poora flow
vulnclaw recon 192.168.1.100                   # sirf info collection
vulnclaw scan 192.168.1.100 --ports 80,443     # vuln scanning
vulnclaw exploit 192.168.1.100 --cve CVE-2024-1234 --cmd id  # exploitation
vulnclaw report session.json                   # report banao
```

### Tareeka 4: Continuous pentest

Lambe aur gehre pentest scenarios ke liye, **periodic cycles** me chalta hai:

```
┌──────────────────────────────────────────────┐
│  Cycle 1 (100 rounds) → auto report → continue │
│  Cycle 2 (100 rounds) → auto report → continue │
│  ...                                         │
│  Jab tak Ctrl+C ya max cycles (default 10)   │
└──────────────────────────────────────────────┘
```

```bash
vulnclaw persistent 192.168.1.100              # default 100 rounds/cycle × 10 cycles
vulnclaw persistent 192.168.1.100 -r 200 -c 5  # 200 rounds/cycle × 5 cycles
vulnclaw persistent 192.168.1.100 --no-report   # auto report nahi banegi

# TUI tareeka
vulnclaw tui --target 192.168.1.100 --mode continuous

# REPL tareeka
🦞 vulnclaw> persistent 192.168.1.100
```

**Khaasiyatein:** cross-cycle state persistence / periodic reports / flexible interruption / incremental discoveries / fully configurable

### Tareeka 5: Web UI

Poora pentest flow browser se operate karo.

```bash
git clone https://github.com/Netw0rkNoob/VulnClaw.git
cd VulnClaw
pip install -e '.[web]'       # source checkout se Web dependencies install karo

# Pehli baar: React frontend build karo (Node.js 18+ chahiye)
cd frontend
npm install
npm run build
cd ..

vulnclaw web                  # start (default 127.0.0.1:7788)
vulnclaw web --port 8080      # custom port
```

PyPI wheel me React build artifacts ya frontend source nahi hota; poori Web UI ke liye upar wale source tareeke se install karo.
Agar browser me **Fallback Web Shell** dikhe (poora scan interface nahi), iska matlab `frontend/dist/index.html` missing hai. Upar ke tareeke se build karke `vulnclaw web` restart karo aur force refresh karo.
Frontend build na hone par bhi `/api/health` jaise API kaam karte rehte hain.

> ⚠️ Default me sirf local loopback address par bind hota hai. Remote access chahiye to explicitly `--host 0.0.0.0 --allow-remote` dena hoga.

---

## Architecture

### Solve engine

VulnClaw by default **model-led solve engine** use karta hai (purana fixed-rounds engine chahiye to `vulnclaw config set session.engine rounds` se wapas ja sakte ho).

**Model-led loop:** framework ab task ko fixed "research directions" me nahi todta, aur na hi stage templates ke hisaab se directory scanning, JS collection ya SQLi tests schedule karta hai. Solve model ko sirf target, history context, evidence memory aur available tools ki list deta hai — agla action kya lena hai ye model khud decide karta hai.

| Primitive | Matlab |
|------|------|
| **Model context** | Target, user constraints, recent messages, evidence summary aur already executed tool calls |
| **Tools list** | `fetch` / browser / directory enumeration / JS collection / encode-decode / Python / `shell_command` / `source_extract` / `runtime_diff_probe` / skill reading jaisi capabilities — model ko sirf optional hands-feet ke roop me di jati hain |
| **Tool transcript** | Tool call ke baad assistant ke `tool_calls` aur `role=tool` observations model context me append hote hain; bade outputs high-signal preview ke roop me jate hain taaki HTML/body/logs baar-baar active context pollute na karein |
| **AgentState evidence** | Har asli tool result `AgentState.evidence` me write hota hai; raw text poora preserve hota hai hash/size/evidence-number ke saath, model `evidence_search` / `evidence_view` se on-demand wapas dekh sakta hai |
| **High-signal memory** | Source code SQL, HTML forms/input, PHP/API links, JavaScript endpoints, `highlight_file` source, `unserialize`/magic methods/dangerous sinks permanently visible facts ke roop me pin hote hain, taaki aage ki probing me asli entry points na kho jayein |
| **Lightweight correction layer** | Sirf tool lifecycle observe karta hai — timing, failure fallback, repeat calls, high-signal target facts aur chhote semantic diffs record karke agle round ke context me hint signals inject karta hai; stage planning nahi karta, tools schedule nahi karta |
| **Evidence gate** | `FINAL:` conclusion ko asli evidence reference ya hit karna zaroori hai; tool output se unsupported flag/conclusion reject hokar exploration jaari rakhta hai |
| **Auto review report** | Target achieve hote hi Markdown report auto-generate hoti hai — solving approach, key evidence, reproduction request packets, curl, response snippets aur evidence index ke saath |

```
MODEL DECIDES → optional tool call → AgentState poora raw evidence record karta hai + active context me high-signal preview + lightweight correction signals
        │
Reasoning jaari / aur tools call / ASK_USER / NO_PATH / FINAL
        │
FINAL evidence gate se verify hota hai → pass to khatam, warna reject reason model ko wapas milta hai aur wo kaam jaari rakhta hai
```

**Context strategy:** solve by default normal conversation history rakhta hai, proactive compression nahi karta. Compression tabhi hota hai jab model context limit ke paas pahuche, user `/compact` chalaye, ya auto-compression explicitly enable ho. Tool outputs poore `AgentState.evidence` me write hote hain, lekin bada output active context me jata hai to sirf bounded high-signal preview inject hota hai — jisme status, response headers/request surface, forms/params, endpoints, source sinks/filters, flag-like tokens, key line numbers, raw size aur hash shaamil hain; poora body/stdout/stderr `evidence_search` se search ya `evidence_view` se paginated dekha ja sakta hai. `fetch` / `http_probe_batch` ke `max_body_chars`, `python_execute_max_output_chars` aur `shell_command.max_output_chars` tabhi raw output tool-layer par trim karte hain jab explicitly positive set hon; trim na hone par raw evidence poori tarah preserved rehti hai. Same raw output dobara aane par active context me sirf `same_as=eXXX` reference rehta hai, body repeat nahi hoti. Terminal echo sirf humans ke liye display layer hai: lambe tool results by default preview me collapse hote hain, poora content evidence me safe rehta hai. Raw output se nikale gaye high-signal facts alag se pin hote hain — forms, params, JS endpoints, PHP/API links, `highlight_file` source, dangerous sinks, request surface, same-body/response diffs aur local proof snippets shaamil hain. `evidence_list` / `evidence_search` / `evidence_view` purani evidence dekhne ke liye hain; same evidence coverage ko baar-baar dekhna short-circuit hota hai, aur lagataar kai rounds sirf evidence palatne par (koi nayi evidence ke bina) stall guard trigger hota hai jo agle step me non-evidence tool, `FINAL`, `ASK_USER` ya `NO_PATH` use karne ko kehta hai. Tool call execute hone ke baad zabardasti ek extra `Summarizing...` LLM call nahi hoti; normal path Chat Completions ka native tool transcript (assistant `tool_calls` + `role=tool`) use karta hai, taaki agla model sampling asli observations par based rahe.

**Evidence-level anti-hallucination gate:** saare asli tool outputs ko only trusted evidence ke roop me record karta hai. Claim kiya gaya flag/conclusion tabhi accept hota hai jab wo asli output me character-by-character mile ya evidence number explicitly reference kare — isse manhgadhant claims poori tarah rukte hain. `NO_PATH` bhi near-success gate ke under aata hai: jab evidence me abhi bhi high-signal anchors bache hote hain, to seedha rukne ke bajaye reject reason model ko wapas milta hai taaki wo verification jaari rakhe.

**Auto review report:** solve me target achieve hone ke baad `AgentState` se deterministically Markdown review report generate hoti hai aur by default terminal par print hoti hai. Report ke liye koi extra LLM request nahi hoti; reproduction request packets, curl aur response snippets asli `fetch` / `http_probe_batch` evidence se aate hain.

**Authorized red-team Skills:** `codex-redteam-mode` ke authorized red-team detail packs skill/knowledge-base ke roop me import kiye gaye hain taaki model on-demand padh sake; jailbreak, refusal-bypass aur session-patch jaisi limit-breaking cheezein import nahi ki gayi hain.

### Core modules

| Module | File | Kya karta hai |
|------|------|------|
| **CLI/TUI entry** | `cli/main.py` + `cli/tui.py` | Typer commands + REPL + TUI |
| **Agent core** | `agent/core.py` | AgentCore coordination entry |
| **Solve engine** | `agent/solver.py` + `agent/agent_state.py` | Model-led loop + AgentState evidence/steps/completion gate |
| **Reasoning/reflection** | `agent/reasoning_state.py` + `reflexion.py` | Structured facts/constraints/attack chains + L0-L4 escalation |
| **Plugin system** | `plugins/` | Low-coupling vuln detection plugin runtime |
| **Skill reference index** | `skills/loader.py` + `resolver.py` | Sirf relevant references resolve karta hai, forced flow inject nahi karta |
| **MCP orchestration** | `mcp/registry.py` + `lifecycle.py` + `router.py` | Service registration + lifecycle + tool routing |
| **Config management** | `config/schema.py` + `settings.py` | Pydantic + YAML + 13 provider presets |
| **Report generation** | `report/generator.py` + `poc_builder.py` | Markdown reports + PoC scripts |
| **Security knowledge base** | `kb/store.py` + `retriever.py` + `ranking.py` | JSON storage + Chinese-aware BM25 retrieval + optional re-ranking |

---

## MCP toolchain

| MCP service | Tools count | Mode | Use | Status |
|---|---|---|---|---|
| fetch | 1 | Local (httpx) | HTTP/HTTPS requests, GET/POST/PUT etc. methods, headers/params/cookies/body/json/form, API testing | Out-of-the-box |
| memory | 2 | Local (JSON) | Context memory, state persistence | Out-of-the-box |
| chrome-devtools | 31+ | stdio MCP | Browser automation, screenshots, JS execution | Deployment zaroori |
| burp | multiple | stdio MCP | HTTP capture, replay, vuln scanning | Deployment zaroori |

> Iske alawa built-in Agent tools bhi hain (`http_probe_batch`, `python_execute`, `shell_command`, `source_extract`, `runtime_diff_probe`, `nmap_scan`, `crypto_decode`, `brute_force_login`, `load_skill_reference`, `evidence_list`, `evidence_search`, `evidence_view` etc.) — ye bina MCP ke call ho sakte hain.

<details>
<summary><strong>Chrome DevTools MCP deployment</strong></summary>

**Prerequisites**: Node.js LTS (v20+) + Chrome browser

```bash
# Step 1: Chrome remote debugging start karo
# Windows
"C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222 --user-data-dir=C:\tmp\chrome-debug
# Linux/Mac
google-chrome --remote-debugging-port=9222 --user-data-dir=/tmp/chrome-debug

# Step 2: VulnClaw config enable karo
vulnclaw config set mcp.servers.chrome-devtools.enabled true
```

Chrome debugging address specify karna ho to `~/.vulnclaw/config.yaml` edit karo:

```yaml
mcp:
  servers:
    chrome-devtools:
      enabled: true
      transport:
        type: stdio
        command: npx
        args: ["-y", "chrome-devtools-mcp@latest", "--browser-url=http://127.0.0.1:9222"]
```

</details>

<details>
<summary><strong>Burp Suite MCP deployment</strong></summary>

**Prerequisites**: Java 11+ + Burp Suite Professional

```bash
# Step 1: Clone karke build karo
git clone https://github.com/PortSwigger/mcp-server.git burp-mcp
cd burp-mcp
./gradlew embedProxyJar    # Windows: gradlew.bat embedProxyJar

# Step 2: Burp Suite me load karo → Extensions → Add → Type: Java → burp-mcp-all.jar chuno

# Step 3: Burp ke MCP tab me "Enabled" tick karo

# Step 4: VulnClaw config enable karo
vulnclaw config set mcp.servers.burp.enabled true
```

Recommended config:

```yaml
mcp:
  servers:
    burp:
      enabled: true
      transport:
        type: stdio
        command: java
        args: ["-jar", "~/.vulnclaw/tools/burp-mcp-all.jar", "--sse-url", "http://127.0.0.1:9876"]
```

</details>

> Detailed deployment instructions ke liye [docs/mcp-deployment.md](docs/mcp-deployment.md) dekho

---

## Built-in Skills

### Core Skills (7)

| Skill | Kya karta hai |
|-------|------|
| pentest-flow | Poore pentest flow ka orchestration |
| recon | Info collection flow |
| vuln-discovery | Vuln discovery flow |
| exploitation | Exploitation flow |
| post-exploitation | Post-exploitation flow |
| reporting | Report generation flow |
| waf-bypass | WAF bypass tricks library |

### Specialized Skills (16)

| Skill | Reference docs | Kya karta hai |
|-------|-----------|------|
| web-pentest | 3 | Web application pentesting |
| android-pentest | 9 | Android app pentesting |
| client-reverse | 20 | Client reverse engineering analysis |
| web-security-advanced | 33 | Advanced web security (injection, bypass, exploit chains) |
| ai-mcp-security | 7 | AI/MCP security testing |
| intranet-pentest-advanced | 15 | Advanced intranet pentesting |
| pentest-tools | 16 | Pentest tools quick reference |
| rapid-checklist | 2 | Quick checklist |
| crypto-toolkit | 3 | Encode/decode + crypto (29 operations) |
| **ctf-web** | 8 | CTF web attacks knowledge base |
| **ctf-crypto** | 6 | CTF cryptography attacks knowledge base |
| **ctf-misc** | 6 | CTF misc knowledge base |
| **osint-recon** | 7 | OSINT open-source intelligence collection |
| **cve-triage** | 1 | CVE lookup aur three-level assessment |
| **hackerone** | 1 | HackerOne bounty scope-guard |
| **secknowledge-skill** | 40 | Web+AI security testing knowledge base |

Skills user input ke hisaab se "optional reference index" ke roop me resolve hote hain — Skill content, stage flows ya methodology playbooks system prompt me zabardasti inject nahi hote. Specialized Skills me `references/` directory me detailed methodology docs hoti hain, jo LLM sirf tab load karta hai jab wo khud `load_skill_reference` se unhe keemti samjhe.

### Built-in encode/decode & crypto tools (crypto_decode)

| Category | Operations |
|------|------|
| Encoding/decoding | base64, base32, base58, hex, url, html, unicode, rot13, caesar, morse (har ek me encode/decode) |
| Hashes | md5, sha1, sha256, sha512 |
| Encrypt/decrypt | aes_encrypt, aes_decrypt (CBC mode, PKCS7 padding) |
| JWT | jwt_decode, jwt_encode |
| Auto-detect | auto_decode — saare common encodings try karke matching results return karta hai |

---

## Config management

### LLM providers

```bash
vulnclaw config provider --list    # saare providers dekho
vulnclaw config provider minimax   # ek command me switch
```

| Provider | Command | Default model |
|--------|------|----------|
| OpenAI | `provider openai` | gpt-4o |
| Anthropic Claude | `provider anthropic` | claude-sonnet-5 |
| MiniMax | `provider minimax` | MiniMax-M3 |
| DeepSeek | `provider deepseek` | deepseek-v4-pro |
| Zhipu GLM | `provider zhipu` | glm-4.7 |
| Kimi | `provider moonshot` | kimi-k2.6 |
| Tongyi Qwen | `provider qwen` | qwen3-max |
| SiliconFlow | `provider siliconflow` | DeepSeek-V4-Flash |
| Doubao | `provider doubao` | Doubao-Seed-2.0-Pro |
| Baichuan | `provider baichuan` | Baichuan4-Turbo |
| StepFun | `provider stepfun` | step-3.5-flash |
| SenseTime | `provider sensetime` | SenseNova-6.7-Flash-Lite |
| 01.AI (Yi) | `provider yi` | yi-lightning |
| Ollama (local) | `provider ollama` | llama3.1 |
| Custom | `provider custom` | manually bharo |

### Command-line config

```bash
vulnclaw config list                          # poori config dekho
vulnclaw config get llm.model                 # ek item dekho
vulnclaw config set llm.api_key sk-xx         # API Key set karo
vulnclaw config set session.max_rounds 30     # max rounds set karo (default 15)
vulnclaw config set session.show_thinking false # reasoning process chhupao
```

### Execution approval (bina confirmation run karna)

Dangerous tools (shell / python / PoC) har execution se pehle by default `Approve this execution? [y/N]` confirmation poochhte hain. Adjust karne ke teen tareeke:

```bash
# ① Permanently config me likho (recommended)
vulnclaw config set safety.permission_mode full_access

# ② Sirf current REPL session ke liye (restart par config file wala mode wapas)
mode full_access          # switch karo; bina argument ke current mode dekho

# ③ Sirf ek run ke liye
VULNCLAW_SAFETY_PERMISSION_MODE=full_access vulnclaw solve <target>
```

| Mode | Behaviour |
|------|------|
| `ask` (default) | Har execution par y/N poochhta hai; 300 seconds (`safety.approval_timeout_seconds`) me jawab na mile to auto-reject |
| `auto_review` | Read-only commands whitelist (ls / cat / nmap scans etc.) seedha run hoti hain, baaki par poochhta hai; `safety.trusted_commands` se prefixes add kar sakte ho |
| `full_access` | Sab kuch seedha run, koi sawaal nahi |

> ⚠️ `full_access` me pentest target se aaye pages, response bodies aur report content sab untrusted input hain — prompt injection bina confirmation ke arbitrary commands chala sakta hai. Ye sirf isolated ranges, CTF ya disposable VMs me use karo; real environments ke liye `auto_review` + custom trusted prefixes better hain.

### Configurable options

| Config item | Default | Kya karta hai |
|--------|--------|------|
| `llm.provider` | openai | LLM provider |
| `llm.api_key` | khaali | API Key |
| `llm.auth_mode` | static | `static` ya `oauth` |
| `llm.chatgpt_auto_proxy` | false | Built-in ChatGPT backend bridging proxy auto-start |
| `llm.base_url` | provider ke hisaab se | API base URL |
| `llm.model` | provider ke hisaab se | Model name |
| `llm.temperature` | 0.1 | Sampling temperature |
| `llm.max_tokens` | 4096 | Ek baar me max output tokens |
| `session.engine` | solve | `solve` (model-led) / `team` (role team) / `rounds` (purana fixed-rounds) |
| `session.solve_max_steps` | 240 | Solve ka runaway-safety budget; planned rounds nahi hain, normal kaam model khud finish/ask/no-path karta hai |
| `session.solve_max_directions` | 3 | Purani config se compatibility; default model-led solve me research directions use nahi hote |
| `session.solve_max_tool_rounds` | 6 | Purani config se compatibility; ek model turn ke andar lagataar tool follow-ups ki runaway safety limit, planned workflow rounds nahi |
| `session.context_auto_compact` | true | Sab LLM call paths par context auto-compression allow ya nahi |
| `session.context_compact_trigger_ratio` | 0.70 | Auto-compression trigger hone wala context ratio |
| `session.context_compact_target_ratio` | 0.55 | Compression ke baad target context occupancy ratio |
| `session.context_recent_message_groups` | 12 | Har compression me preserved recent complete message groups |
| `session.context_summary_max_tokens` | 3500 | Long-term structured context summary ka max token budget |
| `session.context_output_reserve_tokens` | 0 | Model output ke liye reserved tokens; 0 hone par `min(llm.max_tokens, 8192)` |
| `session.context_compaction_audit_enabled` | true | Persistent AgentState me compression audit events record hon ya nahi |
| `session.solve_auto_report` | true | Solve me target achieve hone par Markdown review report auto-generate |
| `session.solve_report_show` | true | Auto report banne ke baad terminal par seedha print |
| `session.max_rounds` | 15 | Max rounds |
| `session.output_dir` | ./vulnclaw-output | Reports output directory |
| `session.report_format` | markdown | Report format (markdown / html) |
| `session.poc_language` | python | PoC generation language (python / bash) |
| `session.language` | auto | Interface language (auto / zh / en), default English |
| `session.show_thinking` | false | LLM reasoning process dikhao |
| `session.persistent_rounds_per_cycle` | 100 | Continuous pentest me rounds per cycle |
| `session.persistent_max_cycles` | 10 | Continuous pentest max cycles (0 = infinite) |
| `session.persistent_auto_report` | true | Continuous pentest me har cycle auto report |
| `session.stale_rounds_threshold` | 5 | Dead-loop detection threshold |
| `safety.permission_mode` | ask | Dangerous tool execution approval policy: `ask` (default, har baar y/N) / `auto_review` (read-only whitelist bina confirmation, baaki poochhta hai) / `full_access` (sab direct, no confirmation) |
| `safety.approval_timeout_seconds` | 300 | Bina jawab ke execution approval kitne seconds baad auto-reject |
| `safety.trusted_commands` | khaali | `auto_review` mode me bina-approval command prefixes (jaise `nmap`, `git diff`); disabled names se shuru entries reject hoti hain |
| `safety.enable_python_execute` | true | python_execute built-in tool enable (off karna zyada safe) |
| `safety.python_execute_max_lines` | 50 | Ek python_execute me max code lines |
| `safety.python_execute_show_warning` | true | Har python_execute se pehle security warning dikhao |
| `safety.python_execute_audit_enabled` | true | python_execute audit records local config directory me likho |

### Environment variables

| Variable | Kya karta hai |
|------|------|
| `VULNCLAW_LLM_PROVIDER` | LLM provider name |
| `VULNCLAW_LLM_API_KEY` | API Key |
| `VULNCLAW_LLM_AUTH_MODE` | static / oauth |
| `VULNCLAW_LLM_CHATGPT_AUTO_PROXY` | Built-in ChatGPT proxy |
| `VULNCLAW_LLM_BASE_URL` | API base URL |
| `VULNCLAW_LLM_MODEL` | Model name |
| `VULNCLAW_SESSION_MAX_ROUNDS` | Max rounds |
| `VULNCLAW_SESSION_STALE_ROUNDS_THRESHOLD` | Dead-loop detection threshold |
| `VULNCLAW_SESSION_REASONING_STATE_ENABLED` | Structured reasoning state on/off |
| `VULNCLAW_SESSION_REFLEXION_ENABLED` | Adaptive reflection engine on/off |
| `VULNCLAW_SESSION_REFLEXION_MAX_SAME_FAILS` | Same vuln par lagataar failures ka reflection threshold |
| `VULNCLAW_SESSION_ESCALATION_MAX_LEVEL` | Payload escalation limit (0-4) |
| `VULNCLAW_SESSION_PLUGIN_RUNTIME_ENABLED` | Plugin runtime on/off |
| `VULNCLAW_SESSION_PLUGIN_MAX_REQUESTS_PER_TARGET` | Ek target ke liye plugin request budget |
| `VULNCLAW_SAFETY_PERMISSION_MODE` | Execution approval mode (ask / auto_review / full_access) |
| `VULNCLAW_SAFETY_APPROVAL_TIMEOUT_SECONDS` | Execution approval wait seconds |
| `VULNCLAW_SAFETY_TRUSTED_COMMANDS` | auto_review bina-approval command prefixes (comma-separated) |

Priority: **environment variables > config file > built-in defaults**

Config file `~/.vulnclaw/config.yaml` me hoti hai.

---

## Changelog

Poora changelog [CHANGELOG.md](CHANGELOG.md) me dekhein.

---

## Contribution guide

Open source contributions me swagat hai! PR bhejne se pehle [CONTRIBUTING.md](CONTRIBUTING.md) (Chinese) ya [CONTRIBUTING_EN.md](CONTRIBUTING_EN.md) (English) padh lein.

Project integration testing ke liye `dev` branch use karta hai — saare PRs `dev` branch par hi bhejein.

---

## Security disclaimer

**Public Alpha stage**: VulnClaw authorized security testing, CTF, lab environments aur controlled research scenarios ke liye public Alpha software hai — ise production environment me security control ya authorization mechanism ke roop me use mat karein. Use karne se pehle [SECURITY.md](SECURITY.md) padhein.

VulnClaw sirf **authorized security testing** ke liye hai. Tool use karne se pehle yaqeeni banayein ki:

1. Aapko target system ki **explicit authorization** mili hai
2. Test scope target owner ke saath **likhit me confirm** hua hai
3. Aap local **laws aur regulations** follow kar rahe hain

Bina authorization ke kisi system ka penetration test karna illegal hai. Is tool ke authors misuse ke liye zimmedar nahi hain.

---

## License

[MIT License](LICENSE)

---

## Community me aao

Aur security enthusiasts ke saath discuss karo, share karo aur grow karo

| Community discussion group | Developers group chat |
|:--:|:--:|
| Discuss aur share karo, latest product updates aur usage tips pao | Hamse judo, open source contribution aur deep technical discussions karo |
| ![VulnClaw community discussion group](assets/社区交流群.jpg) | ![VulnClaw developers group chat](assets/VulnClaw开发者群聊.png) |
| **QQ group number: 954402631** | **QQ group number: 1065858551** |

---

<div align="center">

> 🦞 **VulnClaw** — taaki har penetration test ka ek saaf plan ho.

</div>
