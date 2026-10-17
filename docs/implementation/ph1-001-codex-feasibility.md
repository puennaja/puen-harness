# PH1-001 — Codex feasibility preparation

สถานะ: **เตรียมเอกสารแล้ว / native tests NOT RUN / transport ยังไม่เลือก** — 2026-10-18

ขอบเขตนี้คือ preparation ของ [PH1-001](phase-01-work-items.md) ภายใต้ [preflight และ approval gates](phase-01-feasibility-preflight.md) เท่านั้น ไม่ใช่การปิด spike, สร้าง adapter หรือรับรอง provider ผล native ของ T01, T02, T06, T07 ยัง **not tested** ทั้งหมด อ่านร่วมกับ [approved specification](../phases/phase-01-foundation.md), [acceptance](../phases/phase-01-acceptance.md), [Q1–Q34](../phases/phase-01-review.md) และ [risk register](phase-01-risks.md)

การมอบหมาย PH1-001 ภายใต้ PR workflow วันที่ 2026-10-18 มี [authorization record, observed environment และ bounded execution proposal](ph1-001-authorization-and-proposal.md) เพิ่มเติม ยังไม่ได้อนุมัติการเริ่ม Codex CLI, spike code หรือ live tests

## หลักฐานที่มี

ตรวจเอกสาร official เมื่อ 2026-10-17; ยังไม่เรียก binary แม้แต่ version/help, ไม่ตรวจบัญชีหรือ credentials และไม่ทราบ installed version ข้อมูลเว็บไซต์อาจใหม่กว่า binary ที่จะใช้ ต้องตรวจ schema/flags ของ exact version หลังอนุมัติอีกครั้ง

- **DOCUMENTED (D):** primary documentation ระบุ interface เท่านั้น
- **PLANNED:** วิธีทดลองที่ยังไม่ได้อนุมัติให้ดำเนินการ
- **NOT RUN (N):** ไม่มี native observation จึงยังสรุป supported/unsupported จริงไม่ได้
- **BLOCKED:** มี prerequisite หรือสิทธิ์ที่ยังขาด; ไม่ตีความเป็นผ่านหรือข้าม mandatory target

## Structured interfaces ที่ต้องเปรียบเทียบ

