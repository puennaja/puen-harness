# PH1-001 — Codex Local Inventory (P0-CX)

วันที่ 2026-10-18 (Asia/Bangkok) — **Static inventory recorded / CLI inventory BLOCKED before invocation / ไม่ใช่ native conformance**

## Scope และ authorization

ผู้ใช้มอบหมาย P0-CX บน current development host รวม Ubuntu 26.04 เป็น exploratory evidence เท่านั้น และอนุญาต documentation commit, push branch `feature/PH1-001-codex-local-inventory` และเปิด Draft PR เข้า `master` ใน `puennaja/puen-harness` แยกจากสิทธิ์ provider execution ฐานหลัง `git fetch origin master` คือ `22733936100573aa29c510c07900254ce8764840`; working tree สะอาดก่อนสร้าง branch จาก `origin/master` ไม่แก้ local `master`

Permit จำกัด `codex --version`, `codex --help`, `codex app-server --help`, `codex exec --help` รวมสูงสุด **4 invocations**, sequential, **10s/invocation**, **no retries** และต้องหยุดก่อน execution หาก startup อาจอ่าน configuration ไม่ปลอดภัยหรือสร้าง artifacts นอกขอบเขต สิทธิ์นี้ไม่รวม inference, auth checks/credential inspection, login/logout, dependencies, provider configuration changes, live provider API, schema generation, adapter/runtime code หรือ PH1-002

อ้างอิง [approved foundation](../phases/phase-01-foundation.md), [acceptance A01–A40](../phases/phase-01-acceptance.md), [CONTEXT](../../CONTEXT.md), ADR [0001](../adr/0001-preserve-provider-agent-loops.md)/[0002](../adr/0002-task-context-authority.md), [plan](phase-01-plan.md), [work items](phase-01-work-items.md), [risks](phase-01-risks.md), [Codex preparation](ph1-001-codex-feasibility.md), [previous proposal](ph1-001-authorization-and-proposal.md), [shared preflight](phase-01-feasibility-preflight.md) และ [review pipeline](../review-pipeline.md) รายงานนี้เพิ่มหลักฐาน/authorization รอบใหม่ ไม่เปลี่ยน approved requirements หรือ qualification targets

## Evidence classes

- **L-STATIC / OBSERVED:** อ่าน host facts, executable/package files และ Git metadata โดยไม่เริ่ม Codex process
- **D-SOURCE:** อ่าน source ทางการตรง release ตาม local package metadata; ไม่ใช่ execution observation หรือ binary authenticity proof
- **D-DOC:** เอกสารทางการเพื่อเปรียบเทียบเท่านั้น
- **UNKNOWN / NOT RUN / BLOCKED:** ไม่มีผล CLI หรือ native behavior ให้สรุป ไม่มี assumption ถูกยกระดับเป็น supported capability

## Observed host และ executable provenance — L-STATIC

| Check ที่รันจริง | Result / ข้อจำกัด |
| --- | --- |
| `cat /etc/os-release` | `Ubuntu 26.04.1 LTS`, `VERSION_ID=26.04`; ไม่ตรง mandatory Ubuntu 24.04 LTS |
| `uname -sm` | `Linux x86_64`; เป็น execution environment ของ shell นี้ ไม่รับรอง physical-host/container topology |
| `command -v codex`, `type -a codex` | PATH เลือก `/usr/local/bin/codex`; มี executable อีกตัวใน VS Code extension path แต่ไม่ได้เลือก/execute/hash ตัวนั้น |
| `ls -l /usr/local/bin/codex`, `readlink -f /usr/local/bin/codex` | symlink `../lib/node_modules/@openai/codex/bin/codex.js` → `/usr/local/lib/node_modules/@openai/codex/bin/codex.js` |
| อ่าน `package.json`, `bin/codex.js` ของ installation นี้ | npm package ระบุ `@openai/codex` **`0.162.1`**, repository `openai/codex`; launcher เป็น JavaScript ที่ spawn native binary และส่งต่อ inherited environment ไม่ใช่ alias ที่ไม่ทราบ implementation |
| `command -v node` | `/usr/bin/node` ตาม shebang `/usr/bin/env node`; ไม่ execute Node หรือ launcher |
| อ่าน platform `package.json` และ `codex-package.json` | platform package ระบุ `0.162.1-linux-x64`; layout ระบุ version `0.162.1`, target `x86_64-unknown-linux-musl`, variant `codex`, entrypoint `bin/codex` |
| `file` ของ native path ด้านล่าง | ELF 64-bit LSB PIE, x86-64, static-pie linked, stripped |
| `namei -l` ของ native path | path components ที่แสดงเป็น `nobody:nogroup`, directories `0755`, binary `0755`; ownership/mode ไม่ใช่ signature verification |
| `strings` ของ native binary (filtered) | พบ `arg0/src/lib.rs`, `codex-arg0`, `CODEX_HOME`; ใช้ระบุจุดตรวจ source เท่านั้น ไม่พิสูจน์ control flow |

Native path ที่ launcher resolve ตาม platform package ที่พบ:

```text
/usr/local/lib/node_modules/@openai/codex/node_modules/@openai/codex-linux-x64/vendor/x86_64-unknown-linux-musl/bin/codex
```

Fingerprints จาก `sha256sum` ของ installed files (ไม่ใช่การตรวจเทียบ signed release):

| File | SHA-256 |
| --- | --- |
| `@openai/codex/bin/codex.js` | `61b0194f3bb6534439c8d26a3ed57d0805f84b884588b761795323eeb92fcf70` |
| native `bin/codex` ตาม absolute path ด้านบน | `628e35487d888a28f0427fc37501da529dd3d81fcbad7a2c3421c1e8670d92e1` |

**Exact CLI-reported version: UNKNOWN / NOT RUN.** `0.162.1` เป็น version ที่สังเกตจาก package/layout metadata เท่านั้น ไม่ใช่ผล `codex --version` ยังไม่ได้ยืนยัน installer, registry integrity/signature, reproducible-build correspondence หรือความตรงกันของ binary กับ upstream source

## Startup blocker — D-SOURCE, ไม่มีการทดลอง side effects

ตรวจ source ผ่าน GitHub read API ของ public repository ทางการ `openai/codex` โดยไม่เรียก Codex หรือ provider inference API: tag `rust-v0.162.1` resolve เป็น commit **`092d3acd6bec3e3a14bdc7e7a2810ab628ab759d`**; Git blob ของ `codex-rs/arg0/src/lib.rs` คือ `c3467367190389297406c1aefd40d42ca503b6b5` ใช้ immutable commit links ด้านล่าง

