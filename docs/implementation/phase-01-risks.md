# Phase 1 — Implementation Risks and Open Decisions

สถานะ: **แผนเสนอ รออนุมัติ** — 2026-10-17 ความเสี่ยงด้านล่างยัง Open ไม่มีรายการใดถือว่าได้รับการยอมรับแทนผู้ใช้หรือพิสูจน์แล้ว

อ้างอิง [approved review risks R1–R12](../phases/phase-01-review.md), [implementation plan/checkpoints](phase-01-plan.md) และ [work items](phase-01-work-items.md) รหัส PR/D เป็นรายการวางแผน ไม่แก้ข้อกำหนด Q1–Q34 หรือ A01–A40

## Risk register

Owner ด้าน implementation หมายถึงผู้พัฒนาที่ได้รับมอบหมาย ไม่ใช่ทีม/บุคคลที่สมมติว่ามีอยู่แล้ว การอนุญาต environment/data/billing และการเปลี่ยน scope เป็นหน้าที่ของผู้ใช้

| Risk | ความเสี่ยงและผลกระทบ | Mitigation / หลักฐานที่ต้องมี | Owner / gate / งานที่เกี่ยวข้อง |
| --- | --- | --- | --- |
| PR01 — Native feasibility | CLI/SDK อาจบังคับ readonly, tool network, approvals, cleanup หรือ identity ตามสเปกไม่ได้ ทำให้ไม่มี operational coding route | spikes แยก transport/profile; live negative tests; unsupported = ineligible ไม่ใช่ unsafe fallback เอกสารหรือ flags ไม่ใช่ proof | ผู้พัฒนา + ผู้ใช้ที่ HC1/HC3; PH1-001, PH1-002, PH1-017, PH1-018, PH1-026; inherits R1/R12 |
| PR02 — Auth/config isolation | subscription/API อาจโหลด configuration หรือ persistence ต่างกัน; bare/headless setting ที่ลด extensions อาจใช้ auth แบบหนึ่งไม่ได้ | แยก effective config, credential helper, extensions และ native artifacts ทุก auth row; ห้าม auto switch ไป API billing หรืออ้างผลข้าม mode | ผู้พัฒนา/ผู้ใช้ HC1/HC4; PH1-001, PH1-002, PH1-009, PH1-017, PH1-018, PH1-023; inherits R2 |
| PR03 — Mandatory environments | ไม่มี macOS 15 Apple Silicon/Ubuntu 24.04 x86_64 ที่เหมาะกับ native sandbox หรือไม่มีบัญชีทั้งสอง auth modes ทำให้ตรวจครบสิบ row ไม่ได้ | ระบุ test hosts/accounts/owners ก่อน campaign; ตรวจ exact OS/architecture ไม่ใช้ CI label ทั่วไป; unavailable row = not tested และ MVP blocked | ผู้ใช้จัดหา/อนุญาต environment ที่ HC0/HC6; PH1-025–PH1-028; inherits R10 |
| PR04 — Permission escape | approved command หรือ workspace path อาจเปิด indirect script effects, symlink escape, inherited credentials, hooks/MCP/agents หรือ Git drivers ที่ไม่อนุมัติ | real confinement tests รวม allowed script, tool-network denial, fake credential paths, inherited env และ config injection; scope expansion deny/end; no commit/push/PR/deploy | ผู้พัฒนา HC3/HC5; PH1-009, PH1-010, PH1-017, PH1-018, PH1-021, PH1-026; inherits R1/R9 |
| PR05 — Model/discovery truth | catalog อาจไม่ยืนยัน account entitlement, alias เปลี่ยนหรือ service reroute; private prompt อาจถูกส่งก่อนตรวจพบ | แยก requested/reported identity และ live/curated provenance; determine safe preflight/approved synthetic probes ใน spikes; unknown mandatory signal บล็อก; stop mismatch ไม่อ้างว่าย้อนคืนข้อมูล/ค่าใช้จ่าย | ผู้พัฒนา HC1/HC4; PH1-001, PH1-002, PH1-016–PH1-020; inherits R3 |
| PR06 — False locality | localhost อาจเป็น proxy หรือ Ollama cloud model; การตั้ง env ให้ child ไม่เปลี่ยน daemon ที่เปิดไว้ | trusted evidence ของ running daemon/model digest + observed locality/egress tests; unverified profile ineligible; ไม่เปลี่ยน service/restart/download แทน operator | ผู้พัฒนา + operator HC4; PH1-019, PH1-026; inherits R4 |
| PR07 — Context/requirements loss | token estimate โดยเฉพาะ Thai/Unicode ไม่แม่น, native context ต่างจาก payload, refs/policy เปลี่ยนหลัง preview หรือ acceptance ถูกใช้ข้าม revision | qualified capacity/method/reserve; immutable revisions; source fingerprints + final revalidation; no silent truncation; criterion acceptance ผูก exact revision | ผู้พัฒนา HC3/HC5; PH1-005, PH1-010, PH1-011, PH1-020, PH1-022; inherits R5/R11 |
| PR08 — Cost uncertainty | usage/cost หายหลัง cancel, subscription quotas ไม่เทียบ API invoice, native retries/เครื่องมือทำให้ใช้เกิน estimate | durable per-Task reservations/attempts, exact arithmetic, explicit unknown-cost permission, no refund/double count, timeout; รายงาน monetary subtotal ว่าไม่ครบเมื่อมี unknown | ผู้พัฒนา/ผู้ใช้ HC3/HC6; PH1-007, PH1-012–PH1-014, PH1-026; inherits R6 |
| PR09 — Crash/locks/process identity | หลายไฟล์ไม่ใช่ transaction เดียว, competing instances, stale lock/PID reuse หรือ crash หลัง spawn ทำให้ duplicate writes/usage state ผิด | prove admission protocol ทั้งสอง OS; no dispatch ก่อน durable admission; lock canonical identities; indeterminate state และ operator review แทน auto replay/refund/kill | ผู้พัฒนา HC3; PH1-010, PH1-012, PH1-014, PH1-023, PH1-024; inherits R8 |
| PR10 — Persistence/privacy | default journal หรือ native logs/cache/temp เก็บ raw/PII/credential; delete อาจลบเกิน ownership หรืออ้างลบ provider data | minimized metadata, redaction tests, native footprint แยก profile, no-transcript mode ที่ตรวจแล้ว, explicit raw consent, owned-only deletion และ partial failure report | ผู้พัฒนา/ผู้ใช้ HC3/HC5/HC6; PH1-015, PH1-017–PH1-019, PH1-023; inherits R7/R9 |
| PR11 — Terminal/protocol input | split escape sequences, oversized events, output ที่เหมือน approval หรือ malformed stream หลอก UI/กินหน่วยความจำ/ทิ้ง orphan work | bounded framing/sanitization, trusted approval UI แยกจาก provider text, PTY/Unicode/Ctrl-C/EOF tests; missing completion ไม่เป็น success | ผู้พัฒนา HC2/HC3/HC5; PH1-006, PH1-008, PH1-014, PH1-015, PH1-022 |
| PR12 — False evidence | green mocks, recorded streams หรือผลหนึ่ง OS/auth ถูกอ้างเป็น conformance/quality/success ทั้งระบบ | evidence provenance และ namespace แยก, registry rejects mock profiles, A03 positive ทุก row, live native assertions, revision-bound human acceptance และ final evidence audit | ผู้พัฒนา + ผู้ใช้ HC2/HC4/HC7; PH1-006, PH1-016, PH1-020, PH1-026–PH1-028; inherits R11 |
| PR13 — Overbuilt first slice | ถ้าทำ routing/UI/accounting เต็มก่อน end-to-end จะพบ interface mistakes ช้า แต่การเร่ง slice อาจกลายเป็นเปิด native ก่อนพร้อม | M1 เป็น mock-only, minimal readonly flow และ guardrails ขั้นต่ำ; unsupported features deny; M2 ก่อน native integration ไม่มี shipped unsafe-development path | ผู้พัฒนา/ผู้ใช้ HC2/HC3; PH1-003–PH1-016; inherits R10 |
| PR14 — Stack/package drift | runtime/Ink/React/SDK versions ไม่เข้ากัน หรือ upcoming README ถูกใช้แทน stable release; packaging เปลี่ยน signal/ESM behavior | pin exact dependency/runtime versions และ stable docs/tag; typecheck + packaged CLI/PTY smoke บน targets; invalidate changed conformance profile; ไม่มี implicit upgrade | ผู้พัฒนา HC1/HC6; PH1-003, PH1-004, PH1-025, PH1-026; inherits R12 |
| PR15 — Scope/authority creep | ถ้า runtime เริ่ม native resume, handoff, learned routing, global caps หรือไปเปลี่ยน approved specs เพื่อปิด blocker จะผิด Q decisions | scope check ทุก item/checkpoint; product decisions อยู่กับผู้ใช้; implementation plan ไม่ใช่ runtime/live permission และไม่อนุญาต tracker/publish/deploy | ผู้พัฒนา/ผู้ใช้ทุก checkpoint; PH1-003, PH1-028; inherits approved phase boundaries |

