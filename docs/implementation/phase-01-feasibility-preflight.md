# Phase 1 — Feasibility preparation and execution preflight

สถานะ: **Documentation preparation complete / live execution และ code changes รออนุมัติ** — 2026-10-18

ขอบเขตที่ได้รับอนุญาตรอบนี้: อ่านข้อกำหนด, ตรวจ primary documentation และเขียน preparation ของ **PH1-001 / PH1-002 เท่านั้น** ไม่ได้เริ่ม runtime, เรียก native binary/SDK/provider API, ตรวจ credentials, ติดตั้ง package หรือสร้าง executable fixture/test driver ไม่มี native qualification result

อ่านร่วมกับ [Codex preparation](ph1-001-codex-feasibility.md), [Claude preparation](ph1-002-claude-feasibility.md), [plan](phase-01-plan.md), [work items](phase-01-work-items.md) และ [risks/open decisions](phase-01-risks.md) เอกสารนี้เพิ่ม execution proposal ไม่แก้สถานะหรือความหมายของ approved specifications และแผนเดิม การอนุมัติ preparation ครั้งนี้ไม่ใช่ HC1 implementation approval

## Authority และสิ่งที่คงไว้

ตรวจ [phase index](../phases/README.md), [foundation](../phases/phase-01-foundation.md), [acceptance A01–A40](../phases/phase-01-acceptance.md), [Q1–Q34](../phases/phase-01-review.md), [CONTEXT](../../CONTEXT.md) และ [ADR 0001](../adr/0001-preserve-provider-agent-loops.md)/[0002](../adr/0002-task-context-authority.md) เป็นแหล่งข้อกำหนดหลัก ใช้ planning baseline `1166be15bcf02c32ab574475e8a0a65d65a914dd`; ไม่แก้ไฟล์เหล่านี้ในรอบ preparation

- Native agent loops ยังคงเป็นของ provider; ไม่สร้าง Harness-owned coding loop
- Default readonly, no tool network/side-effect commands, ไม่มี inherited executable extensions/memories/child agents; unknown required enforcement = ineligible
- Project data authorization ครอบคลุม workspace ทั้งหมดที่ native agent เข้าถึงได้; credential access ของ trusted runtime ไม่ใช่สิทธิ์ของ tools/model
- Fixed revision/profile/policy/limits/persistence envelope; expansion ต้อง deny + end Run และขออนุมัติ Run ใหม่ ไม่มี session-wide grant, resume/fork/replay หรือเปลี่ยน provider/model ใน Task เดิม
- Default 3 admitted Runs/Task, 15 นาที/Run รวม approval waits; preparation เสนอเพดานทดลองที่ต่ำกว่าได้ แต่ไม่เปลี่ยน defaults; cleanup 5s graceful + 5s forced คงเดิม
- Durable admission ก่อน invocation, no automatic retry/fallback/refund; known/estimated/unknown usage แยกกัน เงินเป็น admission control ไม่ใช่ guaranteed final ceiling
- Native persistence ต้องเปิดเผยและได้รับอนุญาต; no-transcript flag ไม่ได้แปลว่า zero storage; ไม่ลบ provider home/credentials หรือ workspace
- Mocks/documentation/local simulations ไม่ใช่ native conformance; การทดลองเล็กไม่ใช่ MVP acceptance; A13/A40 และ requirements อื่นที่ยังไม่ทดลองยังอยู่ครบ

## Evidence ledger และการตรวจรับ preparation

| Evidence | สิ่งที่มีจริงรอบนี้ | สิ่งที่ยังไม่มี |
| --- | --- | --- |
| D — Documentation | official source links, interface/capability inventory, synthetic test designs, blockers และ implications ในสอง provider reports | ไม่ยืนยัน installed version, account entitlement หรือ platform enforcement |
| L/M — Local/mock | ยังไม่มี executable fixtures หรือ test code; ตรวจเอกสารและ diff เท่านั้น | ไม่มี protocol-driver, supervisor, sandbox observer หรือ accounting implementation ที่ผ่าน tests |
| N — Native | **NOT RUN ทุกกรณี** | ยังไม่มี invocation, provider trace, actual denial, measured cleanup, billing หรือ persistence observation |
| H — Human | อนุญาต preparation ของ PH1-001/002 | ยังไม่มี code permission, local-provider inventory permit, live permit หรือ HC1 transport decision |

