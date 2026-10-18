# PH1-001 — Authorization, bounded proposal and review handoff

วันที่ 2026-10-18 — **เอกสารสำหรับ Draft PR / native feasibility BLOCKED / ยังไม่ปิด Work Item**

บันทึกรอบถัดมาในวันเดียวกัน: ผู้ใช้อนุมัติ P0-CX help/version 4 commands บน current host รวม Ubuntu 26.04 exploratory พร้อม commit/push/Draft PR ของ branch ใหม่แล้ว ดู [authorization และผล static inventory ล่าสุด](ph1-001-codex-local-inventory.md) ซึ่งหยุดก่อน invocation ตาม startup blocker (0/4) Authorization ledger และคำถาม P0 ด้านล่างเก็บเป็นประวัติรอบ proposal เดิม ไม่ได้ยกเลิกสิทธิ์ล่าสุด และยังไม่อนุญาต CODE-CX/LIVE-CX

ปัญหาที่ต้องตรวจคือ Codex App Server หรือ exec JSON สามารถรักษา fixed Run envelope ตาม Phase 1 ได้จริงหรือไม่ งานที่อนุญาตในรอบนี้ทำให้ขอบเขตและการทดลองถัดไปตรวจทานได้ก่อนเริ่ม process หรือใช้บัญชี provider ไม่เลือก transport จากเอกสารเพียงอย่างเดียว

## Scope และแหล่งข้อกำหนด

Work Item: **PH1-001 — Codex capability feasibility spike**; source branch `feature/PH1-001-codex-feasibility` → target `master` ใน `puennaja/puen-harness` ฐานหลัง fetch: `a123234047ec831d4c62a1e64dfb8b0a021396b8` ซึ่งเป็นทั้ง local `master` และ `origin/master` ตอนเริ่ม branch เอกสาร Codex/Claude preparation ที่มีในฐานเป็นงานเดิม ไม่ใช่ PH1-002 deliverable ใหม่ใน PR นี้

อ่านก่อนเริ่ม: [phase index](../phases/README.md), [foundation](../phases/phase-01-foundation.md), [A01–A40](../phases/phase-01-acceptance.md), [Q1–Q34](../phases/phase-01-review.md), [CONTEXT](../../CONTEXT.md), ADR [0001](../adr/0001-preserve-provider-agent-loops.md)/[0002](../adr/0002-task-context-authority.md), [plan](phase-01-plan.md), [work items](phase-01-work-items.md), [risks/D01–D12](phase-01-risks.md), [Codex capability/tests CX-01–CX-12](ph1-001-codex-feasibility.md) และ [shared preflight](phase-01-feasibility-preflight.md)

คำสั่งผู้ใช้รอบนี้กำหนดให้ Implementation Agent ส่ง Draft PR ก่อนการ review โดย Wit และให้ผู้ใช้ถือ final approval ไม่ได้อ้างว่า Human Implementation/Verification Gates หรือ independent review ผ่านแล้ว ขั้นตอน publication นี้ได้รับอนุญาตโดยตรงจาก first assignment ไม่ใช่การอนุมานจาก implementation approval

## Authorization ledger