ไม่มี mitigation ใดรับประกันหยุด billing กลาง request, ป้องกัน malicious provider/OS ที่อยู่นอก trusted model, ตรวจ secret ทุกชนิด หรือทำ generalized quality guarantee ทั้งหมดนี้ยังคงขอบเขตตาม approved specification

## Unresolved implementation decisions

ทุก D มีสถานะ **Pending** ข้อเสนอไม่ใช่ค่าที่ตัดสินใช้ไปแล้ว หากยังไม่ถึงจุดตัดสิน งาน mock ที่ไม่พึ่ง choice นั้นทำต่อได้หลังได้รับอนุญาต แต่ native feature ที่พึ่ง choice ต้อง block

| ID | สิ่งที่ยังไม่ทราบ/ต้องเลือก | ข้อเสนอและทางเลือก | หลักฐานขั้นต่ำ / owner / checkpoint |
| --- | --- | --- | --- |
| D01 | Exact Node/TypeScript/schema/test dependency versions และ module target | Node 24 LTS + strict compiled ESM + JSON Schema/Ajv; เปรียบเทียบ runtime loader, Zod และ test runner ก่อน lock | stable version/dependency compatibility บนสอง OS, ไม่ mutate input ตอน validation; ผู้พัฒนาเสนอ ผู้ใช้รับที่ HC1 ผ่าน PH1-003 |
| D02 | Ink หรือ readline สำหรับ terminal และวิธีทดสอบ input/stream | Ink minimal view เป็น candidate; readline ลด dependencies แต่เพิ่ม redraw/signal work; ไม่ implement สอง frontends ถาวรโดยไม่จำเป็น | tiny offline UI experiment ด้วย Thai/Unicode, Ctrl-C, safe stream และ testing compatibility; HC1 ก่อน PH1-008 |
| D03 | Codex transport/version และ safe entitlement/model-identity preflight | App Server candidate เทียบ exec JSON; เลือกเฉพาะ interface ที่ผ่าน fixed envelope; catalog ไม่ยืนยัน entitlement | PH1-001 evidence แยก auth/OS พร้อม missing/mismatch/cancel cases; ผู้ใช้ตัดสินที่ HC1; unresolved = native disabled |
| D04 | Claude SDK/headless transport, permission host และ subscription-safe profile | TypeScript SDK candidate เทียบ structured CLI; credential helper/permission host ต้องอนุมัติ ไม่โหลด extensions โดยปริยาย | PH1-002 แสดง effective config/auth/isolation/persistence; bare ไม่ใช่หลักฐาน subscription support; HC1/HC4 |
| D05 | File transaction layout, lock/process identity และ OS-specific state paths | immutable JSON + durable admission/outcomes; JSONL/index เป็น derived view; เสนอ private OS state directory แต่ยังไม่สร้าง path; SQLite metadata index เป็นทางเลือกที่ต้องพิสูจน์ cross-store consistency | design รับที่ HC1; crash/partial-write/fsync/rename/alias/PID tests ผ่าน PH1-010/PH1-012/PH1-014 ก่อน HC3; ไม่อ้าง JSONL append อย่างเดียวเป็น transaction |
| D06 | Primary write adapter, enabled write auth profiles และ models | เลือกจาก Codex/Claude ที่ผ่านหลักฐานทั้งสอง OS; ยังไม่เลือกผู้ชนะหรือตั้ง task/model quality ranks | PH1-017/PH1-018 + bounded task fixtures + permission proof; ผู้ใช้เลือก HC5 ก่อน PH1-021; baseline อีก provider ยัง mandatory |
| D07 | Effective context limits, tokenizer/estimator และ output/working reserve ต่อโมเดล | เลือก supported tokenizer หรือ conservative method ที่อธิบาย uncertainty ได้; ไม่มี chars-as-tokens assumption และไม่เดา context จากชื่อ model | exact profile capacity, Thai/Unicode/oversize/native overflow tests; PH1-011; ผู้พัฒนาเสนอ ผู้ใช้รับก่อน HC3/model use; unknown required capacity = ineligible |
| D08 | Currency/precision/rounding, pricing source/freshness และ operator spending limits | exact decimal/integer representation ต่อ currency ที่รองรับ; ไม่ใช้ floating-point/FX assumptions; API estimate, subscription quota และ unknown แยกกัน | PH1-013 arithmetic/reconciliation fixtures และผู้ใช้กำหนด allowance/unknown-cost permissions; HC3/HC6; ไม่ตั้งวงเงินเงินจริงแทนผู้ใช้ |
| D09 | Test hosts/access และ account ownership สำหรับ T01–T10 | ต้องมี exact Ubuntu/macOS targets และ subscription/API access ของทั้ง coding providers พร้อม installed local Ollama models; ยังไม่ทราบ host/credentials ที่ใช้ได้ | ผู้ใช้ยืนยัน environment/access plan ที่ HC0 และก่อน HC6; ไม่มี host หรือ account = row not tested ไม่ลด targets |
| D10 | Live test authorizations, provider versions/endpoints และ campaign budget | แบ่ง bounded experiments/campaign ตาม profile; synthetic fixtures, explicit destination/persistence, capped attempts/time และ known/unknown cost consent | ผู้ใช้อนุมัติ live permit ก่อน invocation ทุกครั้งหรือภายใน scope/วงเงินที่อนุมัติไว้; HC0/HC3/HC5/HC6; plan approval ไม่พอ |
| D11 | Native artifact footprint, raw persistence exceptions และ evidence locations | no-transcript preference + dedicated configuration; metadata-only defaults; raw/native artifacts ที่หลีกเลี่ยงไม่ได้ต้องแสดงและขอ opt-in ไม่ถือว่า flag เดียวปิด storage ทั้งหมด | per-profile filesystem/retention inspection จาก PH1-001/PH1-002/PH1-017–PH1-019/PH1-023; ผู้ใช้รับก่อนเปิดใช้ profile; unknown/unapproved footprint block |
| D12 | PTY tool, package distribution details และ pinned UI/native test dependencies | node:test + UI-specific tools เป็น candidate; เลือก PTY harness ที่พิสูจน์ได้ทั้งสอง OS; npm compiled package ก่อน single executable | dependency/license/security review, build/terminal/package proof; design HC1 และ packaging proof HC6; ไม่มี publish authority |