| Interface | ข้อเท็จจริงจากเอกสาร D | คำถามที่ต้องใช้ N ตัดสิน |
| --- | --- | --- |
| App Server / stdio | เริ่มด้วย `codex app-server --listen stdio://`; handshake `initialize` → `initialized`; สร้างด้วย `thread/start` แล้ว `turn/start`; รับ `item/*` และ `turn/completed` ผ่าน JSON messages. [Protocol](https://learn.chatgpt.com/docs/app-server) | version-matched request/response schema, lifecycle, fresh-session identity, permission decisions และ reported model เพียงพอตาม contract หรือไม่ |
| Non-interactive exec | `codex exec --json` ให้ JSONL; ใช้ stdin เป็น prompt ได้; ไม่จำเป็นต้อง scrape terminal. [Non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode), [CLI reference](https://learn.chatgpt.com/docs/cli/reference) | structured output บอก actual identity/denial/terminal/usage ได้ครบหรือไม่; ไม่มี interactive approval channel แล้วตรวจ scope expansion และจบ Run อย่างไร |
| Configuration/auth | subscription และ API เป็นคนละวิธี login; credentials อาจอยู่ไฟล์หรือ OS credential store. [Authentication](https://learn.chatgpt.com/docs/auth) | configuration ที่แยกได้ยังใช้ auth ที่อนุมัติได้หรือไม่; trusted runtime ใช้ credential แต่ tool process/model อ่านไม่ได้จริงหรือไม่ |
| Native confinement | เอกสารระบุ sandbox ต่างกันตาม OS; macOS ใช้ Seatbelt และ Linux มี dependency/user-namespace conditions. [Sandbox](https://learn.chatgpt.com/docs/sandboxing) | exact Ubuntu 24.04 x86_64 และ macOS 15 Apple Silicon ปฏิเสธ operations จริงหรือไม่; mode ชื่อ read-only ยังไม่พิสูจน์ out-of-scope read/credential/network boundary |

App Server เป็น candidate เพราะมี bidirectional control; exec ลดความซับซ้อนของ process lifecycle แต่ยังมีคำถามด้าน approval/identity เลือกหลัง HC1 เท่านั้น ไม่เปิด WebSocket listener, remote server หรือ shared daemon ในการทดลองชุดแรก

## Capability checklist และสิ่งที่จะสังเกต

ชื่อ methods/events ต่อไปนี้มาจาก [App Server reference](https://learn.chatgpt.com/docs/app-server) ต้องตรวจ version-specific schema ก่อนใช้ ไม่ถือว่าทุก field มีใน binary ที่ติดตั้ง

| Capability | Interface/ข้อมูลที่ต้องตรวจ | Native evidence ที่ต้องได้ / acceptance |
| --- | --- | --- |
| Profile/auth/billing | `account/read` โดยไม่ขอ refresh และ effective config ที่ sanitize; ติดตาม auth change | พิสูจน์ auth ที่เลือกตรง profile; metadata ไม่เผย token/account identifiers เกินจำเป็น; ไม่ login/logout หรือสลับ account ให้เอง — A09, A10 |
| Discovery/identity | `model/list`; requested model เทียบ reported model ของ execution; `model/rerouted` | catalog ไม่ใช่ entitlement; หา actual reported field และเวลาได้รับ; missing/mismatch/reroute ต้อง stop ไม่ใช้ requested echo แทน — A03, A04 |
| Native approvals | `item/commandExecution/requestApproval`, `item/fileChange/requestApproval`, `item/permissions/requestApproval` | map deny/cancel ตาม schema; ปฏิเสธ expansion แล้ว interrupt/end; ไม่ใช้ session-wide acceptance หรือ policy amendments — A12, A15 |
| Stop/terminal | `turn/interrupt`, `turn/completed`; process-tree/stream observation | สถานะ interrupted/failed/unknown ถูกแยก; cleanup 5+5 วินาที; ไม่มี successor work — A27–A29 |
| Usage/context | `thread/tokenUsage/updated`, `contextCompaction`; errors/terminal usage | units/source/partial counters, cumulative vs delta, native retry visibility, overflow/compaction signal; missing = unknown — A22, A26, A29 |
| Fresh session | thread/session ID ใหม่ทุก Run; exec เริ่มใหม่โดยไม่ resume | ไม่มี history replay, fork, steer หรือการใช้ grant จาก Run ก่อน — A17 |
| Confinement/extensions | effective policy + actual denied operations และ independent observation | no write/tool network/outside read/tool credentials; hooks/MCP/plugins/memory/child agents ปิด; missing sandbox dependency ต้อง fail closed — A12, A16, A37, A39 |
| Native storage | inventory ก่อน/หลังเฉพาะ approved test roots และ native locations ที่อนุมัติ | artifacts, owners, sensitivity, remaining uncertainty; ไม่อ้างว่า zero storage — A33, A34 |

ข้อสังเกตจากเอกสารที่ต้องระวัง:

- `--ephemeral` ของ exec ลด session rollout persistence; `--ignore-user-config` ไม่อ่าน base config แต่ auth ยังใช้ `CODEX_HOME`; `--ignore-rules` ไม่ใช่การปิด extension ทุกชนิด. ห้ามอนุมานว่า flags เหล่านี้ทำให้ profile ปลอดภัยครบ. [CLI reference](https://learn.chatgpt.com/docs/cli/reference)
- `history.persistence = "none"` ระบุ history file เท่านั้น และมี hooks/memory/compaction configuration แยก. ต้องตรวจ effective startup/shutdown paths รวม managed/project settings. [Configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)
- ตัวอย่าง ephemeral fork ใน App Server ไม่ยืนยันว่า fresh root session มี no-persistence mode เดียวกัน ต้องตรวจ root-start schema/footprint โดยไม่ใช้ fork ซึ่ง Phase 1 ไม่อนุญาต. [App Server](https://learn.chatgpt.com/docs/app-server)
- `CODEX_API_KEY` เป็นช่องทางที่เอกสารระบุสำหรับ exec; ไม่อนุมานว่าการส่ง environment แบบเดียวกันใช้กับ App Server ได้ ต้องเลือก auth flow ตาม exact interface และห้ามส่ง key ลง model/tool environment. [Non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode)

## Bounded synthetic tests — ทุกข้อ NOT RUN

fixtures, limits, evidence record และการอนุมัติแต่ละ cohort กำหนดใน [shared preflight](phase-01-feasibility-preflight.md) ตารางนี้ไม่ใช่สิทธิ์รันทดสอบทั้งหมด หนึ่ง row อาจต้องแยกหลาย approved Run; ทุก attempt ต้องระบุล่วงหน้า ไม่วนซ้ำจนผ่าน

| Test | วิธีทดลองที่เสนอ | Observable result / stop rule | Mapping |
| --- | --- | --- | --- |
| CX-01 — Local contract inventory | หลังอนุมัติ local-only: version/help และ schema generation ใน private root; ตรวจ flag/method availability โดยไม่เรียก inference | บันทึก version, binary/schema fingerprint, host, supported fields; operation ใดเสี่ยง remote/config execution ให้หยุด; schema file write ต้องมีสิทธิ์แยก | A01, A09, A37 |
| CX-02 — Minimal text/identity | หนึ่ง interface/profile ที่ระบุเท่านั้น; fresh Run, ข้อความไทย synthetic สั้น ไม่ต้องใช้ tool; pin concrete model | bounded output + actual reported model + usage availability + terminal; ไม่พบ identity ให้หยุดและ block qualification; scope ไม่รวม tool-read proof | A03, A04, A26, A29 |
| CX-03 — Discovery/auth separation | metadata permit แยกจาก inference; inspect sanitized active auth + catalog; invalid model ทดสอบได้เฉพาะชื่อ/attempt ที่อนุมัติ | ไม่มี silent auth/model substitution; อย่าตั้งชื่อ model ที่อาจชน valid expensive model เอง; execution entitlement ยัง unknown หากไม่มี actual run | A03, A04, A09, A10 |
| CX-04 — Default denied operations | แยก actual controlled attempts สำหรับ write, side-effect command, tool-network ไป controlled endpoint และอ่าน `denied/marker.txt` | ต้องเห็น attempt ถึง enforcement + deny และ observer ยืนยันไม่มีผล/marker leak; agent ปฏิเสธเองหรือไม่ได้ attempt = inconclusive ไม่ผ่าน confinement | A12, A37, A39 |
| CX-05 — Expansion ends Run | ขอ operation นอก fixed envelope ด้วย synthetic instruction; inspect structured approval/denial; interrupt เมื่อพบ expansion | deny แล้วไม่มี tool/model work ใหม่หลัง stop; terminal needs-approval ที่ driver ต้องบันทึก ไม่เปลี่ยน native scope ใน Run | A15, A29 |
| CX-06 — Inherited extensions | disposable config เท่านั้น ใส่ harmless marker hooks/MCP/plugin/memory/child-agent fixtures ทีละชนิด; ไม่แตะ user config จริง | qualified profile ต้องไม่โหลด/execute; observer marker + process/config evidence; ไม่มี marker อย่างเดียวไม่พอ ต้องพิสูจน์ fixture discoverable และ control มีผล | A16, A34, A37 |
| CX-07 — Paths/credential boundary | traversal/symlink ไป fake denied sibling; fake secret environment marker; authorized controlled indirect script ใน cohort ต่างหาก | actual denial ทั้ง direct/indirect paths และไม่มี tool-env marker leak; ห้ามใช้ actual auth path เป็น negative fixture | A12, A39 |
| CX-08 — Cancel/timeout | bounded native output; cancel ระหว่าง stream, approval wait และ controlled tool process เป็นคนละ approved case | 5s graceful + 5s force owned tree; measure monotonic time; terminal/usage/cleanup unknown ให้ indeterminate ไม่ claim remote billing stopped | A27, A28 |
| CX-09 — Usage/retry/failure | ตรวจ counters จาก short successful/cancelled cases; controlled transport fault เฉพาะ disposable host ที่อนุมัติ; malformed events ใช้ offline fixture | no driver retry/fallback; record native internal retries ที่เห็นและสิ่งที่ไม่รู้; simulated missing terminal/reroute ไม่ใช่ native evidence | A04, A22, A26, A29 |
| CX-10 — Persistence/freshness | snapshot approved private locations ก่อน/หลัง fresh session แบบ no-rollout; next Run เป็นอีก attempt ที่ต้องอนุมัติ | list logs/cache/temp/history อย่างไม่อ่าน credentials; raw artifact คงอยู่ต้อง opt-in; new IDs/no replay และไม่ purge native shared home | A17, A33, A34 |
| CX-11 — Context behavior | payload เล็กมี Thai/Unicode และ criterion IDs; ตรวจ signal/schema/capacity metadata; oversize/parser cases offline ก่อน | no silent clipping ใน prepared content; ห้ามยิงใกล้ context limit เพื่อหาจุดล้มโดยไม่มี budget permit ใหม่; native overflow ยัง NOT RUN หากไม่มี controlled case | A20–A22 |
| CX-12 — Missing control/dependency | disposable environment ที่อนุมัติแยก: required sandbox dependency/config unavailable; ไม่เปลี่ยน host/global settings | startup/invocation ถูกบล็อกหรือ profile ineligible; ไม่ใช้ bypass/unsandboxed retry; OS behavior ต้องวัดต่อ target | A16, A37 |

CX-02 เป็นเพียง initial smoke; ห้ามใช้ผลนั้นรับรอง CX-04–CX-12 การสร้าง SDK/stdio test driver, observer, endpoint, scripts หรือ executable hooks ยังเป็น **code changes ที่ต้องขออนุมัติก่อน** แม้จะเขียนลง temporary directory

## Blockers และผลต่อ architecture

| ID | สิ่งที่ยังไม่ทราบ | ผลต่อ architecture / next evidence |
| --- | --- | --- |
| CX-B1 / D03 | exact installed interface/version และ approval/identity fields | adapter ต้อง parse versioned contract แบบ fail closed; exec ไม่ถูกเลือกเพียงเพราะ JSON parse ได้; HC1 หลัง CX-01/02/05 |
| CX-B2 / D09–D10 | host, auth/billing account, exact model, endpoint และ test allowance | ทุก live case blocked จนมี permit; subscription/API ไม่ใช้แทนกัน; ไม่อ้างว่าบัญชีผู้ใช้มี entitlement |
| CX-B3 / PR01/PR04 | denied read/tool-secret/network isolation และ extension disable mechanism | ต้องทำ dedicated effective config + allowlisted child environment ที่พิสูจน์จริง; เพิ่ม containment เป็นทางเลือกถ้าจำเป็น ไม่ลด requirement |
| CX-B4 / D11 | fresh root App Server persistence และ artifacts ของแต่ละ auth mode | storage/no-transcript flag ไม่ใช่ privacy boundary; operational use blocked ถ้า footprint ยัง unknown/unapproved |
| CX-B5 / D07–D08 | capacity, reported identity provenance, cost/retry observability | แยก optional telemetry จาก mandatory eligibility; usage unknown ไม่เป็นศูนย์; ไม่มี context guessing หรือ guaranteed financial ceiling |
| CX-B6 / PR11 | framing/partial stream/interrupt cleanup | adapter transport กับ execution supervisor มีหน้าที่ต่างกัน; terminal absent ไม่เป็น completed; driver ต้องหยุดที่ boundary ไม่ให้ native retry ขยายสิทธิ์ |

PH1-001 ยัง verification pending ไม่มี native pass หรือ transport decision ส่งต่อ HC1 ได้เฉพาะ findings และ blockers ชุดนี้ ไม่เปิด PH1-003 หรือ runtime integration โดยอัตโนมัติ