| Activity | สถานะสิทธิ์ปัจจุบัน | ขอบเขต/เหตุผล |
| --- | --- | --- |
| อ่านเอกสารและ Git metadata | AUTHORIZED | first assignment และ approved preparation; จำกัด repository นี้ |
| ตรวจ static host facts และค้น binary path โดยไม่เริ่ม provider | AUTHORIZED / ทำแล้วตาม evidence ด้านล่าง | อ่าน OS/architecture และ shell path lookup เท่านั้น; ไม่อ่าน provider configuration/auth state |
| เขียน documentation ของ PH1-001 | AUTHORIZED | bounded proposal, mappings, blockers และ review handoff; ไม่แก้ approved specs/ADRs หรือ PH1-002 |
| บันทึก workflow ใน `AGENTS.md` | AUTHORIZED | ผู้ใช้กำหนด GitHub PR workflow “from now on”; operational instructions ไม่เปลี่ยน product requirements |
| สร้าง feature branch และ scoped local commit | AUTHORIZED | first assignment ระบุ branch pattern และ commit convention ชัดเจน |
| Push feature branch / เปิด Draft PR เข้า `master` | AUTHORIZED แยกทั้งสอง action | first assignment สั่งทั้ง commit/push และ Draft PR; ใช้ existing GitHub transport authentication เท่านั้น ไม่อ่าน/แสดง token values และไม่แตะ provider credentials |
| เริ่ม Codex CLI รวม version/help/schema/auth commands | NOT APPROVED / NOT RUN | HC0-LOCAL ใน shared preflight ยังไม่ได้รับคำตอบอนุมัติ; startup อาจโหลด config หรือเขียน native state |
| สร้าง spike runner, observer, executable fixtures หรือ runtime code | NOT APPROVED / NOT IMPLEMENTED | ยังไม่มี explicit HC0-CODE permission; assignment ให้ทำ proposal หากยังไม่มีสิทธิ์ code |
| Live metadata/discovery/inference, ใช้ provider credentials, subscription/API billing | NOT APPROVED / NOT RUN | ต้องมี explicit per-profile data/auth/billing/persistence permit; publication permission ไม่ใช่ provider permit |
| เปลี่ยน sandbox/OS policy/install dependencies/เปิด listener | NOT APPROVED | ต้องระบุ necessity และขอสิทธิ์แยก; ไม่มี bypass หรือ silent fallback |
| Merge/close/self-approve/mark ready, PH1-002 หรือ PH1-003+ | OUT OF CURRENT SCOPE | หยุดหลังเปิด Draft PR; Wit review pending; Human Gate Owner ตัดสินขั้นถัดไป |

## Actual local evidence — ไม่ใช่ native conformance

| Check / command ที่รันจริง | Result | ขอบเขตของข้อสรุป |
| --- | --- | --- |
| `uname -s` / `uname -m` | PASSED: `Linux` / `x86_64` | environment ที่ execute commands นี้เท่านั้น ไม่ยืนยันเครื่องอื่นของผู้ใช้ |
| `cat /etc/os-release` | PASSED: `Ubuntu 26.04.1 LTS`, `VERSION_ID=26.04` | **ไม่ตรง mandatory Ubuntu 24.04 LTS target**; ไม่ใช้ผลเครื่องนี้ fill T01/T02 และไม่เปลี่ยน target |
| `command -v codex` | PASSED: พบ `/usr/local/bin/codex` ใน PATH | lookup เท่านั้น ยังไม่ยืนยันว่าเป็น supported binary/version และไม่ execute path นี้ |
| `git fetch --no-tags origin master` และ ref comparison | PASSED: local/remote base ตรง SHA ด้านบน | branch สร้างจาก latest fetched master; ไม่มี push เข้า master |
| GitHub repo/default branch และ duplicate PR preflight | PASSED: `puennaja/puen-harness`, target `master`; ไม่พบ open PR ของ source ตอน preflight | ใช้ existing GitHub session สำหรับงาน Git ที่อนุญาต ไม่ใช่ native provider auth proof |
| Read-only Python documentation check และ `git diff --check` | PASSED | จำกัด diff/untracked เป็นสามไฟล์ที่ระบุด้านล่าง, local links ใช้ได้, whitespace/newlines ถูกต้อง, ไม่พบ private-key/API-key patterns ที่ตรวจ, mapping 13 ข้อและ CX designs 12 ข้อครบ; ไม่ใช่ all-secret detection guarantee |
| Codex version/help/schema/App Server/exec | NOT RUN — permission pending | installed version, actual interfaces และ effective configuration ยัง UNKNOWN |
| Native behavior / T01, T02, T06, T07 | BLOCKED / NOT TESTED | ไม่มี provider request, auth inspection, measured usage หรือ actual sandbox denial |
| Wit review / Human acceptance | NOT RUN / PENDING | PR ต้องส่งเพื่อ review ก่อน ห้ามกล่าวว่า reviewed/approved แล้ว |

host mismatch เป็น blocker ของ qualification ไม่ใช่ failure ของ Codex capability เพราะยังไม่ได้ทดลอง หากผู้ใช้อนุญาต exploratory work บน Ubuntu 26.04 ภายหลัง ต้องบันทึกเป็น non-target evidence และทดสอบ mandatory targets แยก

