# Preserve native coding-agent loops; keep Ollama text-only in the MVP

Status: Accepted on 2026-10-17; discussion decision confirmed in Q1/Q3/Q17 and documentation approved by the user.

Puen Harness coordinates Codex and Claude Code through qualified structured interfaces while leaving their agent loops with those providers. Ollama is a bounded text route in Phase 1, without file/shell tools; harness does not build a local coding-agent loop. This trades uniform coding capability for a smaller, enforceable MVP and avoids duplicating native tool execution, recovery and safety behavior. A future Ollama coding route requires a separately reviewed existing-agent integration or a new architectural decision.
