# Marketplace release controls

Merge the reviewed release commit to `main` and put the matching `vMAJOR.MINOR.PATCH` tag on that commit. Dispatch **Publish Extension** with that tag from `main` while that release commit is still its tip. The workflow verifies the exact commit and package version, rejects extension-host entry points, and defaults to a packaging-only run. A GitHub Release or tag push does not publish to a marketplace. A real publish requires setting `dry_run` to false and independent environment approval.

## Repository-owner setup before merging

- Protect `main`: require reviewed pull requests, restrict direct pushes and force pushes, and require code-owner review of publishing workflow changes. Limit bypass and review release-related repository roles.
- Restrict creation, modification and deletion of `v*` tags to authorized release managers; keep release tags immutable after creation.
- Create the `editor-marketplace-production` environment. Allow only the `main` branch, no tags, require a designated independent reviewer and disable self-review and administrator bypass wherever available.
- Provision narrowly scoped `VSCE_RELEASE_TOKEN` and `OVSX_RELEASE_TOKEN` **only** as environment secrets. Revoke the old publisher credentials and remove their repository/organisation secret grants before relying on the new boundary. Do not add replacements at repository or organisation scope. Confirm the existing publisher and namespace are active.

A workflow file does not configure the repository or environment policies. Verify them before enabling publication. If a gate is unavailable, stop rather than dispatching from another ref or publishing locally.
