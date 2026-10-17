# Phase 1 — Implementation Plan

สถานะ: **แผนเสนอ รออนุมัติ** — 2026-10-17 ไม่ใช่ implementation authorization หรือ provider conformance evidence

## แหล่งข้อกำหนดและสถานะปัจจุบัน

อ้างอิง baseline commit `882a68fbdfa9dde2838a117f5e19ff7af2d2706b` และเอกสารที่อนุมัติแล้ว: [phase index](../phases/README.md), [Phase 1 specification](../phases/phase-01-foundation.md), [A01–A40](../phases/phase-01-acceptance.md), [Q1–Q34 และ risk register](../phases/phase-01-review.md), [glossary](../../CONTEXT.md), [native-agent ADR](../adr/0001-preserve-provider-agent-loops.md) และ [Task Context ADR](../adr/0002-task-context-authority.md)

repository มีเฉพาะเอกสาร ยังไม่มี runtime, dependency manifest, build หรือ test harness การออกแบบด้านล่างเป็นข้อเสนอสำหรับ greenfield ไม่ใช่คำอธิบาย implementation ที่มีอยู่ ไม่ได้ตรวจ sibling repositories หรือบัญชี provider

รอบการวางแผนนี้สร้างเฉพาะเอกสารสามไฟล์: ไฟล์นี้, [work items](phase-01-work-items.md) และ [risks/decisions](phase-01-risks.md) ไม่แก้สเปก, glossary หรือ ADR ที่อนุมัติแล้ว ไม่ติดตั้ง dependencies ไม่เรียก provider และไม่เริ่มเขียน runtime

## หลักการจัดลำดับ

1. เริ่ม PH1-001/PH1-002 เพื่อพิสูจน์ความเป็นไปได้ของ Codex/Claude ก่อนลงทุนกับ integration เต็มรูปแบบ แยกผลอ่านเอกสารออกจากผลทดลองจริง
2. ทำ vertical slice แบบ offline/mock ให้ครบวงจรก่อน Auto, real adapters, accounting เต็มรูปแบบ และงานเขียน: สร้าง Task → preview/approve → durable admission → stream/cancel → journal → เปิด Task หลัง restart
3. milestone ระหว่างพัฒนาเปิดได้เฉพาะความสามารถที่ทำเสร็จแล้ว งานที่ยังบังคับข้อกำหนดไม่ได้ต้องปฏิเสธ ไม่ใช่เปิดใช้ผ่าน development bypass
4. ก่อน native execution ต้องมีทั้งการอนุญาตทดลองอย่างเจาะจงและมาตรการที่เกี่ยวข้องครบ ผล mock ไม่มีสิทธิ์เปลี่ยนสถานะ native profile เป็น passed
5. พัฒนาและตรวจทีละ profile ได้ แต่ MVP จะผ่านได้เมื่อครบ mandatory matrix สิบรายการ และ coding adapter ตัวเดียวกันอย่างน้อยหนึ่งตัวผ่านงานเขียนบนทั้งสอง OS

Q1–Q34 คงเดิมทั้งหมด มีตาราง traceability ใน work-items การเปลี่ยนรายละเอียด implementation ที่ทำให้ต้องผ่อนข้อกำหนดให้หยุดและเสนอการเปลี่ยนสเปกแยกต่างหาก

## Milestones และ dependencies