ไฟล์ที่เปลี่ยน: `AGENTS.md`, `docs/implementation/ph1-001-codex-feasibility.md` และไฟล์ proposal นี้ ตรวจ `git diff --exit-code a123234047ec831d4c62a1e64dfb8b0a021396b8 --` สำหรับ approved phase specs, ADRs, CONTEXT, implementation plan/work-items/risks, shared preflight และ PH1-002 ได้ exit 0 ไม่มีการแก้ไฟล์เหล่านั้น `AGENTS.md` เพิ่มเฉพาะ operational workflow ที่ผู้ใช้กำหนด การตรวจลิงก์เป็น local-path validation ไม่ได้ทดสอบ endpoint ภายนอก

## Bounded next-step proposals — รออนุมัติแยกกัน

### P0-CX — Local contract inventory

ขออนุมัติเฉพาะ **CX-01 บางส่วน** บน host ที่ผู้ใช้ระบุ เลือก Ubuntu 24.04 x86_64/macOS 15 Apple Silicon หรืออนุญาต environment ปัจจุบันเพื่อ exploratory inventory อย่างชัดเจน ต้องยืนยัน executable provenance และวิธี safe startup/config isolation ก่อนเริ่ม

รายการ command ที่เสนอ: `codex --version`, `codex --help`, `codex app-server --help`, `codex exec --help` ใช้ absolute executable path ที่ตรวจแล้ว ไม่ใช้ shell alias/wrapper ที่ไม่ทราบที่มา จำกัด **4 invocations รวม**, **10 วินาทีต่อ command**, sequential, no retry; หาก cleanup จำเป็นให้ใช้ 5s graceful + 5s forced เฉพาะ owned process tree ค่า limits ต้องบันทึกใน permit ก่อนทำ ไม่ execute commands จาก proposal นี้อัตโนมัติ

ไม่มี prompt, inference, account RPC, `login status`, login/logout/token refresh, remote model discovery หรือ schema generation ใน permit ย่อยนี้ ไม่มีสิทธิ์อ่าน credential/config home จริง หาก safe startup ต้องใช้ dedicated config root หรือ native artifacts ที่จะเขียนยังไม่ทราบ ให้หยุดแล้วเสนอ filesystem/persistence scope ใหม่ก่อน การรับรอง provenance จาก path lookup อย่างเดียวไม่พอ และไม่มี full raw stdout/stderr capture โดยปริยาย

Deliverable หลังอนุมัติ: exact version/interface/flags inventory และ sanitized evidence; ไม่ถือว่า identity, permissions, costs หรือ retention ผ่านเพียงเพราะ help มี flags

### CODE-CX — Disposable offline spike code

ขออนุมัติภายหลัง P0 เพื่อสร้างเฉพาะ bounded structured-protocol driver และ observer ของ PH1-001 ใน location ที่ผู้ใช้รับ ไม่สร้าง full Harness runtime หรือ PH1-004 bootstrap

ขอบเขตที่เสนอ: parse framed JSON/JSONL จาก synthetic streams, reject missing/mismatched identity หรือ terminal, explicit deny/end state, bounded buffers/deadlines และ no-retry invocation contract โดย **default tests offline เท่านั้น** กำหนด exact paths, pinned tooling/dependencies และ offline check commands หลังรู้ installed interface ก่อนขอ code permission ไม่มี dependency install/provider execution/credential access รวมอยู่ด้วย เวอร์ชัน runtime และ parser schema ยังไม่เลือกเพราะ D01/D03 pending

Offline expectations: malformed/partial/oversized events, usage missing/duplicate, denial followed by extra work, terminal absent และ synthetic reroute ต้องมีผลที่ตรวจได้ แยก simulated evidence จาก native behavior ผลเขียวของ driver ไม่ qualify native profile; local process cleanup ยังต้องมี owned-process tests และ native tests ต่างหากเมื่อได้รับอนุญาต

### LIVE-CX — First Codex text/identity smoke