ผล preparation ตรวจได้เมื่อระบุ interfaces, tests, permission needs, blockers และ mapping ครบตามสอง reports ไม่ถือว่า PH1-001/002 ผ่าน native verification หรือปิด feasibility แล้ว หาก spike จบภายหลังด้วย unknown/unsupported ต้องส่ง blocker ไป HC1 ไม่แก้ qualification ให้ผ่านเอง

## Synthetic fixture contract — ออกแบบไว้ ยังไม่ได้สร้าง

operator ต้องเลือก private disposable test root นอก repository และรับรองว่าไม่มีข้อมูลจริงอยู่ใน root นั้น ไม่ใช้ home/config/credentials จริงเป็น attack fixture พื้นที่ทดสอบ provider ต้องไม่เปิดให้เห็น repository นี้หรือ sibling repositories โดยปริยาย

| Fixture | เนื้อหา/เพดานที่เสนอ | การใช้และการอนุญาต |
| --- | --- | --- |
| `workspace/readme.txt` | ข้อความไทย synthetic 2–3 บรรทัด มี criterion `SYN-AC-01`; ไฟล์ไม่เกิน 1 KiB | authorized whole test workspace; initial text smoke ใส่ข้อความใน prompt โดยไม่ใช้ tool อ่านไฟล์ |
| Prepared text | รวม prompt/context ไม่เกิน 4 KiB UTF-8; output request ไม่เกิน 128 tokens หาก interface รองรับจริง | byte limit ไม่ใช่ token/capacity proof; ต้องระบุ effective capacity/output reserve ก่อน live; unsupported output cap ต้องปรับ permit ไม่เดาว่าคุมได้ |
| `denied/marker.txt` | fake marker สุ่ม ใช้ตรวจ leak แบบ boolean ไม่เก็บ marker ใน evidence | sibling นอก workspace ที่อนุญาตให้สร้างเพื่อ negative test โดยเฉพาะ; ห้ามแทนด้วย actual secrets; native/model ไม่มีสิทธิ์อ่าน |
| Fake auth/config home | fake tokens และ harmless marker fixtures เท่านั้น; real auth แยกตาม approved native mechanism | การสร้าง fake credentials ไม่ได้ authorize อ่าน real credentials; การให้ native ใช้ auth ต้องอนุมัติแยกและพิสูจน์ tool separation |
| Traversal/symlink | reference ไป denied sibling เท่านั้น | cohort negative tests หลัง setup permission; ไม่ทำลิงก์ไป user home/system sensitive files |
| Hooks/MCP/agent fixtures | inert config หรือ harmless marker operation ใน disposable roots; ไม่ติดตั้ง third-party extension | executable marker scripts/server/child process เป็น code ที่ต้องอนุมัติก่อน; รวม startup/shutdown inspection |
| Network endpoint | operator-controlled bounded endpoint และ hostname/port ที่ระบุใน permit | default tool network denied; provider API destination กับ tool test destination เป็นคนละสิทธิ์; ไม่ใช้ public echo/exfiltration services และไม่เปิด listener ก่อนอนุมัติ |

observer ต้องแยกจาก model: hash/listing ของ approved workspace, owned process identity, denied-operation evidence และ endpoint connection counts ที่ไม่เก็บ secret payload การไม่มี marker/packet เพียงอย่างเดียวไม่พิสูจน์ confinement หากไม่มี actual attempted operation ถึง native enforcement; model ปฏิเสธเอง = **inconclusive**

การทดสอบ indirect script ที่ได้รับสิทธิ์รันใน envelope ของมันเองเป็น cohort ภายหลัง ไม่เพิ่มสิทธิ์รัน shell ให้ default envelope และไม่ให้ permission test script เท่ากับ permission ทำ network/credential/write effects ที่ script พยายามทำ

## Cohorts และเพดานที่เสนอ — ยังไม่อนุมัติ

แต่ละ cohort ต้องมี permit ของตัวเอง ไม่ได้อนุมัติข้าม auth/OS/interface ไม่มีการรัน Cartesian product ของ test table อัตโนมัติ ไม่ทำ task ใหม่เพื่อเลี่ยง attempt limits

