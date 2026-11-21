# Roadmap

Status: foundation demonstrations have run and the owner selected WIT components on 2026-10-03. The separate host passed adversarial checks, and retained PDS events passed local resumable delivery qualification on October 4. Section 8 of the spec and the qualification records describe the evidence. Tracking issue: [#40](https://github.com/subculture-collective/pirs/issues/40). The roadmap was revised for Spaces on 2026-11-21; [spaces.md](spaces.md) gives the reasons.

Pirs aims to define how a PDS is extended and to prove it with a working distribution. The milestones follow the levels in the [hook-point spec](spec/hook-points.md): observe, serve, act, decide. M1 to M4 each end with a plugin that could not have been built at the level before.

Each issue is one demonstrable behaviour with its own acceptance criteria and verification. `ready-for-agent` means scoped for implementation once its blockers close. `needs-decision` means the owner has a choice to make. P0 gates its milestone, P1 completes it, P2 is later.

No dates are assigned. The first two milestones will show how fast this goes.

Issue status checked against GitHub on 2026-11-21: **16 of 39 issues closed**. Issue links point to GitHub, where the issues moved on 2026-11-10; each GitHub number is the Gitea number plus 3.
☑ means closed; ☐ means open. Issue closure and milestone exit-demonstration
qualification are tracked separately.

## Milestones

### M0 - Foundation

CI runs the smoke check, real account events are verified locally, the image builds, an upstream merge is rehearsed and the plugin interface is chosen.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☑ Closed | [#5](https://github.com/subculture-collective/pirs/issues/5) Run the smoke check in Gitea CI | P0 | none |
| ☑ Closed | [#6](https://github.com/subculture-collective/pirs/issues/6) Deliver commit and account events from a real local account | P0 | none |
| ☑ Closed | [#7](https://github.com/subculture-collective/pirs/issues/7) Build and boot the Pirs container image | P0 | none |
| ☑ Closed | [#8](https://github.com/subculture-collective/pirs/issues/8) Rehearse an upstream release merge | P1 | [#5](https://github.com/subculture-collective/pirs/issues/5) |
| ☑ Closed | [#9](https://github.com/subculture-collective/pirs/issues/9) Choose the plugin interface with a measured spike | P0 | none |

### M1 - Observe

A sandboxed plugin in a separate host process receives PDS events with enforced limits. Exit demo: a commit webhook plugin.

The [commit webhook reference](qualification/commit-webhook.md) has source and
component checks. Its required successful local receiver demo conflicts with
#13's loopback/private destination prohibition and awaits an owner decision.
M1 has not passed its exit demonstration.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☑ Closed | [#10](https://github.com/subculture-collective/pirs/issues/10) Run one plugin in a separate host process with enforced limits | P0 | [#9](https://github.com/subculture-collective/pirs/issues/9) |
| ☑ Closed | [#11](https://github.com/subculture-collective/pirs/issues/11) Deliver PDS events to the host with a resumable cursor | P0 | [#10](https://github.com/subculture-collective/pirs/issues/10), [#6](https://github.com/subculture-collective/pirs/issues/6) |
| ☑ Closed | [#12](https://github.com/subculture-collective/pirs/issues/12) Check a plugin manifest against its imports at install | P0 | [#10](https://github.com/subculture-collective/pirs/issues/10) |
| ☑ Closed | [#13](https://github.com/subculture-collective/pirs/issues/13) Grant outbound requests only to allowed hosts | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☑ Closed | [#14](https://github.com/subculture-collective/pirs/issues/14) Ship a commit webhook plugin | P0 | [#11](https://github.com/subculture-collective/pirs/issues/11), [#13](https://github.com/subculture-collective/pirs/issues/13) |
| ☐ Open | [#15](https://github.com/subculture-collective/pirs/issues/15) Run level 0 against an unmodified PDS | P1 | [#11](https://github.com/subculture-collective/pirs/issues/11) |

### M2 - Serve

Plugins answer XRPC methods and HTTP routes and read public records and blobs. Exit demo: a static site hosting plugin.

The [local static site demonstration](qualification/static-site.md) ran on
2026-10-10: a real disposable account published records/blobs, and an installed
component served a committed page update without restarting the PDS or host.
Its browser evidence covers #17's scriptless local HTML isolation. All eight
hosted checks passed at `3878de5`, including the installed site checks, before
[PR #57](https://git.subcult.tv/subculture-collective/pirs/pulls/57) merged.
Public hosting remains outside this local demonstration's scope.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☑ Closed | [#16](https://github.com/subculture-collective/pirs/issues/16) Serve plugin XRPC methods through service proxying | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☑ Closed | [#17](https://github.com/subculture-collective/pirs/issues/17) Serve plugin HTTP routes without sharing the PDS's origin | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☑ Closed | [#18](https://github.com/subculture-collective/pirs/issues/18) Let plugins read public records and blobs | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☑ Closed | [#19](https://github.com/subculture-collective/pirs/issues/19) Give each plugin quota-bound key-value storage | P1 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☑ Closed | [#20](https://github.com/subculture-collective/pirs/issues/20) Ship a static site hosting plugin | P0 | [#17](https://github.com/subculture-collective/pirs/issues/17), [#18](https://github.com/subculture-collective/pirs/issues/18) |

### M3 - Act

An account holder grants a plugin scoped access and the plugin writes records for them under operator policy. Exit demo: an MCP server plugin.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☐ Open | [#21](https://github.com/subculture-collective/pirs/issues/21) Prototype plugin grants as OAuth clients with `repo:` and `space:` scopes | P0 | [#16](https://github.com/subculture-collective/pirs/issues/16) |
| ☐ Open | [#22](https://github.com/subculture-collective/pirs/issues/22) Write records for an account under a scoped grant | P0 | [#21](https://github.com/subculture-collective/pirs/issues/21) |
| ☐ Open | [#23](https://github.com/subculture-collective/pirs/issues/23) Install, disable and remove plugins under operator policy | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☐ Open | [#24](https://github.com/subculture-collective/pirs/issues/24) Ship an MCP server plugin for an account's records | P0 | [#22](https://github.com/subculture-collective/pirs/issues/22), [#17](https://github.com/subculture-collective/pirs/issues/17) |

### M4 - Decide

A small tested patch set lets plugins reject requests before the PDS acts. Exit demo: a generic uniqueness rule replaces the hardcoded like, repost and follow dedup.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☑ Closed | [#25](https://github.com/subculture-collective/pirs/issues/25) Carry a tested patch set against the PDS package | P0 | [#8](https://github.com/subculture-collective/pirs/issues/8) |
| ☐ Open | [#26](https://github.com/subculture-collective/pirs/issues/26) Accept or reject record writes through `record.validate` | P0 | [#25](https://github.com/subculture-collective/pirs/issues/25), [#10](https://github.com/subculture-collective/pirs/issues/10) |
| ☐ Open | [#27](https://github.com/subculture-collective/pirs/issues/27) Replace the hardcoded like, repost and follow dedup with `record.unique` | P0 | [#26](https://github.com/subculture-collective/pirs/issues/26) |
| ☐ Open | [#28](https://github.com/subculture-collective/pirs/issues/28) Gate account creation through `account.create` | P1 | [#26](https://github.com/subculture-collective/pirs/issues/26) |
| ☐ Open | [#29](https://github.com/subculture-collective/pirs/issues/29) Expose sign-in and authorisation checks as `auth.session` | P1 | [#25](https://github.com/subculture-collective/pirs/issues/25) |
| ☐ Open | [#30](https://github.com/subculture-collective/pirs/issues/30) Define ordering and failure rules for several deciding plugins | P0 | [#26](https://github.com/subculture-collective/pirs/issues/26) |
| ☐ Open | [#55](https://github.com/subculture-collective/pirs/issues/55) Rebase the PDS patch set onto the current release | P1 | none |

### M5 - Standard

Spec draft 1 is published with a conformance suite, plugins are published as atproto records, and a second PDS implementation is tested.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☐ Open | [#31](https://github.com/subculture-collective/pirs/issues/31) Publish spec draft 1 from implementation evidence | P0 | [#15](https://github.com/subculture-collective/pirs/issues/15), [#16](https://github.com/subculture-collective/pirs/issues/16), [#21](https://github.com/subculture-collective/pirs/issues/21), [#30](https://github.com/subculture-collective/pirs/issues/30) |
| ☐ Open | [#32](https://github.com/subculture-collective/pirs/issues/32) Provide a conformance suite per level | P0 | [#31](https://github.com/subculture-collective/pirs/issues/31) |
| ☐ Open | [#33](https://github.com/subculture-collective/pirs/issues/33) Publish and install plugins as atproto records compatible with EmDash's registry | P0 | [#12](https://github.com/subculture-collective/pirs/issues/12) |
| ☐ Open | [#34](https://github.com/subculture-collective/pirs/issues/34) Run levels 0 and 1 against a second PDS implementation | P1 | [#32](https://github.com/subculture-collective/pirs/issues/32) |
| ☐ Open | [#35](https://github.com/subculture-collective/pirs/issues/35) Take the proposal to the community thread | P1 | [#31](https://github.com/subculture-collective/pirs/issues/31) |
| ☐ Open | [#36](https://github.com/subculture-collective/pirs/issues/36) Specify how plugins see Spaces data | P1 | none |
| ☐ Open | [#52](https://github.com/subculture-collective/pirs/issues/52) Deliver Space writes to plugins through write notifications | P1 | [#36](https://github.com/subculture-collective/pirs/issues/36), stable Spaces release |
| ☐ Open | [#53](https://github.com/subculture-collective/pirs/issues/53) Answer managedspace access checks from a plugin | P1 | [#36](https://github.com/subculture-collective/pirs/issues/36), stable Spaces release |
| ☐ Open | [#54](https://github.com/subculture-collective/pirs/issues/54) Let the MCP plugin read the account's personal Space data | P1 | [#24](https://github.com/subculture-collective/pirs/issues/24), [#36](https://github.com/subculture-collective/pirs/issues/36), stable Spaces release |

### M6 - Prichal runtime

The host is threat-modelled and reviewed, Pirs qualifies as an opt-in Prichal runtime, and the marketplace decision is made.

| Status | Issue | Priority | Blocked by |
| --- | --- | --- | --- |
| ☐ Open | [#37](https://github.com/subculture-collective/pirs/issues/37) Threat-model the host and review it before hosted use | P0 | [#23](https://github.com/subculture-collective/pirs/issues/23), [#30](https://github.com/subculture-collective/pirs/issues/30) |
| ☐ Open | [#38](https://github.com/subculture-collective/pirs/issues/38) Qualify Pirs as an opt-in Prichal runtime | P1 | [#37](https://github.com/subculture-collective/pirs/issues/37), [#8](https://github.com/subculture-collective/pirs/issues/8) |
| ☐ Open | [#39](https://github.com/subculture-collective/pirs/issues/39) Decide whether to open a marketplace | P2 | [#33](https://github.com/subculture-collective/pirs/issues/33), [#38](https://github.com/subculture-collective/pirs/issues/38) |

## Order of work

The shortest path to a running plugin is [#9](https://github.com/subculture-collective/pirs/issues/9), [#10](https://github.com/subculture-collective/pirs/issues/10), [#6](https://github.com/subculture-collective/pirs/issues/6), [#11](https://github.com/subculture-collective/pirs/issues/11), [#12](https://github.com/subculture-collective/pirs/issues/12), [#13](https://github.com/subculture-collective/pirs/issues/13), [#14](https://github.com/subculture-collective/pirs/issues/14). [#5](https://github.com/subculture-collective/pirs/issues/5) and [#7](https://github.com/subculture-collective/pirs/issues/7) have no blockers and can run alongside.

After [#12](https://github.com/subculture-collective/pirs/issues/12), the serve issues ([#16](https://github.com/subculture-collective/pirs/issues/16) to [#19](https://github.com/subculture-collective/pirs/issues/19)), operator policy ([#23](https://github.com/subculture-collective/pirs/issues/23)) and registry records ([#33](https://github.com/subculture-collective/pirs/issues/33)) are independent of each other.

The patch track starts at [#8](https://github.com/subculture-collective/pirs/issues/8) and [#25](https://github.com/subculture-collective/pirs/issues/25) and does not depend on M2 or M3. Its first hook, [#26](https://github.com/subculture-collective/pirs/issues/26), also needs the host from [#10](https://github.com/subculture-collective/pirs/issues/10).

Milestone order is not a blocker by itself. Only the links in the tables are.

## Owner decisions

| Status | Issue | Decision |
| --- | --- | --- |
| ☑ Closed | [#9](https://github.com/subculture-collective/pirs/issues/9) | WIT components selected on 2026-10-03 after the measured spike |
| ☐ Open | [#21](https://github.com/subculture-collective/pirs/issues/21) | Per-server or per-account installation. The grant mechanism was chosen on 2026-11-21: OAuth with upstream `repo:` and `space:` scopes |
| ☐ Open | [#31](https://github.com/subculture-collective/pirs/issues/31) | Whether to make the repository public, and the licence for the spec text |
| ☐ Open | [#35](https://github.com/subculture-collective/pirs/issues/35) | The text posted to the community thread and the account it comes from |
| ☐ Open | [#38](https://github.com/subculture-collective/pirs/issues/38) | Whether Prichal offers Pirs as an opt-in runtime |
| ☐ Open | [#39](https://github.com/subculture-collective/pirs/issues/39) | Whether to open a marketplace |

## Risks

- **Upstream churn.** `@atproto/pds` released about 1.8 times a week between April and October 2026, and `context.ts`, which a patch set must touch, was its most-changed file. [#8](https://github.com/subculture-collective/pirs/issues/8) and [#25](https://github.com/subculture-collective/pirs/issues/25) measure the real cost before M4 commits to it.
- **Spaces.** On 2026-11-21 Spaces exist only in the `@atproto/pds` alpha (`0.0.0-spaces-alpha-20261117141222`); stable 0.5.64 has none. Space writes bypass the event stream and reach syncers over public HTTPS. [#36](https://github.com/subculture-collective/pirs/issues/36) specifies the plugin contract from the alpha now. [#52](https://github.com/subculture-collective/pirs/issues/52), [#53](https://github.com/subculture-collective/pirs/issues/53) and [#54](https://github.com/subculture-collective/pirs/issues/54) wait for a stable release. Work before then should avoid assuming one repository per account. See [spaces.md](spaces.md).
- **No upstream signal.** Bluesky has not said it would accept hook points. The plan assumes Pirs carries its patches indefinitely.
- **Toolchain.** Nobody in the atproto ecosystem uses the WebAssembly component model yet. [#9](https://github.com/subculture-collective/pirs/issues/9) decides on measurements, not preference.
- **A standard needs a second implementer.** [#34](https://github.com/subculture-collective/pirs/issues/34) and [#35](https://github.com/subculture-collective/pirs/issues/35) test whether anyone else can and will adopt the contract. If they fail, Pirs is a product feature, not a standard.

## Evidence

A milestone closes on demonstrations, not on merged code. Section 8 of the spec lists what has been run and is updated whenever a hook gains or loses evidence. Local runs do not prove behaviour on a public hostname, with real identities, or under hostile load.
