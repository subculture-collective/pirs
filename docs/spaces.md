# Spaces and the Pirs roadmap

Status: assessment, 2026-11-21. It changes the roadmap as recorded in
[roadmap.md](roadmap.md) and the issues listed at the end. Nothing here has
been run against a PDS; it comes from reading the Spaces proposal and the
published alpha package.

## Why this matters

The community thread [Making the PDS extensible](https://discourse.atmosphere.community/t/making-the-pds-extensible/1304)
collects use cases for PDS plugins. Its most detailed reply (posts 9 and 10)
argues that most of them belong in separate services: atproto data is mostly
public, so running code next to the data gains little, and an MCP server can
already run outside the PDS with OAuth.

That argument has one stated exception: Spaces. Space data is not public. Every
application that reads a space keeps its own copy of it. A plugin that runs on
the account's own host can work on that data without another copy leaving the
host. That is the case for Pirs that an outside service cannot match, and the
roadmap should lean on it.

## What Spaces is, as of 2026-11-21

- [Proposal 0016, Atproto Spaces](https://github.com/bluesky-social/proposals/blob/main/0016-permissioned-data/README.md)
  is merged in `bluesky-social/proposals`. It is still a proposal; names and
  behaviour may change.
- `@atproto/pds` `latest` is 0.5.64 and contains no `com.atproto.space` code.
  The `alpha` dist-tag, `0.0.0-spaces-alpha-20261117141222`, implements the
  `com.atproto.space.*` methods and the three baseline policies.
- A space is identified by an authority DID, a space type NSID and a key. Each
  member keeps their records for a space in a space repo on their own PDS. The
  space authority issues short-lived credentials that let an application read
  the whole space.
- Spaces provide access control, not encryption. Any service that is allowed to
  read a space can read its data.
- Space writes do not go through the public event stream. In the alpha, the
  sequencer in `dist/sequencer/` contains no Spaces code. Instead, the repo host
  notifies the space host, which notifies registered syncers over HTTPS
  (`notifySpaceUpdate`). Syncers then pull changes with `listRepoOps`.

## What Spaces changes in the roadmap

### Granting a plugin access (#21)

Spaces adds a `space:` OAuth scope. It selects spaces by authority, type and
key. It limits writes to named collections and reads to the user's own repo
(`read_self`) or the whole space (`read`). Space type declarations give the
consent screen a readable title, and permission sets can bundle `space:` and
`repo:` permissions.

So the first option in #21, an OAuth grant issued to the host, now has an
upstream consent model for both public and space data. A grant format invented
by Pirs would not work on another PDS. The owner chose the OAuth direction on
2026-11-21:

- Each plugin is its own OAuth client, so the account holder approves each
  plugin separately on the standard consent screen.
- The host holds the tokens; the plugin never sees them, as #22 already
  requires.
- A memberspace can list allowed applications (`allowedApps`) and require a
  client attestation, which lets a space owner admit or refuse a plugin by its
  client ID.

#21 still needs a prototype. One check is open: #21 requires revocation to
take effect on the plugin's next call. With OAuth, that depends on whether the
PDS checks access tokens on every request or only when they are refreshed.

### The MCP plugin (#24, #54)

For public records, the thread is right that an MCP server outside the PDS does
the same job. #24 stays the M3 exit demo for public records. #54 extends the
same plugin to the account's personal space data (bookmarks, drafts, mutes)
with a `read_self` grant, once a stable PDS ships Spaces. That extension is the
reason to run MCP inside Pirs at all.

### Space events (#36, #52)

Pirs rule 4 says the PDS-to-host contract uses existing atproto mechanisms. For
spaces, that mechanism is the syncer flow: the host authorizes space sync,
registers a notification endpoint, and pulls `listRepoOps` from a cursor made
of the space revision and each repo revision. This needs no PDS patch.

Two constraints follow:

- Notifications are HTTPS calls to a public endpoint. The current bridge is a
  private Unix socket and plugin origins are loopback only
  ([ADR 0002](decisions/0002-plugin-http-origin.md)), so the host needs a public
  endpoint for this path.
- An application that syncs only space data still needs the public event
  stream for `#account` and `#identity` events. Pirs already has that from
  level 0.

#36 now covers only the specification, written from the alpha. Its existing
rule stands: no work against the alpha beyond reading it. #52 delivers space
events once a stable PDS ships Spaces.

### A decision hook without a PDS patch (#53)

A `managedspace` delegates read and writer-tracking decisions to a managing app
through `com.atproto.managedspace.checkAccess`. A Pirs plugin can answer that
method through the XRPC serving that #16 already provides, which allows
follower-only or subscriber-only spaces with no PDS patch.

The alpha gives the call a 10-second timeout and refuses access when the
managing app fails (`dist/space-policy/manager.js`, `checkAccess`). That is
upstream precedent for the failure rule #30 has to choose. The call goes
through the PDS's safe fetch, so the plugin needs a public endpoint here too.

### What Spaces does not change

- **Rejecting writes (#26).** Write permission in a space decides only whether
  the authority tracks and announces a writer. It does not stop an account
  writing to its own space repo. `record.validate` still needs the level 3
  patch.
- **Plugin storage (#19).** Per-account plugin settings could live in the
  account's personal space and move with the account. Any application granted
  that space type could read them, so secrets cannot go there. The host's
  key-value store stays.

### The patch base (#55)

The Pirs patch set is built on `@atproto/pds` 0.5.37; stable is 0.5.64. Spaces
will land on a later stable release, so #55 rebases the patch set onto the
current release before the level 3 hooks build on it.

## Prichal

Prichal hosts the unmodified upstream PDS, so it gets Spaces when upstream
ships them in a stable release, whether or not it ever runs Pirs. The work that
creates for Prichal (key custody, backup, export and capacity) is tracked in
[prichal#57](https://github.com/subculture-collective/prichal/issues/57).

## Issues changed on 2026-11-21

| Issue | Change |
| --- | --- |
| [#21](https://github.com/subculture-collective/pirs/issues/21) | OAuth direction chosen; prototype and decision record remain |
| [#24](https://github.com/subculture-collective/pirs/issues/24) | Scope limited to public records; space data moved to #54 |
| [#36](https://github.com/subculture-collective/pirs/issues/36) | Specification only, written from the alpha; no longer blocked by #31 |
| [#52](https://github.com/subculture-collective/pirs/issues/52) | New: deliver space writes to plugins through write notifications |
| [#53](https://github.com/subculture-collective/pirs/issues/53) | New: answer `managedspace` access checks from a plugin |
| [#54](https://github.com/subculture-collective/pirs/issues/54) | New: let the MCP plugin read the account's personal space data |
| [#55](https://github.com/subculture-collective/pirs/issues/55) | New: rebase the PDS patch set onto the current release |