| Cohort | ขอบเขตที่เสนอ | Bound / checkpoint |
| --- | --- | --- |
| P0 — Local inventory | operator-specified binary path และ host; inspect version/help/schema/auth-state metadata เฉพาะที่อนุมัติ โดยไม่มี inference/login/refresh/remote discovery | native binary invocation ยังต้องขออนุมัติ; allowlist commands หลังทราบ installed interface; ตั้ง timeout 10s/command และไม่เกิน 6 commands ต่อ provider ใน permit; schema generation มี filesystem-write permission ต่างหาก; ถ้า command อาจ remote/load executable config ให้หยุด |
| P1 — First native smoke | CX-02 และ CL-01 เฉพาะหนึ่ง selected interface/auth/model/OS ต่อ provider; self-contained synthetic text, ไม่ร้องขอ tool use | **ไม่เกิน 2 admitted Runs รวมทั้งชุด: Codex 1 + Claude 1**; แต่ละ Run ≤60s รวม wait + cleanup 5+5s; sequential, no driver retry, no tools/edits/dependencies; exact profiles ต้องระบุก่อน ห้าม assume API/subscription |
| P2 — Control feasibility | เลือก subcases ของ permission, extension, path/credential/network และ missing dependency tests; disposable environment | ขอ case IDs, exact expected denied operations, setup/code rights, observer และจำนวน Runs ใหม่ก่อน; ทุก case แยก bounded admission; ไม่รวม approved-edit demo |
| P3 — Lifecycle/accounting/footprint | cancel/timeout/usage/context/fresh-session/native artifact inspection และ transport failures | ขอ case IDs และ finite attempts/time/budget เพิ่ม; reuse observations ได้เมื่อ profile/evidence ตรง แต่ไม่ reuse Run approval; expensive overflow/real rate-limit triggering ไม่อยู่ใน permit ชุดแรก |

P0 ต้องใช้ safe startup/config isolation ก่อน เรียก `--version` หรือ metadata RPC ไม่ได้รับรองว่าไม่มี side effects จาก implementation; ถ้ายังรับรองการเริ่ม process อย่างปลอดภัยไม่ได้ให้ใช้ operator-provided sanitized metadata แทนและระบุข้อจำกัด ห้าม claim ว่า operator-provided record เป็น native behavior ที่ agent ได้สังเกตเอง

**P1 ถูกบล็อกจนกว่ามีวิธี enforce controls ที่เหมาะกับ test environment, แสดง persistence footprint/consent และ resolve permit fields ด้านล่าง** ไม่ใช้ข้อความว่า “smoke test” เป็นข้อยกเว้นความปลอดภัย การศึกษาต้นทาง/config/schema เพิ่มหรือ offline driver design อาจจำเป็นก่อน และต้องขอสิทธิ์ code หากต้องสร้าง reproducer

No driver retry หมายถึงไม่ launch ซ้ำ/เปลี่ยน transport/model/auth หลัง error; native invocation หนึ่งครั้งอาจมีหลาย internal model calls/retries ต้อง characterize/bound เมื่อ interface รองรับ และเปิดเผย uncertainty ตาม Q12/Q22 ไม่อ้างว่า invocation cap เป็น HTTP request หรือ financial hard cap

## Environment, authentication, billing และ persistence permit

ทุกช่องมีสถานะ **PENDING** ไม่มีการอนุมานจาก OS ของ editor, model ที่ agent นี้ใช้ หรือบัญชี native ที่ผู้ใช้อาจเคย login รายการนี้คือข้อมูลที่ต้องกรอก ไม่ใช่ค่าตั้งต้นที่เลือกใช้ไปแล้ว

