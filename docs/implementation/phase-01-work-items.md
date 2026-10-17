# Phase 1 — Work Items

สถานะ: **Planned / รออนุมัติ implementation** — 2026-10-17 ไม่มีรายการใดเริ่มพัฒนา ทดสอบ provider หรือผ่าน qualification แล้ว

อ่าน [แผนและ checkpoints](phase-01-plan.md), [risks/decisions](phase-01-risks.md) และ [acceptance contract ที่อนุมัติ](../phases/phase-01-acceptance.md) ร่วมกัน รหัส PH1-* เป็นรหัสวางแผนภายใน ไม่ใช่ tracker issue ที่สร้างไว้แล้ว ผู้รับผิดชอบ implementation คือผู้พัฒนาที่จะได้รับมอบหมาย ส่วนการตัดสิน scope/permissions/MVP acceptance เป็นของผู้ใช้

## วิธีอ่านและเกณฑ์จบงาน

ชนิดหลักฐาน: D = documentation/design inspection, L = local filesystem/process/core tests, M = mock/recorded-event contract tests, N = live native provider บน exact profile/OS ที่อนุญาต, H = human review ไม่มี D/L/M ประเภทใดแทน native conformance และ H ไม่ได้เพิ่ม independent verifier ที่เลื่อนไป Phase 2

ทุก item ต้องมีผลตรวจของตัวเอง พร้อม command/environment/profile/fixture references ตามที่ใช้จริง หากยังรันไม่ได้ให้ระบุ not run และ blocker ไม่ใส่ passed หาก code/local checks เสร็จแต่ N/H ที่ item ต้องใช้ยังขาด ให้แยกสถานะ implementation complete / verification pending และห้ามปิดเป็น verified/qualified ส่วน native capability ยังไม่ถือว่า qualified จนมี N evidence ตามสเปก Spikes จบด้วยผลว่า unsupported/unknown ได้ แต่ต้องบล็อกงาน native ที่พึ่งพาความสามารถนั้น

dependency คือ prerequisite ของ item ไม่ใช่ลำดับตามเลข ID ทุก item ยังคงสถานะ Planned ตารางนี้เป็นแหล่ง dependency หลักของแผน

## Dependency register

| ID | Work item | Depends on | Milestone |
| --- | --- | --- | --- |
| PH1-001 | Codex capability feasibility spike | none | M0 |
| PH1-002 | Claude capability feasibility spike | none | M0 |
| PH1-003 | Stack, module interfaces และ feasibility decision | PH1-001, PH1-002 | M0 |
| PH1-004 | TypeScript/test bootstrap และ contract fixtures | PH1-003 | M1 |
| PH1-005 | Task Context และ private store ขั้นต่ำ | PH1-004 | M1 |
| PH1-006 | Mock Adapter และ typed event contract | PH1-004 | M1 |
| PH1-007 | Run Controller และ admission ขั้นต่ำ | PH1-005, PH1-006 | M1 |
| PH1-008 | Minimal terminal vertical slice | PH1-007 | M1 |
| PH1-009 | Policy/profile และ approval authority | PH1-007 | M2 |
| PH1-010 | Workspace identity, clean-Git gate และ locks | PH1-005, PH1-009 | M2 |
| PH1-011 | Selected-file context, schema/freshness และ limits | PH1-005, PH1-009, PH1-010 | M2 |
| PH1-012 | Durable admissions, crash และ reconciliation protocol | PH1-007, PH1-010 | M2 |
| PH1-013 | Cost/usage admission และ cumulative budgets | PH1-009, PH1-012 | M2 |
| PH1-014 | Process/stream supervision และ failure handling | PH1-006, PH1-009, PH1-012 | M2 |
| PH1-015 | Redaction, safe terminal streams และ persistence gate | PH1-008, PH1-009, PH1-012 | M2 |
| PH1-016 | Local diagnostics, discovery และ identity registry | PH1-009, PH1-011, PH1-013, PH1-014, PH1-015 | M2 |
| PH1-017 | Codex baseline adapter integration | PH1-001, PH1-010, PH1-016 | M3 |
| PH1-018 | Claude baseline adapter integration | PH1-002, PH1-010, PH1-016 | M3 |
| PH1-019 | Ollama local text adapter integration | PH1-010, PH1-016 | M3 |
| PH1-020 | Static Auto, eligibility และ bounded task fixtures | PH1-016, PH1-017, PH1-018, PH1-019 | M3 |
| PH1-021 | Approved-edit envelope, diff/check evidence | PH1-010, PH1-011, PH1-013, PH1-014, PH1-015, PH1-023 | M4 |
| PH1-022 | Full command/lifecycle และ revision-bound acceptance | PH1-008, PH1-011, PH1-013, PH1-020, PH1-021, PH1-023 | M4 |
| PH1-023 | Native footprint consent, raw capture และ owned deletion | PH1-005, PH1-009, PH1-010, PH1-012, PH1-015 | M4 |
| PH1-024 | Cross-feature failure integration | PH1-012, PH1-013, PH1-014, PH1-015, PH1-017, PH1-018, PH1-019, PH1-020, PH1-021, PH1-022, PH1-023 | M4 |
| PH1-025 | Package/setup และ two-OS local smoke | PH1-022, PH1-024 | M4 |
| PH1-026 | Ten-profile baseline qualification | PH1-020, PH1-023, PH1-024, PH1-025 | M5 |
| PH1-027 | Same-adapter approved-edit demos บนสอง OS | PH1-021, PH1-022, PH1-026 | M5 |
| PH1-028 | Evidence audit และ human MVP gate | PH1-026, PH1-027 | M5 |