D01–D05/D12 ต้องแยก “เลือกแนวทาง” ออกจาก “พิสูจน์แนวทางบน native profile แล้ว” HC1 อาจรับข้อเสนอสำหรับ offline work ได้ แต่ไม่รับรอง proof ที่ยังต้องทำใน M2/M3 ทุก choice ที่มีผลต่อสเปกต้องขออนุมัติแก้สเปกต่างหาก ไม่เพิ่ม ADR หรือย้าย authority ในรอบวางแผนนี้

## Feasibility stop conditions

หยุด native path และนำประเด็นกลับให้ผู้ใช้ตัดสินใจเมื่อ:

- ไม่มี transport/profile ที่บังคับ readonly/tool-network/credential/extension limits หรือ model identity ได้ตาม requirement
- subscription/API isolation ไม่สามารถพิสูจน์ได้โดยไม่เปลี่ยน billing/auth เอง หรือ permission host ต้องใช้ uncontrolled extensions
- ไม่มีหลักฐาน locality ของ running Ollama หรือไม่มี effective context capacity ที่ใช้ตรวจ admission ได้
- storage/locking/cleanup ยังทิ้งโอกาส replay, refund หรือ dispatch ก่อน durable admission ที่ทดสอบแก้ไม่ได้
- raw native persistence อยู่นอกขอบเขตที่อนุมัติ หรือ test environment อาจเปิดเผยข้อมูลจริงที่ไม่อนุญาต
- test hosts/accounts/budget ไม่ครบ mandatory matrix หรือไม่มี same write adapter บนทั้งสอง OS