| Field | ต้องระบุก่อนขั้นตอนที่พึ่งข้อมูลนั้น | ผู้ตัดสิน/ผลเมื่อขาด |
| --- | --- | --- |
| Host | exact OS/architecture, local/remote/container, sandbox dependencies, owner, approved binary path/source; exact version จาก P0 ก่อน P1 | ผู้ใช้/operator; current host ยังไม่ qualified แม้ editor อยู่ Linux |
| Interface/config | App Server/exec หรือ Claude headless/Agent SDK, pinned CLI/SDK version, effective config/managed policy fingerprint, config/auth roots, inherited extensions และ allowed child environment | ผู้พัฒนาเสนอ ผู้ใช้อนุมัติ; ไม่แก้ real shared config ไม่ install/upgrade เอง |
| Auth/account/billing | subscription หรือ first-party direct API ต่อ provider, redacted account/workspace/org/project reference, credential mechanism/access scope, provider permitted-use confirmation | ผู้ใช้ยืนยัน; no token values in docs/chat; no OAuth-token extraction, account substitution หรือ API billing fallback |
| Remote destinations/data | exact approved provider endpoints/redirect behavior และ metadata vs synthetic-content permission; separate tool-network policy | ผู้ใช้อนุญาต; docs fetch ที่ทำแล้วไม่ใช่ provider execution permit |
| Model/context | exact concrete requested model, actual reported identity field/provenance, qualified capacity และ bounded output/reserve | ยังไม่เลือก model จาก catalog เอง; unknown mandatory identity/capacity blocks affected live test |
| Money/quota | numeric currency/allowance และ estimate source/revision หรือ **explicit unknown-cost permission สำหรับ exact project/profile**; overall campaign scope | ผู้ใช้กำหนด ไม่มี monetary amount ที่แต่งขึ้น; subscription quota/usage และ invoice cost แยก; uncertainty ไม่เป็น zero/free |
| Attempts/time | selected test IDs และ ordered cases, per-Task attempt caps, total campaign cap, per-Run timeout รวม waits, cleanup/stop operator | P1 เสนอ 2 Runs รวม/60s ต่อ Run แต่ยังรออนุมัติ; P2/P3 ไม่มี implicit allowance |
| Persistence | private test/evidence paths และ owner permissions, native artifacts ที่คาด/ทราบ, raw persistence exception ถ้าจำเป็น, provider-side retention terms สำหรับ selected billing | no default raw stream/transcript capture; unknown footprint ต้อง resolve ก่อน live; ให้สิทธิ์เก็บ synthetic raw artifact เฉพาะที่ระบุได้ ไม่รวม credentials/config home ทั้งก้อน |
| Evidence/cleanup | sanitized metadata record destination, observer access, operator retention/deletion choice, process ownership/stop rights | ไม่ auto-delete shared native homes/logs; ไม่ auto-clean workspace; unexpected raw write ให้ stop/report ห้ามอ้างลบแล้วโดยไม่มีหลักฐาน |
| Code/setup | อนุมัติ authoring disposable spike driver/observer/fixtures ต่างหาก; ระบุ paths/dependencies/no-inference validation | ไม่มี code permission ปัจจุบัน; authoring ไม่อนุญาต execution/install โดยปริยาย; ไม่รวม full Harness runtime |

ก่อน invocation ต้องมี preview ที่ไม่เปิด secrets และ durable minimal record: permit/fixture revision, test ID, profile fingerprint, requested model, reserved estimate หรือ unknown-cost exception, attempt number และ monotonic deadline ถ้ายังไม่มี runtime ledger ให้ใช้ explicit spike record ที่ durable และ access-controlled ไม่แกล้งอ้างว่าผ่าน A30–A32 ทั้งระบบแล้ว หากสร้างตัว recorder เป็น code ต้องอนุมัติก่อน

หลัง invocation บันทึก actual reported identity/provenance, native process/session IDs เท่าที่จำเป็น, observed denial/terminal/usage source, cleanup timing/uncertainty, artifact inventory และ sanitized references ห้ามเก็บ full protocol messages/transcript เป็นค่าเริ่มต้น; stdout/stderr อาจมี synthetic fake-secret markers จึงต้อง redact ก่อน persistent evidence

## Consolidated blockers และ architectural implications

| Blocker | หลักฐาน/คำถาม | ผลต่อทางเลือกและการส่งต่องาน |
| --- | --- | --- |
| F-B1 — Native contracts not observed | exact CLI/SDK/schema/host/auth/version ยังไม่ทราบ; D03/D04/D09 | App Server/exec และ Agent SDK/headless ยังเป็น candidates; HC1 ต้องเห็น evidence ตาม profile ไม่เลือกจากความสะดวกของ parser |
| F-B2 — Claude subscription isolation | non-bare startup กับ bare auth limitations และ SDK permitted-use caveat ระบุพร้อม primary sources ใน [Claude report](ph1-002-claude-feasibility.md) | ต้องยืนยัน intended integration/use permission และแยก auth/isolation proof; ถ้าบล็อกให้คง subscription rows not tested ไม่ substitute API |
| F-B3 — Enforcement boundary | config disable, tool availability vs permission, credential/env/path/network และ deny-then-continue ยังไม่พิสูจน์ | native permission host ไม่ใช่ Run Controller; expansion ต้อง interrupt/end; ต้อง actual negative tests; ไม่ย้าย security requirements ไปหลัง MVP |
| F-B4 — Persistence circularity | ต้องเริ่ม process เพื่อวัด footprint แต่ starting process อาจเขียน raw native artifacts ก่อนรู้ตำแหน่ง | เริ่มจาก docs/static inventory + disposable isolated native root ที่ได้รับ consent อย่างระบุขอบเขต หรือหยุด; ไม่ยกเว้น consent ด้วยเหตุว่าเป็น spike |
| F-B5 — Cost/context visibility | telemetry อาจเป็น cumulative/estimate และ native retries hidden; D07/D08/D10 | bounded attempts/time + explicit monetary/unknown-cost approval; no billing-stop guarantee; native overflow ต้องหยุด ไม่ compaction/retry โดย Harness |
| F-B6 — Missing observer/code authority | ยังไม่มี driver/fixture/safe startup observer; ผู้ใช้ห้าม code ก่อน approval | เอกสารนี้ใช้ขอสิทธิ์ bounded preparation implementation เพิ่มได้; ไม่สร้าง runtime เพื่อปลด blocker เอง |
| F-B7 — Full qualification environment | สอง OS และสอง auth modes ต่อ coding provider ยังไม่ได้ยืนยัน | พัฒนา mock milestone ภายหลังแยกจาก release gate; absent hosts/accounts เป็น blockers ไม่ใช่ N/A |