| Milestone | Work items | ผลที่ตรวจรับได้ | สิ่งที่ยังอ้างไม่ได้ |
| --- | --- | --- | --- |
| M0 — Feasibility/design | PH1-001–PH1-003 | รายงาน Codex/Claude แยก transport/auth/OS, รายการ blocker และข้อเสนอ stack/interface | ยังไม่มี adapter ผ่าน conformance; เอกสารเพียงอย่างเดียวไม่ยืนยัน native controls |
| M1 — Mock vertical slice | PH1-004–PH1-008 | terminal flow ครบหนึ่ง Task/Run ด้วย mock; approval, admission, bounded stream/cancel, private journal, restart | ไม่มี production native route; ไม่มีเงินหรือ inference จริง; ไม่ใช่ MVP |
| M2 — Native prerequisites | PH1-009–PH1-016 | policy, workspace/locks, context, durable accounting, supervisor, privacy และ discovery ผ่าน local/contract tests | native permission/locality/persistence ยังต้องทดสอบจริง |
| M3 — Controlled native integration | PH1-017–PH1-020 | adapter integrations และ static routing ตรวจทีละ profile ใน test environment ที่อนุมัติ | ผลบาง profile ไม่เท่ากับสิบ baseline rows หรือประกาศรองรับทุก auth/OS |
| M4 — Complete Phase 1 behavior | PH1-021–PH1-025 | approved edit, command/lifecycle ครบ, privacy/delete, cross-feature fault tests, package/setup พร้อมตรวจรับ | งานเขียนจาก mock หรือ OS เดียวไม่ผ่าน A13; package ไม่ใช่ release approval |
| M5 — Full MVP qualification | PH1-026–PH1-028 | สิบ baseline profiles, A01–A40 ตาม applicability, same-adapter edit demos สอง OS และ human sign-off | ไม่มีสิทธิ์ลด mandatory targets หรือเลื่อน requirement ที่ยังไม่ผ่านไป Phase 2 เอง |

เส้นทางหลักคือ M0 → M1 → M2 → M3 → M4 → M5 โดย PH1-001/PH1-002 ไม่มี dependency ต่อกัน และ PH1-017/PH1-018/PH1-019 ไม่มี dependency ว่าต้องเลือก provider ตัวแรกให้ผ่านก่อนจึงเริ่มเขียนอีกตัว รายละเอียด DAG อยู่ใน work-items ไม่กำหนดระยะเวลาหรือจำนวนผู้พัฒนาจนทราบผล spikes และ test-host availability

M0 ที่ยังมี native unknown อนุญาตให้ขออนุมัติ M1 แบบ mock-only ต่อได้ แต่ไม่อนุญาตให้ข้าม native checkpoint หากได้หลักฐานว่า requirement ทำไม่ได้ ต้องเสนอทางเลือกให้มนุษย์ตัดสินใจ ไม่เลือกทางลัดเอง

## ข้อเสนอ TypeScript stack

ทุกตัวเลือกในตารางยังเป็น **proposed** ต้องตัดสินที่ HC1 หลังดูผล spikes ไม่มี package versions หรือ lockfile ถูกสร้างในรอบนี้