PH1-021 มี prerequisite เพิ่มแบบเลือกตามหลักฐาน: ต้องผ่าน read-only integration ของ PH1-017 **หรือ** PH1-018 สำหรับ coding adapter ที่ HC5 เลือก และต้องยืนยัน write-capable candidate บนทั้งสอง OS ห้ามเดาว่า Codex หรือ Claude จะเป็นผู้ชนะ อีก adapter ยังต้องผ่าน baseline matrix ของตัวเอง

## รายละเอียดงานที่ตรวจรับแยกได้

### PH1-001 — Codex capability feasibility spike

- Deliverable: เปรียบเทียบ App Server กับ exec JSON สำหรับ subscription/API บนสอง OS ระบุ identity/discovery เทียบ entitlement, permission/approval transport, filesystem/network/credentials, inherited extensions, usage, native retention และ stop behavior พร้อม supported/unsupported/unknown ต่อข้อ
- ตรวจรับ: ทุกข้อมีเอกสารหรือ bounded reproducer ระบุ exact version/profile; failure มีหลักฐานและผลต่อ gate ไม่มี terminal scraping หรือ assumption จาก flags หากไม่มีสิทธิ์/host ให้บันทึกเป็น unknown ไม่อ้างว่าทดลองแล้ว
- Evidence: D; N เฉพาะ live permit ของ HC0 ใช้ synthetic data/test credentials และวงเงินที่ระบุ งานนี้ไม่ทำ production adapter และไม่ประกาศ conformance
- Mapping: A03, A04, A09, A12, A15, A16, A26–A29, A34, A37, A39 / Risks: PR01, PR02, PR04, PR05, PR10 / Checkpoint: HC0 → HC1

### PH1-002 — Claude capability feasibility spike

- Deliverable: เปรียบเทียบ TypeScript SDK กับ structured headless/permission host แยก subscription/API และสอง OS ตรวจ tool availability เทียบ approval, extension isolation, unsandboxed escape, identity substitution, usage, persistence และ shutdown hooks
- ตรวจรับ: แสดงวิธีคง subscription auth พร้อมปิด unapproved configuration ที่พิสูจน์ได้ หรือรายงาน blocker ห้ามใช้ bare mode แล้วสรุปว่า subscription ใช้งานได้; unsupported envelope ต้องถูกปฏิเสธ
- Evidence: D; N เฉพาะ HC0 permit ไม่มีการใช้บัญชี/provider จริงในรอบเขียนแผนนี้
- Mapping: A03, A04, A09, A12, A15, A16, A26–A29, A34, A37, A39 / Risks: PR01, PR02, PR04, PR05, PR10 / Checkpoint: HC0 → HC1

### PH1-003 — Stack และ interfaces

- Deliverable: ข้อสรุป D01–D05/D12 ที่ตัดสินได้ พร้อม interface responsibilities, contract/error taxonomy, evidence classes และรายการที่ยังบล็อก native execution ไม่แก้ ADR/สเปกเดิมโดยปริยาย
- ตรวจรับ: เลือก runtime/UI/schema/tests/store approach จาก trade-offs และ spike results; หาก native transport ยังไม่พิสูจน์ ต้องแยกสิทธิ์ทำ mock-only ออกจากสิทธิ์ทำ integration; ทุก unresolved choice มี owner/checkpoint
- Evidence: D/H ไม่มี model/version/rank ที่แต่งขึ้น / Mapping: A04, A08, A17, A29, A30, A34, A37 / Risks: PR01, PR13, PR14, PR15 / Checkpoint: HC1

### PH1-004 — Bootstrap และ fixture contracts

- Deliverable: หลังได้รับอนุญาตจึงสร้าง TypeScript package, pinned dependencies, build/typecheck/local test commands, versioned synthetic fixtures และ evidence metadata contract
- ตรวจรับ: clean install/build/typecheck/test ใน approved development environment; fixture ทุกชุดระบุ simulated ไม่มี provider discovery/inference ใน default tests ไม่มี native route enabled
- Evidence: L/M / Mapping: A19, A29, A38 / Risks: PR12, PR14 / Checkpoint: HC1 implementation scope

