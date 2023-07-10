# Configure react typescript with prettier, eslint, automatic testing and enforce with precommit
1. Scaffold the project 
> yarn create react-app mycoolapp --template typescript
2. Add lint and format scripts in package.json:
```json
{
    "scripts": {
  		"lint": "eslint src",
  		"fix": "eslint --fix src",
  		"format": "prettier --write \"src/**/*.ts\" \"src/**/*.tsx\"",
  		"prepare": "husky install"
    }
}
```
3. Add husky and lint-staged to package.json as root keys:
```json
{
  // The rest of your package.json, like name, version and scripts 
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "src/**/*.{js,jsx,ts,tsx}": [
      "npm run lint",
      "npm run format",
      "git add"
    ]
  }
}
```

4. Add `.prettierrc` in root directory:
```json
{
    "semi": true,
    "singleQuote": false,
    "printWidth": 90,
    "trailingComma": "es5",
    "tabWidth": 2,
    "useTabs": false
}
```
5. add `.eslintrc.json` in root directory:
```json
{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:@typescript-eslint/recommended",
        "plugin:react/recommended",
        "eslint-config-prettier"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "@typescript-eslint",
        "react",
        "eslint-plugin-prettier"
    ],
    "rules": {
        "indent": [
            "off"
        ],
        "linebreak-style": [
            "error",
            "unix"
        ],
        "quotes": [
            "error",
            "double"
        ],
        "semi": [
            "error",
            "always"
        ],
        "prettier/prettier": [
            "error"
        ],
        "no-console": "warn",
        "max-len": [
            "error",
            {
                "code": 90,
                "tabWidth": 2,
                "ignoreComments": true,
                "ignoreUrls": true
            }
        ],
        "@typescript-eslint/no-explicit-any": "error"
    },
    "settings": {
        "react": {
            "version": "detect"
        }
    }
}
```
6. install dev dependancies (make sure to do so in a git repository, just run `git init` if errors occour):
> yarn add @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-prettier eslint-plugin-prettier@^5.0.0-alpha.1 eslint-plugin-react husky lint-staged prettier -D


7. Run husky
> yarn prepare
8. Add hooks
> yarn husky add .husky/pre-commit "yarn format && yarn lint --max-warnings=0 && yarn test --watchAll=false"

9. Test by running:
> git add src && git ci -m 'test commit'

10. install prettier and eslint for vscode:
- https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode
- https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint
