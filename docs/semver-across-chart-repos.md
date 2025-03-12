# How do we manage Chart versions across this repo and `rancher/charts`?

Essentially, we must respect SemVer and understand how it interacts both:
a) within `ob-team-charts` and b) on the `rancher/charts` repo.

So first, the best ELI5, primer of SemVer for a refresher:
- A SemVer consists of: `core version`, `build metadata` (optional), `prerelease identifier` (optional),
- When SemVer encounters a `-<some details>` **first** it will consider the version a pre-release.
- When SemVer encounters a `+<some details>` **first** everything following `+` will be considered build metadata (even `-` characters).
- The `-` is considered part of `non-digit` characters to SemVer; it is valid within `<build>`
- The `+` is not valid anywhere other than when separating and identifying where `<build>` starts
- In other words, there are 3 forms of "extended SemVer" and they are:
    - `<semver core>-<prerelease>`
    - `<semver core>+<build>`
    - `<semver core>-<prerelease>+<build>`

With all that in mind, because we are adding a suffix in `ob-team-charts` we are using `-`.
Which means that when testing charts directly from `ob-team-charts` the "Pre-release Charts" feature has to be enabled.
(This is a per Rancher User setting if you test with multiple users, update all of them.)

This way, once we are ready to ship an update to `rancher/charts` the final version will look like:
`106.0.0+up{upstream}-rancher.{num}` - which is a valid SemVer with an optional `<build>` suffix.

In contrast, consider if we used `+` then when we suffix the full `ob-team-charts` version at `rancher/charts` it would produce
a version of `106.0.0+up{upstream}+rancher.{num}` which is invalid SemVer. In this scenario, we would have to adjust tooling
used for `rancher/charts` to modify `+` in the upstream version to be a legal character.

This led to our conclusion that we should continue using `-`