### PH1-005 — Task Context/private store ขั้นต่ำ

- Deliverable: canonical JSON revisions, generated Markdown, stable criterion/constraint IDs, private test-store access และ Task reopen สำหรับ text-only slice
- ตรวจรับ: draft ไม่ใช่ approved revision; model proposal เปลี่ยน approved fields ไม่ได้; unsupported schema ถูกปฏิเสธ; reload คืน requirements ไม่คืน approval/native conversation ข้อมูลเขียนใน private store ไม่ใช่ target repo
- Evidence: L/M / Mapping: A17–A19, A33, A36 / Risks: PR07, PR09, PR10 / Checkpoint: HC2

### PH1-006 — Mock Adapter/event contract

- Deliverable: scripted mock start/output/approval/usage/error/completion พร้อม malformed/partial events และ controllable clock ไม่มี process/network/provider side effects
- ตรวจรับ: caller ใช้ interface เดียวกับ adapters อื่น แต่ mock profile/evidence แยก namespace; missing terminal event ไม่กลายเป็น completed; output ไม่มีอำนาจอนุมัติหรือเปลี่ยน route
- Evidence: M / Mapping: A04, A15, A26–A29, A38 / Risks: PR05, PR11, PR12 / Checkpoint: HC2

### PH1-007 — Run Controller/admission ขั้นต่ำ

- Deliverable: preview → explicit approval → durable admission → mock execution → terminal reason; readonly default, หนึ่ง active Run/instance, 3 Runs/Task, 15 นาที/Run รวมเวลารอ
- ตรวจรับ: dispatch spy ไม่ถูกเรียกก่อนบันทึก admission; reject ไม่กิน attempt หากยังไม่ invoke; admitted failure ไม่ refund; scope expansion จบเป็น needs-approval; Task-bound provider/model และ fresh session semantics คงเดิม
- Evidence: L/M / Mapping: A08, A15, A17, A23, A27, A30, A36 / Risks: PR04, PR08, PR09, PR12 / Checkpoint: HC2

### PH1-008 — Mock terminal vertical slice

- Deliverable: minimal Ink/CLI ที่ผ่าน HC1 เชื่อม Task creation/open, fixed mock route, preview/approve, stream/status/context/budget inspection, cancel/exit และ private journal
- ตรวจรับ: สาธิต flow ใน plan ครบ รวม restart ไม่มี replay; ป้อนข้อความกลาง Run ไม่เริ่มงานเพิ่ม; output plain escaped/bounded; native/Auto/edit/unsupported commands ถูกปฏิเสธ ไม่ซ่อนว่าเป็น mock
- Evidence: L/M/H / Mapping: A02, A03, A17, A23, A27, A33, A36, A38 แบบ simulated / Risks: PR11, PR12, PR13 / Checkpoint: HC2 — development milestone เท่านั้น

### PH1-009 — Policy/profile authority

- Deliverable: user ceiling, approved project snapshot, restrictive intersection, profile fingerprints, metadata permits และ fixed Run envelope ที่ runtime validate จริง
- ตรวจรับ: repository/model/CLI inputs ขยายสิทธิ์ไม่ได้; auth/endpoint/billing/persistence/config เปลี่ยนแล้ว approval เก่าใช้ไม่ได้; ไม่มี remote probe ก่อนอนุญาต; unqualified enforcement ไม่มี fallback
- Evidence: L/M; native proof ใน PH1-017/PH1-018/PH1-026 / Mapping: A08–A10, A12, A15, A16, A37 / Risks: PR01, PR02, PR04, PR10 / Checkpoint: HC3

### PH1-010 — Workspace/locks

- Deliverable: canonical identity, read-only Git inspection ที่ไม่รัน unapproved hooks/drivers, clean-Git write admission, Task lock และ shared canonical-workspace write lock
- ตรวจรับ: staged/unstaged/untracked work บล็อก write โดยไม่ clean/stash/reset; read-only non-Git ทำได้; two-instance/path-alias conflict ถูกปฏิเสธ; required input เปลี่ยนระหว่าง preview/start ยกเลิก approval; ไม่สรุปว่า startup path check ป้องกัน native symlink escape ได้แล้ว
- Evidence: L/M; actual confinement ต้อง N / Mapping: A14, A19, A32, A39, A40 / Risks: PR04, PR07, PR09 / Checkpoint: HC3

### PH1-011 — Context preparation

