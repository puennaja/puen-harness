# Keep approved Task Context independent of native conversations

Status: Accepted on 2026-10-17; discussion decision confirmed in Q10/Q19/Q21 and documentation approved by the user.

A Task's approved requirements live in versioned canonical JSON with a generated Markdown view; provider-native conversations are not their authority. Phase 1 starts a fresh native session for each Run and binds a Task to one provider/model after first admission. This sacrifices seamless conversational continuation to make approval scope, restart behavior, accounting and later provider handoff traceable without trusting incompatible native session formats. Native resume and cross-provider continuation require Phase 2 review.