ทางเลือกที่เสนอให้มนุษย์พิจารณาได้คือเปลี่ยน implementation approach ภายใต้ข้อกำหนดเดิม, เพิ่ม bounded spike, พัฒนา mock-only ส่วนที่ไม่เกี่ยวต่อ หรือเสนอแก้สเปกอย่างชัดเจน ไม่มี silent downgrade, subscription-to-API substitution, unsafe bypass หรือทำเครื่องหมาย mandatory row เป็น N/A เพื่อให้ผ่าน

## หลักฐานจากเอกสารกับสิ่งที่ยังต้องพิสูจน์

อ่าน primary documentation เมื่อ 2026-10-17 เฉพาะเพื่อออกแบบ spikes ไม่เรียก provider, ไม่ตรวจ credentials และไม่อ้างว่า accounts/versions ในเครื่องผ่านแล้ว

| Primary source | ใช้ประกอบข้อเสนอใด | สิ่งที่เอกสารไม่พิสูจน์ให้ |
| --- | --- | --- |
| [Node release schedule](https://nodejs.org/en/about/previous-releases) | Node 24 เป็น LTS ณ วันที่ตรวจ จึงเป็น runtime candidate | exact dependencies และ signal/filesystem behavior ของ package จริงบน targets |
| [Ink](https://github.com/vadimdemedes/ink) | React terminal rendering และ UI testing approach | stable tag/Node/React compatibility; master README อาจเป็น upcoming release |
| [TypeScript strict](https://www.typescriptlang.org/tsconfig/strict), [module guidance](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options) | strict compiled Node ESM proposal | runtime validation หรือ provider protocol correctness |
| [Ajv options](https://ajv.js.org/options.html), [Node filesystem](https://nodejs.org/api/fs.html) | validation behavior และ storage primitives ที่ต้องเลือกใช้ | crash-safe multi-record transaction/locking ของ implementation ที่ยังไม่มี |
| [Codex App Server](https://developers.openai.com/codex/app-server/), [non-interactive](https://developers.openai.com/codex/noninteractive/) | transport alternatives สำหรับ PH1-001 | capability/entitlement/enforcement ตาม exact profile ของผู้ใช้ |
| [Claude headless](https://code.claude.com/docs/en/headless), [sandboxing](https://code.claude.com/docs/en/sandboxing) | auth/permission-host/config isolation hypotheses ของ PH1-002 | subscription-safe configuration และ strict confinement ที่ทดสอบจริงทุก row |
| [Ollama FAQ](https://docs.ollama.com/faq) | running service/cloud-disabled locality hypothesis | server configuration, digest, effective capacity หรือ cancellation ของ daemon ที่จะใช้จริง |

## Approval request

ขอให้ผู้ใช้รับหรือปรับแผนและขอบเขตเริ่มต้น PH1-001/PH1-002 ก่อนเริ่ม implementation คำตอบที่อนุมัติเพียงการเขียนเอกสารแผนไม่ถูกตีความเป็นสิทธิ์เขียน runtime หรือ live provider invocation; HC0 ต้องบันทึกขอบเขต implementation และ execution permits ให้ชัดเจนก่อนทำขั้นตอนที่เกี่ยวข้อง