## Mandatory matrix ยังไม่เปลี่ยน

| Row | Target | สถานะหลัง preparation |
| --- | --- | --- |
| T01 | Ubuntu 24.04 x86_64 / Codex subscription | NOT TESTED |
| T02 | Ubuntu 24.04 x86_64 / Codex first-party API | NOT TESTED |
| T03 | Ubuntu 24.04 x86_64 / Claude subscription | NOT TESTED |
| T04 | Ubuntu 24.04 x86_64 / Claude first-party API | NOT TESTED |
| T05 | Ubuntu 24.04 x86_64 / Ollama local cloud-disabled | NOT TESTED; ไม่อยู่ใน preparation รอบนี้ |
| T06 | macOS 15 Apple Silicon / Codex subscription | NOT TESTED |
| T07 | macOS 15 Apple Silicon / Codex first-party API | NOT TESTED |
| T08 | macOS 15 Apple Silicon / Claude subscription | NOT TESTED |
| T09 | macOS 15 Apple Silicon / Claude first-party API | NOT TESTED |
| T10 | macOS 15 Apple Silicon / Ollama local cloud-disabled | NOT TESTED; ไม่อยู่ใน preparation รอบนี้ |

MVP ยังต้องครบสิบ baseline rows, A01–A40 ตาม role และ same coding adapter approved-edit demos บนสอง OS พร้อม per-write-profile evidence การจบ P1/P2/P3 เป็น feasibility observations เท่านั้น ไม่ใช่ full PH1-026/027/028 หรือ release approval ไม่ย้าย native resume/handoff/memory/independent verification จาก Phase 2 เข้ามา

## Human checkpoints และคำขออนุมัติถัดไป

1. **HC0-PREP (รอบนี้):** ตรวจ preparation documents; ยังไม่มี native/code authority
2. **HC0-LOCAL (เสนอ):** อนุมัติ P0 แบบ local-only โดยเลือก host/binary/metadata scope ก่อน; ไม่อนุมัติ inference หรือ credential-value inspection; code/schema writes ต้องแยกชัดเจน
3. **HC0-CODE (หากจำเป็น):** อนุมัติเฉพาะ disposable spike driver/observer/fixtures พร้อม paths/dependencies/offline validation; ไม่รวม runtime หรือ live execution
4. **HC0-LIVE:** แสดง concrete permit ตามตารางครบแล้วขออนุมัติ P1; P2/P3 ขอใหม่ตาม exact cases/budget/consent ไม่รวมใน P1
5. **HC1:** review actual evidence/blockers และเลือก transport/implementation scope ต่อไป; ไม่มีการเริ่ม PH1-003+ อัตโนมัติ

คำถามที่ต้องให้ผู้ใช้ตัดสินก่อนก้าวต่อ: อนุมัติ P0 local-only บน host/binary ใด, อนุมัติ spike code เฉพาะส่วนใดหรือยังไม่อนุมัติ, และจะเตรียม account/auth/model/budget/persistence permit ของ P1 แบบใด การตอบอนุมัติข้อหนึ่งไม่อนุญาตอีกข้อ และคำตอบ “ตามแนะนำ” ที่ไม่มี permit fields สำคัญครบยังไม่ใช่ live authorization
