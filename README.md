# Temporary Repo for NPM and Github Automated Release and other github workflows

This is a temporary repo, showcasing github and npm automated releases, automated issue labelers, and much more.

> Note: currently the existing older tags on github repo is not getting auto-released to github releases, when using semantic-release.

## GitHub Workflows Integrated

- [ ] Automated Issue labeler
- [ ] Automated GitHub and NPM releases

## Commands

- Stage the changes:

  ```sh
  git add .
  ```

- Commit the changes, not using `git commit -m`, but using below command for better standardized commit messages:

  ```sh
  npm run commit
  ```

- Push to remote repo:

  ```sh
  git push origin main
  ```

  This runs the github actions, which runs the linting, testing and then releases the package to github releases, as well as to npm, using `semantic-release`.

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

## Credits

- Youtube Video on [Fully Automated npm publish using GitHub Actions and Semantic Release](https://youtu.be/QZdY4XYbqLI)