ขออนุมัติ **CX-02 หนึ่ง Run เท่านั้น** หลัง P0/required controls พร้อม: หนึ่ง exact OS/interface/version/auth/model/endpoint/profile; ข้อความไทย synthetic self-contained ≤4 KiB; output สั้นตาม verified interface capability; no requested tools/edits, no tool network, extensions disabled ตามที่พิสูจน์ได้ ไม่มี Claude Run รวมอยู่ใน proposal นี้

เพดานที่เสนอ: **1 admitted Run / 60 วินาทีรวม waits / cleanup 5+5 วินาที / no driver retry** อยู่ภายใน defaults 3 Runs/Task และ 15 นาที/Run ไม่ขยาย defaults ไม่ถือว่าหนึ่ง native invocation เท่ากับหนึ่ง API request; native internal retries/cost uncertainty ต้อง disclose/bound เท่าที่รองรับ

Permit fields ที่ยัง **PENDING**: host และ binary/version, App Server หรือ exec, subscription หรือ direct API, account/billing reference ที่ไม่เผย secret, concrete requested model/reported-identity provenance, capacity/output reserve, provider endpoints/data authorization, finite monetary allowance หรือ explicit exact-profile unknown-cost permission, private roots/native artifact footprint/raw-persistence consent, observer/stop rights และ durable admission record ไม่มีค่าที่เลือกแทนผู้ใช้หรือสิทธิ์ใช้บัญชีที่เดาเอา

ถ้า field ที่จำเป็นไม่ครบ ให้ BLOCKED ก่อน invocation; unexpected config/auth/destination/model/extension/persistence change ทำให้ permit ใช้ต่อไม่ได้ No retry/fallback, no session-wide grants, no resume/fork และไม่ใช้ API แทน subscription หลัง auth failure

CX-03–CX-12, positive/negative model probes เพิ่ม, network endpoint, filesystem/credential/extension tests, context-overflow และ cancel-during-tool trials ต้องแยก case IDs/attempts/budget/observer ใน permit ใหม่ ไม่มี live test ใดอนุมัติจากการ merge documentation PR

## Acceptance coverage — mapping เท่านั้น ไม่มี native pass

| Requirement ใน PH1-001 | Test design ที่อ้างได้ | สถานะหลักฐาน/งานที่ยังต้องทำ |
| --- | --- | --- |
| A03 | CX-02, CX-03 | D/PLANNED; positive analysis และ unsupported model cases ต่อ mandatory profile ยัง NOT RUN |
| A04 | CX-02, CX-03, CX-09 | requested/reported/missing/mismatch/reroute กำหนดไว้; actual identity signal UNKNOWN; injected reroute ไม่ใช่ N |
| A09 | CX-01, CX-03, CX-10 | version/config/auth/billing/persistence invalidation ระบุใน permit; native observation NOT RUN |
| A12 | CX-04, CX-07 | actual denied write/side effect/outside read/network/fake credential tests ยัง NOT RUN |
| A15 | CX-05 | deny + end + fresh approval; denial ที่ให้ agent continue ไม่ผ่าน; native race/control ยัง UNKNOWN |
| A16 | CX-06, CX-12 | unapproved hooks/MCP/plugins/memory/agents ปิดและ no unsandboxed retry; enforcement NOT RUN |
| A26 | CX-02, CX-09 | measured/estimated/unknown/source/units ไม่รวมเป็น invoice truth; usage NOT RUN |
| A27 | CX-08 | 5s+5s cleanup ระหว่าง stream/tool/approval และ timeout; actual timing NOT RUN |
| A28 | CX-08 | uncertain cleanup → indeterminate; ห้ามอ้าง rollback/remote billing stop; observation NOT RUN |
| A29 | CX-05, CX-09 | failure/partial terminal/no retry/no fallback; driver ยังไม่สร้างและ native NOT RUN |
| A34 | CX-06, CX-10 | footprint แยก auth/OS; no-transcript ไม่ใช่ zero artifacts; native inventory NOT RUN |
| A37 | CX-01, CX-04, CX-12 | missing dependency/unqualified config ต้อง fail closed; static OS observation ไม่ใช่ sandbox proof |
| A39 | CX-04, CX-07 | actual path/symlink/indirect script/tool-env/network denial ยัง NOT RUN; script ต้องถูก attempt จริง |

