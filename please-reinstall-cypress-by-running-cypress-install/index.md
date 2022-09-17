```metadata
{
    "title": "Please reinstall Cypress by running: cypress install",
    "date": "2022-01-11T12:07:27",
    "categories": "Cypress, JavaScript, Node.js, TypeScript",
}
```

cypressを起動しようとしたところタイトルのエラーが発生しました

```json
 No version of Cypress is installed in: /Users/masahirookubo/Library/Caches/Cypress/8.7.0/Cypress.app

Please reinstall Cypress by running: cypress install

----------

Cypress executable not found at: /Users/masahirookubo/Library/Caches/Cypress/8.7.0/Cypress.app/Contents/MacOS/Cypress

----------

Platform: darwin (20.6.0)
Cypress Version: 8.7.0
error Command failed with exit code 1.
info Visit https://yarnpkg.com/en/docs/cli/run for documentation about this command.

```

yarnでcypressを強制installすることで対応しました🙌

```json
 yarn run cypress install --force
```

## 参考記事

[Cypress failed to start on Windows](https://stackoverflow.com/questions/48324493/cypress-failed-to-start-on-windows)
