# Recommended

- use `pnpm` or `yarn` if available (`npm` if no choice)
- all `npm` commands here work with `pnpm` (`npx` == `pnpm dlx`)

# Express

```bash
npm i express
npm i -D nodemon cors dotenv
```

# Airbnb Style for JS/TS (Eslint and Prettier)

- for pure nodejs projects (no any frontend frameworks like react)

1. Install devDependencies:

```bash
npm i -D eslint prettier eslint-config-node eslint-config-prettier eslint-plugin-prettier eslint-plugin-prettier 

```

2. Install `eslint-config-airbnb`

```bash
npx install-peerdeps --dev eslint-config-airbnb
```

3. Generate `.eslintrc.json`

```bash
npm init @eslint/config@latest
# or npx eslint --init
```

4. Setup eslint config

```json
{
    "globals": {
        "node": true
    },
    "extends": ["airbnb", "prettier", "plugin:node/recommended"],
    "plugins": ["prettier"],
    "rules": {
        "prettier/prettier": "error",
        "no-unused-vars": "warn",
        "no-console": "off",
        "func-names": "off",
        "no-process-exit": "off",
        "object-shorthand": "off",
        "class-methods-use-this": "off"
    }
}
```

5. Generate `.prettierrc.json`

```json
{
    "trailingComma": "all",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": false,
    "printWidth": 120,
    "useTabs": false
}

```