- Deliverable: explicit selected-file manifest, fingerprints/freshness/sensitivity, schema/workspace validation, qualified context capacity และ disclosed estimation/reserve method
- ตรวจรับ: Thai/Unicode/spaces ปลอด shell interpolation; required stale refs บล็อก optional unused refs ระบุสถานะ; over-budget ไม่ truncate; unknown required capacity ไม่ถูก manual/Auto bypass; ไม่มี relevance crawl/LLM drafting เบื้องหลัง
- Evidence: L/M / Mapping: A06, A19–A22, A40 / Risks: PR07 / Checkpoint: HC3; D07 ต้องปิดก่อนใช้ model จริง

### PH1-012 — Durable state/crash protocol

- Deliverable: transaction/admission protocol ของ plan พร้อม lock integration, immutable outcomes, reconstruction/reconciliation และ incomplete-evidence handling
- ตรวจรับ: inject full-disk/partial-write/crash ทุกช่วงก่อน/หลัง dispatch แล้วไม่มี invocation ก่อน durable admission ไม่มี automatic refund/replay; unresolved reservation/lock คงอยู่; stale/reused PID ไม่ถูกฆ่าจาก PID อย่างเดียว
- Evidence: L/M บนสอง OS สำหรับ filesystem/process behavior / Mapping: A23, A24, A28, A30–A32 / Risks: PR08, PR09 / Checkpoint: HC3; D05 ต้องมี proof

### PH1-013 — Budget/usage admission

- Deliverable: per-Task attempt/time/money accounting, outstanding reservations, explicit reconciliation, known/estimated/unknown sources และ auth/billing-specific unknown-cost policy
- ตรวจรับ: 0.05 + 0.06 > 0.10 บล็อก; restart/duplicate usage ไม่ double count/refund; missing final usage ไม่เป็นศูนย์; billing change invalidate permission; ใช้ exact monetary representation ที่ HC3 รับ ไม่ใช้ floating-point รวมเงินโดยไม่ระบุ rounding
- Evidence: L/M / Mapping: A07, A09, A23–A26 / Risks: PR08 / Checkpoint: HC3; ไม่มี global/daily caps หรือ financial ceiling claim

### PH1-014 — Execution supervision

- Deliverable: subprocess/HTTP lifetime, bounded events, normal/forced cancellation, timeout/unknown cleanup และ typed errors ใช้ synthetic local executables/streams ก่อน provider
- ตรวจรับ: 5 วินาที graceful + อีก 5 วินาที forced; 15-minute Run รวม approval waits; no retry/fallback; missing completion/partial JSON/overflow ไม่เป็น success; cleanup ไม่แน่นอนบล็อก conflicting work ไม่รับประกัน rollback/remote billing stop
- Evidence: L/M; actual native/OS cleanup ใน N / Mapping: A22, A27–A31, A38 / Risks: PR01, PR08, PR09, PR11 / Checkpoint: HC3

### PH1-015 — Privacy และ safe output

- Deliverable: allowlisted metadata journal, fake-secret/PII redaction, safe streaming/escape handling, payload/event bounds และ gate ป้องกัน raw/native persistence ที่ยังไม่อนุมัติ
- ตรวจรับ: credentials/PII markers ไม่ตก default storage/error; split OSC/control sequences ไม่เปลี่ยน clipboard/title/approval; ไม่มี persistent full history/source/output โดยปริยาย; unknown native footprint บล็อก operational use
- Evidence: L/M; footprint ต้อง N แยก profile / Mapping: A33, A34, A38 / Risks: PR10, PR11 / Checkpoint: HC3; raw opt-in interface ยังรอ PH1-023

### PH1-016 — Diagnostics/discovery/identity

- Deliverable: แยก installed/authenticated/discoverable/qualified/eligible; curated metadata provenance; local-only inspection และ permit-bound remote discovery; requested/reported identity normalization
- ตรวจรับ: absent provider เป็น diagnostic ไม่ crash; unsupported model error ไม่ substitute; catalog ไม่ถูกอ้างเป็น entitlement; missing/mismatch/reroute หยุด; unknown mandatory metadata ไม่แต่ง; mock records เข้าสู่ native registry ไม่ได้
- Evidence: L/M; live discovery ต้อง HC3 permit / Mapping: A01, A03, A04, A06, A09, A10, A37 / Risks: PR01, PR02, PR05, PR12 / Checkpoint: HC3

### PH1-017 — Codex baseline adapter

- Deliverable: selected structured transport, fresh session, readonly envelope, native approval handling, identity/usage/error mapping และ dedicated effective configuration แยก subscription/API
- ตรวจรับ: adapter contract tests ผ่านพร้อม bounded controlled integration ต่อ target/profile ที่ได้รับอนุญาต; extension/network/credential/persistence unknown ยังไม่เปิด normal route ไม่มี native resume หรือ approval ข้าม Run
- Evidence: M + N ที่ HC3/HC4 อนุมัติ; ยังไม่ใช่ full ten-row qualification / Mapping: A03, A04, A12, A15–A17, A22, A26–A29, A34, A37, A39 / Risks: PR01, PR02, PR04, PR05, PR10 / Checkpoint: HC4

