# Harness 0.2.1-rc.3 compatibility

The published `0.1.26` artifact enumerates exact peer versions up to rc.2, so the rc.3 host rejects installation before activation ([#277](https://github.com/NanmiCoder/dsh-agent-teams/issues/277)). This change adds rc.3 as an exact `preview` target. The recommended host and the development cohort stay on rc.2; promoting rc.3 is a release decision.

## Source audit

- rc.2: `687f282e8625c6b8d11ff58211a90524be2e4317` (`dsh-v0.2.1-rc.2`).
- rc.3: `1f81c047fc48fa190a683e469daa699857e9afe8` (`dsh-v0.2.1-rc.3`).
- Six commits including the release and its merge: navigation entry customization, navigation analytics, navigation pin fixes, and the DeepSeek API-key `/models` listing.
- Server packages AgentTeams imports (agent, session, session-projection, subagent, tools, system-prompt, llm, llm-retry, commands, util-values, workspace, api-session-controller, plugin manager and app boot) change only their versions, apart from one doc comment in `api-session-controller`.
- Client changes are additive: `dsh-client-ui-conversation` gains a View-tab More menu slot, a `whenDraftExists` input option and two submission fields; `dsh-client-ui-primitives` gains a `current` Menu row prop, a `left` tooltip side and an icon. AgentTeams uses `Menu`, `MenuEntry` and `Modal` from primitives and the card contract from ui-conversation.

## Verification

The unmodified `0.1.26` npm tarball was rejected by the rc.3 CLI plugin manager in a fresh isolated profile (`installation rejected: ... incompatible with dsh 0.2.1-rc.3`), reproducing #277. The runtime runner's real plugin-manager install check now runs for rc.3 as well as rc.2, and CI requires it for both.

Local macOS arm64 validation passed with pnpm 10.33.0 and Node 26 (CI uses Node 24): `pnpm install --frozen-lockfile`, typecheck, build, full `pnpm verify`, and `harness-runtime-verify.mjs` on rc.3 (296-package cohort) and rc.2 (295) from the same tarball. Both hosts passed all ten scenarios and the plugin-manager install without an exemption. Candidate SHA-256: `b33b258de4aee06bcdfc28770d446630a8ed83c3f4c2577bd5bc525078673770` (`0.1.26` sources, built locally). A copy with the development cohort and overrides moved to rc.3 also installed, typechecked and built cleanly.

Real provider credentials, native desktop interaction and migration of existing user data are outside the isolated fixture coverage. Do not equate these checks with a manual desktop acceptance test.

## Recovery

Existing published artifacts and tags are immutable. Until a release includes rc.3, stay on rc.2 with `0.1.26`; do not bypass host compatibility checks.
