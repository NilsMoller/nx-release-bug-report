# Bug report for nx release

## Full reproduction steps

This repository is already at step 7 (with step 6 being completed already).

The steps taken to reproduce the issue:

1. Create a new nx monorepo: `npx create-nx-workspace@20.5.0-beta.4`
2. Add the nx/js package: `nx add @nx/js`
3. Generate a library to publish: `nx g @nx/js:lib packages/my-package`
4. Add the following release config to `nx.json`:

```json
"release": {
  "releaseTagPatternCheckAllBranchesWhen": true,
  "git": {
    "commitMessage": "chore: bump versions [skip ci]"
  },
  "version": {},
  "changelog": {
    "projectChangelogs": true
  },
  "groups": {
    "packages": {
      "projectsRelationship": "independent",
      "projects": [
        "packages/my-package"
      ],
      "releaseTagPattern": "release/package/{projectName}/{version}"
    }
  }
}
```

5. Run `nx release --skip-publish --first-release` to successfully generate the first release.
6. Change some code in the library and commit it.
7. Run `nx release --skip-publish` to generate a new release.
8. Watch the failure output in the console.