### PH1-018 — Claude baseline adapter

- Deliverable: selected SDK/headless transport และ permission host ที่อนุญาต; effective configuration, isolation และ billing identity แยก subscription/API
- ตรวจรับ: denied expansion จบ Run ไม่ re-prompt เพื่อขยายสิทธิ์; no unapproved hooks/MCP/agents/unsandboxed retry; identity/persistence/shutdown tested ตาม profile ไม่อนุมานจาก Codex หรือ auth mode อื่น
- Evidence: M + scoped N; unproven configurations ยัง disabled / Mapping: A03, A04, A12, A15–A17, A22, A26–A29, A34, A37, A39 / Risks: PR01, PR02, PR04, PR05, PR10 / Checkpoint: HC4

### PH1-019 — Ollama text adapter

- Deliverable: installed-model/digest discovery, bounded prepared-text HTTP stream, effective context/locality evidence และ interruption state; ไม่มี file/shell tools
- ตรวจรับ: cloud model/proxy/unverified daemon ไม่ผ่าน local-only; ไม่ใช้แค่ localhost หรือ child env เป็นหลักฐาน; ไม่ restart service/download model ให้เอง; closing stream ไม่ถูกอ้างเป็น server-side cancel หากยังไม่ยืนยัน
- Evidence: M + scoped N บนสอง OS / Mapping: A01, A03, A04, A11, A20–A22, A26–A29, A34 / Risks: PR05, PR06, PR07, PR08 / Checkpoint: HC4; D09/D11

### PH1-020 — Static Auto และ qualification fixtures

- Deliverable: local task kind, versioned provider/model priorities, hard eligibility filters, rationale/tie-breaks และ bounded task-class fixtures พร้อม registry provenance
- ตรวจรับ: provider eligible iff มีโมเดลผ่าน; เลือก provider ก่อน model; unknown class ต้อง user-declared supported kind/manual route หรือไม่มี eligible Run; known/unknown price ไม่ข้าม floors; changes invalidate pending approvals ตามสเปก
- Evidence: L/M สำหรับ routing; N สำหรับ task/model fixture claims; native registry ไม่ active จน qualification ครบความสามารถนั้น / Mapping: A05–A07, A09, A21 / Risks: PR05, PR07, PR08, PR12 / Checkpoint: HC4

### PH1-021 — Approved edit และ evidence

- Deliverable: fixed edit/check envelope, scope-expansion stop, actual diff/changed-path/check status และ forbidden-operation denial ของ adapter ที่ HC5 เลือก
- ตรวจรับ: clean workspace เท่านั้น; tests/scripts ไม่ใช่ permission ขยาย filesystem/network/credentials; no commit/push/PR/deploy; failed checks/self-report ไม่เป็น Task acceptance; candidate ต้องเป็น adapter เดียวกันสำหรับสอง OS
- Evidence: M ก่อน; N เฉพาะ HC5 permit; full demo acceptance อยู่ PH1-027 / Mapping: A12–A16, A36, A39, A40 / Risks: PR01, PR03, PR04, PR10 / Checkpoint: HC5; ไม่มี selected write adapter ปัจจุบัน

### PH1-022 — Full CLI และ Task acceptance

- Deliverable: command table ครบตามสเปก รวม list/open/delete wiring, route-bound Task behavior, budget/context changes เฉพาะ Run ถัดไป, EOF/Ctrl-C และ explicit criterion acceptance
- ตรวจรับ: in-flight mutation/queued work ถูกปฏิเสธ; revision N accepted ไม่ทำให้ N+1 accepted; missing/stale check evidence ไม่ถูกยกระดับ; exit ใช้ supervisor และไม่ทิ้ง Run อย่างเงียบ ๆ
- Evidence: L/M + real PTY tests สอง OS; N เฉพาะ integration ที่อนุญาต / Mapping: A02, A17, A18, A35, A36, A38 / Risks: PR07, PR09, PR11, PR12 / Checkpoint: HC5

### PH1-023 — Persistence consent และ owned deletion

- Deliverable: per-profile native artifact footprint/persistence preview, explicit raw-capture opt-in, harness-owned deletion/index cleanup และ partial-failure reporting
- ตรวจรับ: unknown/unapproved raw persistence ใช้งานไม่ได้; raw opt-in ยัง redact; active Task ลบไม่ได้; ไม่ลบ workspace/shared provider home/credentials/backups/provider records; ไม่มี automatic expiry หรือ native purge แฝง
- Evidence: L/M สำหรับ lifecycle; N สำหรับ footprint; H สำหรับ consent / Mapping: A33–A35 / Risks: PR02, PR09, PR10 / Checkpoint: HC5/HC6; ผล native ต้องครอบคลุมแต่ละ profile

