# git-releases

The [Git Core PPA](https://launchpad.net/~git-core/+archive/ubuntu/ppa)'s `git`
builds for Ubuntu 24.04 (noble), republished unchanged as GitHub releases. The
agent-devcontainer image installs git from these releases, so building it
depends on GitHub alone.

The PPA keeps only its newest build, so the
[`cache-ppa-release`](.github/workflows/cache-ppa-release.yml) workflow checks it
daily and publishes each new one. A run can also be started by hand.

## Release contents

A release is tagged after the package version, without its epoch and with `~`
spelled `-`: `1:2.56.0-0ppa1~ubuntu24.04.2` becomes
`v2.56.0-0ppa1-ubuntu24.04.2`. It carries:

- `git_<version>_amd64.deb` and `git_<version>_arm64.deb`;
- `git-man_<version>_all.deb`, which `git` requires at the same version;
- `SHA256SUMS`.

The release notes list each file's PPA URL and SHA-256.

## Verification

[`scripts/cache_ppa_release.py`](scripts/cache_ppa_release.py) accepts the PPA's
`InRelease` index only with a valid signature by the PPA key pinned in
[`keys/git-core-ppa.asc`](keys/git-core-ppa.asc)
(`F911AB184317630C59970973E363C90F8F1B6217`). Each `Packages` index and `.deb`
must hash to the SHA-256 the signed index chains to. A release that already
exists is never replaced.

`python3 -I scripts/cache_ppa_release.py --dry-run` runs every check and prints
the release notes without publishing.

GitHub disables scheduled workflows in a public repository after 60 days without
activity; re-enable the workflow from the Actions tab if that happens.