| Source evidence | ผลต่อ P0-CX |
| --- | --- |
| [CLI entry point และ parser](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/cli/src/main.rs#L1031-L1051) | `main` เรียก startup dispatcher ก่อน `MultitoolCli::parse()` ซึ่งจัดการ help/version จึงใช้ argument เหล่านี้ยืนยันว่าไม่มี startup side effects ไม่ได้ |
| [Startup dispatch](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/arg0/src/lib.rs#L168-L184) และ [dotenv loading](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/arg0/src/lib.rs#L296-L323) | อ่าน `.env` ใต้ resolved Codex home และนำค่าที่ผ่าน filter เข้า process environment ก่อน parse; ไม่ทราบว่ามีไฟล์จริงหรือมีข้อมูลใด เพราะไม่ได้ตรวจ home/config/credentials |
| [Helper setup](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/arg0/src/lib.rs#L338-L400) | สร้าง home และ `tmp/arg0`, ตั้ง permissions, สร้าง per-invocation helper directory/lock/symlinks; root directories และ permission changes อาจยังอยู่หลังจบ |
| [Janitor](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/arg0/src/lib.rs#L478-L529) | startup อาจ cleanup directories ที่เข้าเงื่อนไข lock ใต้ shared helper root; ไม่อยู่ใน read-only permit |
| [Temporary-root guard](https://github.com/openai/codex/blob/092d3acd6bec3e3a14bdc7e7a2810ab628ab759d/codex-rs/arg0/src/lib.rs#L341-L354) | release source ปฏิเสธ helper root ใต้ system temp; ไม่ใช้ `/tmp` redirect เพื่อเลี่ยง guard หรือเปลี่ยน temp/config environment เอง |

**Decision: BLOCKED before first CLI invocation.** Matching-release source แสดงความเสี่ยงเพียงพอตาม stop condition ของผู้ใช้ แม้ไม่ได้พิสูจน์ว่า installed binary ทำสิ่งเหล่านี้จริง ไม่ได้อ่านหรือ list provider home/auth/config files, ตรวจ environment values, สร้าง isolated home, เปลี่ยน permissions/config, เริ่ม listener หรือทดลองให้ sandbox ปฏิเสธ writes แล้วค่อยรันต่อ ไม่อ้างว่าได้สังเกต filesystem footprint ของ Codex จริง

## Invocation ledger และ interface inventory

| Allowed command (จะใช้ absolute launcher path หาก safe startup ผ่าน) | Status | Exit / elapsed / output |
| --- | --- | --- |
| `/usr/local/bin/codex --version` | NOT RUN — startup blocker | N/A |
| `/usr/local/bin/codex --help` | NOT RUN — startup blocker | N/A |
| `/usr/local/bin/codex app-server --help` | NOT RUN — startup blocker | N/A |
| `/usr/local/bin/codex exec --help` | NOT RUN — startup blocker | N/A |

ใช้ไป **0/4 CLI invocations**, **0 retries**; 10s deadline ไม่ถูกเริ่มเพราะไม่ launch process ไม่มี stdout/stderr จาก CLI ให้บันทึกหรืออ้าง flags

| Capability / comparison | สิ่งที่ทราบและสิ่งที่ยัง UNKNOWN |
| --- | --- |
| Available App Server / exec interfaces | preparation documents ระบุ structured App Server และ exec JSON เป็น candidates; availability ของ installed CLI ยัง **UNKNOWN** เพราะไม่ได้รัน help/handshake |
| Observed CLI flags | **ไม่มี**; `--listen`, `--json`, `--ephemeral`, `--ignore-user-config` และ permission/model options ยังไม่ใช่ observed installed-CLI capabilities |
| Documentation vs observed CLI behavior | **ยังเปรียบเทียบ actual CLI behavior ไม่ได้**; ไม่มีผล help/version จึงไม่รายงานว่า flags ตรง/ขัดกับเอกสาร |
| Documentation vs startup source | [Official CLI reference](https://learn.chatgpt.com/docs/cli/reference) ที่อ่านวันที่ 2026-10-18 อธิบาย commands/configuration; ไม่ได้พิสูจน์ว่า help/version ไม่แตะ home หรือ artifacts; source inspection ทำให้ต้องแยก startup safety ออกจาก runtime flags |
| Auth/model/entitlement/effective config | UNKNOWN / NOT INSPECTED; ไม่อนุมานจาก package, editor session หรือ PATH |
| Permissions, identity/usage events, cleanup, persistence | UNKNOWN / NOT TESTED; source-level concerns ไม่ใช่ native enforcement หรือ actual artifact evidence |

## Architecture implications และ unresolved decisions

1. **D03 / PR01:** ยังไม่เลือก App Server หรือ exec JSON และยังไม่สร้าง adapter; schema, identity, approval/interrupt/terminal behavior ต้องมี version/profile-specific evidence แยกจาก availability ของ flags
2. **D11 / PR02 / PR10:** future `inspectLocal` ต้องมี startup/config/artifact contract ของตัวเอง การเรียก version/help ผ่าน trusted binary อาจมี side effects ก่อน runtime options มีผล ต้อง resolve ก่อนเสนอ local/native invocation ใหม่
3. **PR12:** เก็บ package version, CLI-reported version, upstream source revision และ native conformance evidence แยกกัน fingerprint ช่วยระบุไฟล์ แต่ไม่ qualify executable หรือ profile
4. **D09 / PR03:** Ubuntu 26.04 เป็น exploratory host เท่านั้น; T01/T02/T06/T07 ยังคง **not tested**, mandatory Ubuntu 24.04/macOS 15 และสิบ baseline rows ไม่เปลี่ยน
5. **D07/D08/D10:** ไม่มี account/model/capacity/billing/persistence permit สำหรับ live work; native paths ที่พึ่งข้อมูลเหล่านี้ยัง blocked ไม่มี inferred eligibility หรือ transport decision

Human decisions ที่ร้องขอเพื่อรอบถัดไป: รับ blocker นี้และเลือกให้ operator ส่ง sanitized inventory หรืออนุมัติ separate startup-isolation proposal ที่ระบุ exact private path, environment allowlist, permitted filesystem effects/cleanup และวิธีไม่อ่าน shared config/credentials ก่อน invocation ไม่ได้ขอ blanket live/code permission และไม่ได้ดำเนิน proposal ใดในรอบนี้

## Actual checks, acceptance coverage และ review handoff

| Check | Result / coverage |
| --- | --- |
| Host/path/package/launcher/hash/source inspection ตามรายการข้างต้น | PASSED สำหรับ static evidence เท่านั้น; พบ startup blocker ตามเงื่อนไขของ permit |
| CLI version/help (CX-01 บางส่วน) | BLOCKED / NOT RUN ทั้ง 4 commands; inventory ยังไม่ครบ ไม่ใช้ package metadata แทน CLI output |
| Documentation scope/local-link/whitespace checks | PASSED; ตรวจเฉพาะสาม Markdown files ใน commit และยืนยันไม่มี runtime/spec/ADR/PH1-002 diff |
| Native CX-02–CX-12, A03/A04/A09/A12/A15/A16/A26–A29/A34/A37/A39 | NOT RUN / ไม่มี native acceptance pass; A01/A09/A37 มีเพียง supporting static evidence บางส่วนจาก CX-01 |
| Runtime build/unit/mock tests | NOT RUN — documentation-only change ไม่มี runtime implementation ในขอบเขต |
| GitHub Actions PR preflight | NOT RUN ณ local evidence commit; ต้องตรวจ remote run ของ PR/head SHA หลังเปิด Draft PR และรายงาน actual status ใน delivery handoff |
| Wit technical review / Human approval | PENDING; ไม่ถือว่า preflight หรือ documentation checks เป็น review/approval |

ไฟล์ใน commit: รายงานนี้ และ pointers ระบุ authorization รอบใหม่ใน Codex preparation/previous proposal เท่านั้น ไม่มีการแก้ runtime, workflow, approved specs, ADRs, plan/work-items/risks, shared preflight หรือ PH1-002

ส่ง Draft PR ตาม authorization ของผู้ใช้ แล้วตรวจสถานะ preflight ตามที่สั่งก่อนหยุด งานนี้ไม่ปิด PH1-001 เป็น verified, ไม่เริ่ม Work Item อื่น และไม่ merge/close/self-approve/mark ready รอ Wit และ Human Gate Owner ตัดสินขั้นถัดไป