### PH1-024 — Cross-feature failure integration

- Deliverable: ทำ failure chain ทีละแบบ: pending approval + timeout, disk failure + dispatch, restart + unknown usage, config/source change + approval, model mismatch + partial edits, terminal injection + journal
- ตรวจรับ: transition/result ที่เชื่อมหลาย modules คง no retry/no refund/no silent success; หลักฐานแยก injected failures จาก observed native failures; ไม่มีการเขียน unit tests ซ้ำ implementation โดยไม่ทดสอบ behavior
- Evidence: L/M; scoped N เฉพาะจำเป็น / Mapping: A04, A09, A15, A22–A40 ตาม chain / Risks: PR04, PR07–PR12 / Checkpoint: HC6 readiness

### PH1-025 — Packaging/setup

- Deliverable: compiled package/local puen startup, diagnostics และ runbooks สำหรับ policy/auth/state paths/recovery/delete/unknown costs พร้อม pinned versions
- ตรวจรับ: package smoke ผ่านสอง target OS; default tests ไม่เรียก provider; ไม่มี auto install model/provider, hooks หรือ publish; setup ไม่อ้างรองรับ OS/architecture อื่นจาก package ที่ติดตั้งได้
- Evidence: L/H บนสอง OS; ยังไม่ใช่ native qualification / Mapping: A01, A02, A30, A31, A35, A37 / Risks: PR03, PR10, PR14 / Checkpoint: HC6

### PH1-026 — Ten-profile qualification

- Deliverable: T01–T10 ใน plan พร้อม exact OS/provider/interface/model/auth/billing/config fingerprints; A01–A40 applicability/evidence matrix และ bounded task-class results
- ตรวจรับ: A03 positive baseline สำเร็จทุก row; native read-only/permissions/path/network/credential/extension/locality/persistence/cleanup มีหลักฐานจริงตาม role; mocks หรือผล OS/auth อื่นแทนไม่ได้ ไม่มี mandatory row ถูกลดเป็น N/A
- Evidence: N + labelled L/M faults; native-only assertions ไม่มี mock pass / Mapping: A01–A40 ตาม roles / Risks: PR01–PR14 / Checkpoint: HC6 → HC7; missing host/account = not tested และ block MVP

### PH1-027 — Two-OS coding demos

- Deliverable: small bug fixture บน clean Git workspace ผ่าน adapter ตัวเดียวกันบน Ubuntu/macOS พร้อม diff, actual check result, Run costs/unknowns และ revision-bound human acceptance
- ตรวจรับ: แต่ละ enabled write profile มีหลักฐานตัวเอง; user เห็น failed/not-run checks ตามจริง; no production Git/remote side effects; cancelled/failed trial ไม่ถูกสรุปเป็น demo success
- Evidence: N/H / Mapping: A13–A15, A17, A23–A29, A33, A34, A36, A39, A40 / Risks: PR01, PR03, PR04, PR08, PR12 / Checkpoint: HC6 → HC7

### Qualification execution units

PH1-026 และ PH1-027 เป็น roll-up สำหรับ gate ไม่ใช่งานทดลองขนาดใหญ่ที่ต้องทำรวดเดียว แยกหน่วยดำเนินงานด้านล่าง แต่ละหน่วยมี permit, fixture results, profile fingerprint, costs/unknowns และ pass/fail/not-tested ของตัวเอง รายการที่ติด blocker ไม่ทำให้หน่วยอื่นมีสิทธิ์ใช้งบหรือข้อมูลเพิ่ม

| Unit | Target / ขอบเขต | Prerequisite เพิ่มเติม |
| --- | --- | --- |
| PH1-026-T01 | Ubuntu / Codex subscription baseline | T01 host/account/config permit |
| PH1-026-T02 | Ubuntu / Codex API baseline | T02 host/account/config permit |
| PH1-026-T03 | Ubuntu / Claude subscription baseline | T03 host/account/config permit |
| PH1-026-T04 | Ubuntu / Claude API baseline | T04 host/account/config permit |
| PH1-026-T05 | Ubuntu / Ollama local baseline | T05 daemon/model/locality permit |
| PH1-026-T06 | macOS / Codex subscription baseline | T06 host/account/config permit |
| PH1-026-T07 | macOS / Codex API baseline | T07 host/account/config permit |
| PH1-026-T08 | macOS / Claude subscription baseline | T08 host/account/config permit |
| PH1-026-T09 | macOS / Claude API baseline | T09 host/account/config permit |
| PH1-026-T10 | macOS / Ollama local baseline | T10 daemon/model/locality permit |
| PH1-027-U | Ubuntu approved-edit demo | selected write profile ผ่าน baseline/control checks และ HC5/HC6 |
| PH1-027-M | macOS approved-edit demo | coding adapter เดียวกับ PH1-027-U และ write profile ของตัวเองผ่าน checks |

