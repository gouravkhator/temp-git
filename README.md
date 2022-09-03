# Temporary Repo for NPM and Github Automated Release and other github workflows

## Other Workflows

- [ ] Automated Issue labeler
- [ ] Automated GitHub and NPM releases

## Upcoming things to try:

- To try in `package.json`:

  ```json
  {
    "plugins": [
      [
        "@semantic-release/git",
        {
          "assets": [
            "dist/**/*.{js,css,html,mjs,cjs,json}",
            "CHANGELOG.md",
            "package.json",
            "package-lock.json"
          ],
          "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
        }
      ]
    ]
  }
  ```