รายละเอียด expected observations ของ CX-* อยู่ใน [Codex preparation](ph1-001-codex-feasibility.md) ไม่คัดลอกเป็น acceptance contract ใหม่ A01/A10/A17/A20–A22/A33 เป็น supporting mappings ใน test designs; ส่วน A01–A40 ทั้งหมดและ Q1–Q34 ยังเป็นไปตาม approved sources ไม่มี mandatory target ถูกลดเป็น N/A

## Architecture decisions, alternatives และ risks

- **ยังไม่เลือก transport (D03 / PR01/PR05):** App Server มี bidirectional lifecycle/approval surface ที่น่าตรวจ แต่เพิ่ม protocol/supervision work; exec JSON เริ่ม process ง่ายกว่า แต่ denial/identity/control อาจไม่พอ ต้องใช้ version/profile evidence ตัดสิน ไม่ใช้ terminal scraping
- **แยก policy/Run authority จาก native transport (PR04):** driver ภายหลังต้อง deny และ stop เมื่อ expansion; permission response ไม่ได้อนุญาต session-wide grant หรือแก้ immutable envelope
- **แยก docs/offline/native evidence (PR12):** ไม่มี guessed capability flags/qualification registry entry; unsupported/unknown ปิด affected native path แทน bypass
- **Host availability (D09 / PR03):** environment ปัจจุบันไม่ใช่ mandatory Ubuntu target; ยังไม่ตรวจ access ของ Ubuntu 24.04/macOS 15 หรือ subscription/API accounts ต้องให้ผู้ใช้จัดหา/อนุญาต
- **Persistence/credentials (D11 / PR02/PR10):** CLI startup เองอาจมี artifacts; dedicated config ไม่พิสูจน์ auth/tool isolation; unknown/unapproved footprint บล็อก live ไม่ purge shared provider homes
- **Cost/context (D07/D08/D10 / PR07/PR08):** pending concrete model/capacity/billing/allowance; ไม่มี request-count/financial ceiling guarantee; missing usage ไม่เป็นศูนย์

ผล ณ ตอนนี้: **authorized documentation delivery พร้อม review; native spike ยัง blocked** การ merge PR ไม่ปิด PH1-001 เป็น verified ไม่เริ่ม PH1-002 และไม่ใช่ MVP qualification; T01/T02/T06/T07 ยัง not tested และ ten-row gate/same-adapter two-OS edit demos คงเดิม

## Human decisions และ review handoff

1. อนุมัติ P0-CX สี่ local CLI invocations หรือไม่ และเลือก mandatory target host หรืออนุญาต Ubuntu 26.04 exploratory inventory? การอนุมัติ documentation/push/PR รอบนี้ยังไม่ตอบคำถามนี้
2. หลังได้ version/interface inventory จะอนุมัติ CODE-CX ใน paths/tooling ใด? หากยังไม่อนุมัติ ให้คง document-only
3. สำหรับ LIVE-CX จะเลือก auth/billing/model/host/profile ใด และ allowance/unknown-cost/persistence consent เท่าใด? ต้องได้ permit ครบและ explicit approval ก่อน live ไม่ถือ subscription ว่าฟรี

Wit: **review pending** ไม่มี feedback หรือ review verdict ที่ได้รับแล้วในรอบนี้ ส่ง Draft PR เพื่อให้ Wit ตรวจ authorization boundaries, mapping completeness, host mismatch และข้อเสนอ transport/negative-test evidence; ยังไม่ระบุ GitHub reviewer account เพราะผู้ใช้ยังไม่ได้ให้ mapping ของ Wit

Human Gate Owner: ตัดสิน proposal, ขอบเขต/สิทธิ์เพิ่มเติม และ final merge approval Implementation Agent หยุดหลังเปิด Draft PR ตาม assignment หากมี feedback ภายหลัง ให้แก้ภายใน scope บน branch เดิมและบันทึก actual rechecks; ประเด็นที่ขยาย scope ต้องให้ผู้ใช้ตัดสินก่อน