สิบหน่วย PH1-026-* สืบทอด dependencies/acceptance ของ PH1-026 แต่ตรวจตาม role และ exact target ใน plan สองหน่วย PH1-027-* สืบทอด PH1-027 ไม่มีการนำผลหน่วยหนึ่งไป fill row อีกหน่วย PH1-026 roll-up ผ่านได้เมื่อสิบหน่วยผ่านครบ และ PH1-027 ผ่านได้เมื่อสอง demos ผ่านพร้อมเงื่อนไข same adapter

### PH1-028 — Evidence audit/MVP acceptance

- Deliverable: release evidence index, Q1–Q34/A01–A40 traceability, all ten rows, demos, setup/recovery และ known limitations พร้อมสถานะที่ซื่อสัตย์
- ตรวจรับ: ผู้ใช้ตรวจ sign-off; missing/failed mandatory behavior = MVP blocked; mock development milestone ไม่แทน native gate; ไม่มีการเปิด deferred Phase 2/3 หรือเปลี่ยนสเปกเพื่อทำให้ checklist เขียวเอง
- Evidence: D/H บนผล L/M/N ที่อ้างได้จริง / Mapping: A01–A40 และ matrix / Risks: PR12, PR13, PR15 / Checkpoint: HC7

## Acceptance ownership A01–A40

ตารางนี้ผูกเจ้าของ implementation/verification ไม่แก้ expected results ของต้นฉบับ ทุก row ต้องได้รับการตรวจ evidence/applicability อีกครั้งใน PH1-026/PH1-028 ข้อที่เกี่ยวกับ native enforcement ต้องมี N ไม่ใช่เพียงผล unit/mock tests

| Acceptance | งานหลัก | งานร่วม/หลักฐานที่ต้องระวัง |
| --- | --- | --- |
| A01 | PH1-016 | PH1-019, PH1-025; diagnostics ไม่ auto invoke |
| A02 | PH1-022 | PH1-008 เป็น early mock subset; PTY จริงภายหลัง |
| A03 | PH1-026 | PH1-016–PH1-019; positive baseline N ครบ T01–T10 |
| A04 | PH1-016 | PH1-006, PH1-017–PH1-019; แยก synthetic reroute จาก N |
| A05 | PH1-020 | deterministic provider-first/model-second |
| A06 | PH1-020 | PH1-011, PH1-016; manual ไม่ bypass |
| A07 | PH1-020 | PH1-013; unknown ไม่เป็น zero |
| A08 | PH1-009 | PH1-007; restrictive intersection |
| A09 | PH1-009 | PH1-013, PH1-016, PH1-020; fingerprint invalidation |
| A10 | PH1-009 | PH1-016; metadata permit ก่อน outbound |
| A11 | PH1-019 | N ของ running daemon/local inference ไม่ใช่ localhost label |
| A12 | PH1-009 | PH1-017, PH1-018; actual native denial N |
| A13 | PH1-027 | PH1-021; same adapter สอง OS และ human acceptance |
| A14 | PH1-010 | PH1-021; ไม่มี automatic cleanup |
| A15 | PH1-021 | PH1-007, PH1-017, PH1-018; deny/end/new Run |
| A16 | PH1-009 | PH1-017, PH1-018; extension isolation N |
| A17 | PH1-007 | PH1-005, PH1-017, PH1-018, PH1-022; fresh sessions |
| A18 | PH1-005 | PH1-022; approved revision authority |
| A19 | PH1-011 | PH1-005, PH1-010; schema/source/workspace |
| A20 | PH1-011 | PH1-019; Thai/Unicode และ explicit files |
| A21 | PH1-011 | PH1-020; known capacity และ no truncation |
| A22 | PH1-014 | PH1-017–PH1-019; report native uncertainty |
| A23 | PH1-013 | PH1-007, PH1-012; attempt survives restart |
| A24 | PH1-013 | PH1-012; durable reservation/reconciliation |
| A25 | PH1-013 | PH1-009; exact unknown-cost permission |
| A26 | PH1-013 | PH1-017–PH1-019; source/method/unknown labels |
| A27 | PH1-014 | PH1-017–PH1-019; real cleanup timing N ตาม profile |
| A28 | PH1-014 | PH1-012; indeterminate ไม่ success |
| A29 | PH1-014 | PH1-006, PH1-017–PH1-019; no retry/fallback |
| A30 | PH1-012 | PH1-014; no invocation before durable admission |
| A31 | PH1-012 | PH1-014; no replay/refund/kill by stale PID |
| A32 | PH1-010 | PH1-012; cross-instance canonical locks |
| A33 | PH1-015 | PH1-023; inspect actual stored data |
| A34 | PH1-023 | PH1-017–PH1-019; per-profile native footprint N |
| A35 | PH1-023 | PH1-022; ownership, active refusal, partial deletion |
| A36 | PH1-022 | PH1-021; revision-bound checks/acceptance |
| A37 | PH1-009 | PH1-017, PH1-018, PH1-025; native fail closed N |
| A38 | PH1-015 | PH1-006, PH1-008, PH1-014, PH1-022; real terminal tests |
| A39 | PH1-021 | PH1-009, PH1-010, PH1-017, PH1-018; indirect script escape N |
| A40 | PH1-010 | PH1-011, PH1-021, PH1-024; last-moment revalidation |