| เรื่อง | ข้อเสนอเริ่มต้น | ทางเลือกและ trade-off | หลักฐาน/จุดตัดสิน |
| --- | --- | --- | --- |
| Runtime | Node.js 24 LTS; pin exact patch ก่อน bootstrap | Bun/Deno ต้องเพิ่ม compatibility proof ของ process, signals, SDK และ filesystem; ไม่เพิ่ม runtime target โดยปริยาย | Node ระบุ v24 เป็น LTS ณ วันที่ตรวจเอกสาร; patch และ dependency support ยังรอ D01 ([Node releases](https://nodejs.org/en/about/previous-releases)) |
| TypeScript | strict checking, ESM, NodeNext resolution พร้อม target ที่ตรึงให้ตรง Node; build JS ด้วย tsc | runtime TS loader ลดขั้นตอน dev แต่เพิ่ม loader differences; ไม่ใช้เป็น production assumption | strict ไม่แทน runtime validation; pin compiler/module options ที่ HC1 ([strict](https://www.typescriptlang.org/tsconfig/strict), [module options](https://www.typescriptlang.org/docs/handbook/modules/guides/choosing-compiler-options)) |
| CLI | Ink แบบหน้าจอง่ายเพียงหนึ่งหน้า ตั้งแต่ M1; command parser เป็น pure module | node:readline ลด React dependencies แต่ต้องจัดการ redraw/input ขณะ stream เอง; Commander ช่วย startup flags แต่ไม่แทน interactive controller | ทดลอง Thai input, Ctrl-C, output sanitization และ test compatibility ก่อนเลือก D02; ไม่สร้าง UI สองชุดถาวร ([Ink](https://github.com/vadimdemedes/ink)) |
| Schema | JSON Schema + Ajv; validate โดยไม่ coerce, เติม defaults หรือ remove fields เงียบ ๆ | Zod ทำ TypeScript types สะดวก แต่ต้องพิสูจน์ schema export/version authority; ห้ามมี JSON/Markdown สอง authority | D01 เลือกและ pin validator; unknown schema ยัง fail closed ([Ajv options](https://ajv.js.org/options.html)) |
| Tests | node:test สำหรับ compiled core/contract tests; UI tests ตาม framework และ PTY tests บน OS จริง | Vitest สะดวกต่อ fake timers/TS แต่เพิ่ม toolchain; snapshot ของ UI อย่างเดียวไม่ทดสอบ process/permission | D12 เลือก PTY tooling และ dependency versions; fake clock ไม่ยืนยัน real-time cleanup |
| Storage | canonical immutable Task Context JSON, per-Run admission/outcome records และ sanitized journal ใน private local directory | SQLite อาจช่วย derived indexes/ledger transactions แต่เพิ่ม cross-store consistency; ใช้เป็น authority แทน Task Context JSON ไม่อยู่ในสเปกที่อนุมัติ | พิสูจน์ crash/lock protocol ก่อน native use; ไม่สมมติว่า rename ไฟล์หนึ่งทำ transaction หลายไฟล์ให้ ([Node fs](https://nodejs.org/api/fs.html)) |
| Packaging | npm package ที่มี compiled JS และ executable puen; เริ่มจาก local package smoke test | single executable/bundling ลด runtime setup แต่เพิ่ม native/ESM/SDK packaging proof | D12; ไม่มี publish/install provider อัตโนมัติ; artifact ต้องใช้ pinned dependencies |

README บน main branch ของ Ink ระบุว่าอาจอธิบายรุ่นที่จะออก จึงต้องเลือก stable release/tag และทดสอบกับ Node/React ที่ pin จริง ไม่คัดลอก requirement จาก HEAD มาอ้างเป็น compatibility result

## Module design และ interfaces ที่เสนอ

แยก deep modules ตามสิ่งที่ต้องเปลี่ยนและทดสอบจริง UI ไม่เรียก provider, shell หรือ storage โดยตรง และไม่สร้าง generic plugin framework สำหรับสิ่งที่ยังไม่มีความแปรผัน

| Module / seam | Interface ที่ caller ต้องรู้ | สิ่งที่ implementation ต้องรับผิดชอบ |
| --- | --- | --- |
| Terminal | ส่ง local commands และแสดง sanitized view/state | input, rendering, key handling; ไม่มีอำนาจอนุมัติจากข้อความ provider |
| Task Context | draft, validate, approve revision, render, inspect | IDs, canonical JSON, source fingerprints, capacity checks; Markdown เป็น generated view |
| Policy & Eligibility | ประเมิน request กับ immutable policy/profile snapshot แล้วคืน allow/deny พร้อมเหตุผล | restrictive intersection, locality/auth/capability floors, version invalidation, static routing; ไม่เรียก LLM |
| Run Controller | prepare preview, admit approved Run, observe, cancel | lifecycle เดียวสำหรับ mock/native; revision-bound approval, deadlines, no retry, separate checks/acceptance |
| Task Store & Admission Ledger | บันทึก revision/admission/outcome, อ่าน usage, ถือ lock, reconcile และ delete เฉพาะข้อมูลที่เป็นเจ้าของ | atomicity, permissions, durable reservations, crash states และ deletion; caller ไม่แก้ JSON files เอง |
| Provider Adapter | inspectLocal; discovery ภายใต้ metadata permit; execute approved request; interrupt/cleanup ภายใต้ supervisor | native protocol parsing, identity/usage/error mapping และ native approval decisions ภายใน envelope; ไม่เลือก route หรือขยายสิทธิ์เอง |
| Execution Supervisor | เริ่ม/ติดตาม/หยุด owned process tree หรือ HTTP stream | environment/argument safety, monotonic timeout, bounded buffers, 5+5 cleanup, process identity และ indeterminate result |

โครงสร้างที่เสนอเป็น single TypeScript package พร้อมโฟลเดอร์ตาม module และ adapters/mock, adapters/codex, adapters/claude, adapters/ollama หลีกเลี่ยง monorepo และ abstract repositories หลายชั้นก่อนมีเหตุผล ไม่มีการสร้างโครงสร้างนี้ในรอบวางแผน

Mock Adapter ใช้ interface เดียวกันแต่ profile namespace แยกจาก native ทุกชิ้นหลักฐานต้องระบุว่าเป็น mock/contract/native/human โดย test/evidence recorder ไม่ใช่เชื่อ flag passed ที่ adapter รายงานเอง production registry ต้องปฏิเสธ mock identity/profile เสมอ

### Adapter alternatives ที่ต้องพิสูจน์ใน spikes

| Adapter | Candidate | Alternative | คำถามที่บล็อกการเลือก |
| --- | --- | --- | --- |
| Codex | App Server ผ่าน structured stdio เพราะมี approval/identity events | exec JSON หากพิสูจน์ envelope และ identity ได้ครบโดยไม่ scrape terminal | exact version, model discovery เทียบ entitlement, subscription/API isolation, tool-network/credential enforcement, ephemeral footprint และ cancellation |
| Claude | TypeScript Agent SDK ที่ยังใช้ native engine พร้อม permission host | headless structured CLI และ adapter-owned permission host ที่ได้รับอนุญาต | SDK/auth compatibility, subscription profile ที่ไม่โหลด untrusted extensions, denial/stop behavior, inherited hooks/MCP, raw persistence; ห้ามสมมติว่า bare ใช้ subscription login ได้ |
| Ollama | HTTP streaming สู่ running local service ที่ตรวจรับแล้ว | CLI wrapper เพิ่ม subprocess/parsing แต่ไม่ได้พิสูจน์ locality ให้เอง | server no-cloud evidence, model digest/effective context, proxy/redirect behavior และ server-side cancellation uncertainty |

อ่าน [Codex App Server](https://developers.openai.com/codex/app-server/), [Codex exec](https://developers.openai.com/codex/noninteractive/), [Claude headless](https://code.claude.com/docs/en/headless) และ [Ollama FAQ](https://docs.ollama.com/faq) เพื่อสร้าง test hypotheses เท่านั้น ข้อมูลเอกสารไม่ใช่ conformance ของ binary/account/OS จริง

## Mock vertical slice ที่เล็กที่สุด

PH1-004–PH1-008 ต้องสาธิตใน temporary synthetic workspace และ private test store:

1. สร้าง Task แบบข้อความสั้น มี criterion อย่างน้อยหนึ่งข้อ พร้อม revision ที่ตรวจได้
2. เลือก mock route คงที่ แสดง context/readonly policy/limits และรับ explicit approval
3. เขียน admission และนับ attempt ก่อนเริ่ม mock; default 3 Runs/Task และ 15 นาที/Run
4. stream ข้อความที่ปลอดภัย แสดง reported mock identity และ usage ว่า simulated/unknown อย่างถูกต้อง
5. ยกเลิกหรือจบ Run แล้วมี outcome/journal; provider completion ไม่เปลี่ยนเป็น Task accepted
6. ปิด/เปิด CLI และเปิด Task เดิมได้ โดยไม่ใช้ approval/session เดิม ไม่ refund attempt และไม่ replay

M1 ยังไม่ต้องมี Auto, native discovery, edit, monetary reconciliation เต็มรูปแบบ, raw capture หรือ delete UI ที่สมบูรณ์ ความสามารถเหล่านี้ต้องปฏิเสธอย่างชัดเจนจนกว่าจะ implement ไม่แสดงว่า pass ส่วนขั้นต่ำที่ต้องมีตั้งแต่ slice คือ approval, readonly defaults, bounded/sanitized output, durable admission, attempt/time limits และ no retry

## Durable state protocol ที่เสนอ

ข้อเสนอนี้ต้องผ่าน PH1-012 และ D05 ก่อน native execution ไม่ใช่ผลพิสูจน์ atomicity:

1. ถือ Task lock และ workspace write lock ที่ canonical identity เมื่อมีสิทธิ์เขียน; ตรวจ context/policy/source/profile อีกครั้ง
2. คำนวณ usage/reservations จาก durable admissions ไม่เชื่อ derived index ที่อาจค้าง
3. เขียน Run admission record เดียวที่รวม attempt, reservation, approval binding และ context/profile fingerprints ให้ durable ก่อน dispatch; publish สำเร็จจึงเริ่ม provider
4. ใช้ immutable outcomes และ explicit reconciliation ที่อ้าง Run เดิม; ไม่บวก reservation ซ้ำกับ reconciled usage
5. crash ระหว่าง durable admission กับ dispatch หรือระหว่าง spawn กับ PID recording ถือว่าไม่แน่นอนและใช้ attempt ไปแล้ว; ไม่มี automatic retry/refund/kill จาก PID อย่างเดียว
6. startup อ่าน journal เพื่อสร้างสถานะภายในใหม่ได้ แต่การอ่าน journalไม่ใช่การ replay provider/tool side effects; unresolved admission/lock ต้องให้มนุษย์ทบทวน

ต้องทดสอบ partial write, fsync/rename failure, full disk, simultaneous instances, stale locks และ PID reuse บนทั้งสอง OS ไม่อ้าง network filesystem หรือ unsupported platform behavior เป็นสิ่งที่ตรวจรับแล้ว

## Human checkpoints และ execution authority

| Checkpoint | เมื่อใด | สิ่งที่ต้องอนุมัติ/หลักฐาน |
| --- | --- | --- |
| HC0 | ก่อน PH1-001/PH1-002 เริ่มงานจริง | อนุมัติขอบเขต implementation/spike; live experiment ต้องระบุ environment, provider/version/auth, synthetic data, destination, spending/attempt/time caps และ persistence แยกจากการอนุมัติแผน |
| HC1 | จบ M0 | รับ feasibility report, ตัดสิน D01–D05/D12 เท่าที่มีหลักฐาน, เลือกทางไปต่อแบบ mock-only หรือ native-capable; ไม่มี implicit scope waiver |
| HC2 | จบ M1 | รับ mock vertical slice เป็น development milestone และอนุมัติงาน controls ชุดถัดไป; ไม่เซ็น native conformance |
| HC3 | ก่อน native integration แต่ละครั้งหลัง M2 | ตรวจ policy/context/ledger/supervisor/privacy prerequisites และอนุมัติ test permit ที่เจาะจง; ไม่มี permission จาก mock result |
| HC4 | หลังผล native integration แต่ละ profile | ตรวจผลจริงเทียบ protocol/permissions/usage/persistence และรายการที่ยัง not tested; ไม่ขยายผลจาก auth/OS หนึ่งไปอีกชุด |
| HC5 | ก่อนงานเขียน PH1-021 | เลือก same coding adapter และ profiles สำหรับสอง OS จากหลักฐาน; อนุมัติ clean temporary Git workspace, edit/check envelope และงบ |
| HC6 | ก่อน PH1-026/PH1-027 | อนุมัติ qualification campaign สำหรับสิบ baseline rows และสอง OS edit demos โดยระบุ accounts/hosts/test costs/retention |
| HC7 | จบ PH1-028 | รับหลักฐาน A01–A40, matrix, limitations และ human Task acceptance; จึงพิจารณารับ MVP |

live feasibility/qualification เป็นการทดลองที่ได้รับอนุญาตใน test environment แยกจาก normal product routing ใช้ข้อมูลสังเคราะห์และ test credentials ที่ควบคุมได้ ไม่สร้าง shipped flag เช่น skip-safety เพื่อเรียก unqualified profile ปกติ registry ยังบล็อก native profile จนมีหลักฐานครบตามความสามารถที่เปิดใช้ การทดสอบที่ยังไม่ผ่านคือ not tested/failed ตามสเปก ไม่ใช่ passed with warning

ทุก live experiment ต้องได้รับ permit ของตัวเองหรืออยู่ใน scope/วงเงินที่อนุมัติชัดเจน การอนุมัติ milestone ไม่ใช่ consent สำหรับบัญชี/provider อื่น หาก test environment ไม่ปลอดภัยหรือไม่มี host/account ที่กำหนดให้หยุดการทดลองนั้น งาน offline ที่ไม่ขึ้นกับ blocker ยังเดินต่อได้

## Full MVP gate ที่คงไว้ทั้งหมด

| Target ID | OS / architecture | Provider | Auth / inference | สถานะปัจจุบัน |
| --- | --- | --- | --- | --- |
| T01 | Ubuntu 24.04 LTS x86_64 | Codex | Native subscription | not tested |
| T02 | Ubuntu 24.04 LTS x86_64 | Codex | First-party direct API | not tested |
| T03 | Ubuntu 24.04 LTS x86_64 | Claude Code | Native subscription | not tested |
| T04 | Ubuntu 24.04 LTS x86_64 | Claude Code | First-party direct API | not tested |
| T05 | Ubuntu 24.04 LTS x86_64 | Ollama | Local, cloud-disabled | not tested |
| T06 | macOS 15 Apple Silicon | Codex | Native subscription | not tested |
| T07 | macOS 15 Apple Silicon | Codex | First-party direct API | not tested |
| T08 | macOS 15 Apple Silicon | Claude Code | Native subscription | not tested |
| T09 | macOS 15 Apple Silicon | Claude Code | First-party direct API | not tested |
| T10 | macOS 15 Apple Silicon | Ollama | Local, cloud-disabled | not tested |

รวมสิบ baseline rows และงานเขียนจาก coding adapter ตัวเดียวกันอย่างน้อยหนึ่งตัวบนทั้งสอง OS แต่ละ write profile มีหลักฐานของตัวเอง ภายในแต่ละ row ต้อง pin exact provider/interface/model/config และทำ bounded task-class qualification; catalog หรือ CLI ติดตั้งสำเร็จไม่ใช่สิทธิ์ใช้โมเดลหรือหลักฐานคุณภาพ

PH1-026/PH1-028 ต้องตรวจ A01–A40 ตาม roles และ applicability ที่อนุมัติ พร้อมแยก mock/fault-injection/live evidence โดย A03 สำเร็จจริงทุก baseline row, A12/A16/A37/A39 ต้องมี actual native enforcement evidence, A11 ต้องพิสูจน์ locality และ A13 ต้องมีสอง OS demos หากขาด mandatory row ให้ release blocked จนมีหลักฐานหรือผู้ใช้อนุมัติแก้สเปกต่างหาก

## สถานะการตัดสินใจและขั้นถัดไป

ยังไม่เลือก exact package/provider/model versions, Codex/Claude transport, primary write adapter, store/lock protocol, tokenizer/reserves, money precision, PTY tooling, native persistence exceptions หรือ test budgets แทนผู้ใช้ รายละเอียด owner/checkpoint/วิธีพิสูจน์อยู่ใน [decision register D01–D12](phase-01-risks.md)

ขั้นถัดไปที่เสนอ: ตรวจเอกสารสามไฟล์นี้ แล้วอนุมัติขอบเขต PH1-001/PH1-002 ก่อนเริ่ม feasibility work จริง การอนุมัติเอกสารแผนอย่างเดียวไม่อนุญาตให้เขียน runtime หรือเรียก provider ต้องมี implementation authorization และ live permits ตาม HC0 อย่างชัดเจน
