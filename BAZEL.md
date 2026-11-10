# Bazel package graph

The first child-owned Bazel target is `//typescript:typecheck`. It compiles the existing workshop client from `typescript/src/index.ts` with TypeScript 5.6.3 (the exact version in the committed pnpm lock), then emits JavaScript. It does **not** execute the script: its `dryRun()` call would contact Sui mainnet. Bazel may fetch pinned build dependencies during module resolution; "offline" here means no chain or wallet runtime action.

With Bazel 8.7.0 available, build from this repository root:

```bash
bazel build //typescript:typecheck --jobs=2 --local_resources=memory=2048
```

The root module pins Node 24.22.2 for Linux x86-64. The `hero` Move package remains on its native Sui workflow and is outside this TypeScript target. Its `Move.lock` references a mutable Sui framework branch, so an offline Bazel Move target needs a separate immutable framework snapshot and compiler 1.36.2.