## Preservation traceability Q1–Q34

ตารางนี้ใช้ตรวจความครบถ้วน ไม่ใช่แหล่งแก้ความหมายของ [approved decision ledger](../phases/phase-01-review.md)

| Decision | สิ่งที่ต้องคงไว้ | Work items |
| --- | --- | --- |
| Q1 | approved small edit/diff/check; no commit/push/PR/deploy | PH1-021, PH1-027 |
| Q2 | Ubuntu และ macOS | PH1-025–PH1-028 |
| Q3 | Ollama text-only, no harness agent loop | PH1-019 |
| Q4 | tested native controls หรือ fail closed | PH1-001, PH1-002, PH1-009, PH1-026 |
| Q5 | admission ไม่ใช่ guaranteed financial ceiling | PH1-013 |
| Q6 | bounded task qualification; unknown class ต้อง declare supported kind/manual หรือ block | PH1-020 |
| Q7 | approved data policy ก่อน remote request | PH1-009, PH1-016 |
| Q8 | fixed Run envelope และขอใหม่เมื่อขยายสิทธิ์ | PH1-007, PH1-009, PH1-021 |
| Q9 | trusted user ceiling/project snapshot | PH1-009 |
| Q10 | Task/Run/Native Session แยกกัน; one active/instance | PH1-005, PH1-007, PH1-010 |
| Q11 | private minimal journal; raw opt-in; manual deletion | PH1-005, PH1-015, PH1-023 |
| Q12 | no automatic retry/fallback; uncertainty explicit | PH1-014 |
| Q13 | unknown-cost permission ผูก project/billing profile | PH1-009, PH1-013 |
| Q14 | Manual/static Auto; local-only hard constraint | PH1-019, PH1-020 |
| Q15 | Ubuntu 24.04 x86_64 และ macOS 15 Apple Silicon | PH1-025–PH1-028 |
| Q16 | ten baseline rows; same write adapter ทั้งสอง OS | PH1-017–PH1-019, PH1-026, PH1-027 |
| Q17 | no unapproved hooks/MCP/plugins/memory/child agents | PH1-001, PH1-002, PH1-009, PH1-017, PH1-018 |
| Q18 | requested/reported model แยก; mismatch/reroute stop | PH1-016–PH1-019 |
| Q19 | canonical JSON/generated Markdown; approved revisions | PH1-005, PH1-011 |
| Q20 | clean Git required for writes; no cleanup | PH1-010, PH1-021 |
| Q21 | fresh sessions; Task-bound provider/model; resume deferred | PH1-007, PH1-017, PH1-018, PH1-022 |
| Q22 | cumulative per-Task accounting; no refunds/unknown zero | PH1-012, PH1-013 |
| Q23 | bounded context; no silent truncation/compaction | PH1-011, PH1-014 |
| Q24 | versioned static provider/model priorities | PH1-020 |
| Q25 | completion/checks/human acceptance แยก | PH1-007, PH1-022, PH1-027 |
| Q26 | explicit commands; no queued/in-flight changes; transient history | PH1-008, PH1-022 |
| Q27 | expansion จบ Run; fresh approval | PH1-007, PH1-021 |
| Q28 | invalid schema/required stale source/workspace block | PH1-005, PH1-010, PH1-011 |
| Q29 | local drafting/explicit files; no hidden model calls | PH1-011, PH1-020 |
| Q30 | 3 Runs, 15 minutes, 5+5 cleanup defaults | PH1-007, PH1-013, PH1-014 |
| Q31 | durable admission, locks, crash review, no replay | PH1-010, PH1-012, PH1-014 |
| Q32 | disclose native artifacts; owned deletion only | PH1-015, PH1-023 |
| Q33 | whole-workspace authorization; tool network แยก | PH1-009, PH1-010, PH1-017–PH1-019, PH1-021 |
| Q34 | mocks ไม่แทน actual native evidence | PH1-026–PH1-028 |

ก่อนเริ่มแต่ละ item ต้องผ่าน checkpoint/decision ที่เกี่ยวข้องใน plan/risks การจบ M1 หรืออนุมัติแผนไม่ได้อนุญาตให้เปิด native execution เอง
