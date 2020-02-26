# CRA with ESLint, Prettier, Typescript, Husky, Lint-Staged

### Test CRA version: **3.4.0**

## Step 1: Create CRA project with typescript

```console
npx create-react-app my-app --template typescript
```

## Step 2: Install dependencies

```console
yarn add -D eslint eslint-config-prettier husky lint-staged npm-run-all prettier
```

## Step 3: Setup Prettier

Create `.prettier` file to your project root folder. (
[Offical Website](https://prettier.io/playground/) )

## Step 4: Adjust ESLint

Adjust ESLint setting in package.json to avoid confilct bewtween ESlint and
prettier.

```json
{
  "eslintConfig": {
    "extends": ["react-app", "prettier"]
  }
}
```

## Step 5: Setup Husky and Lint-Staged

Add following scripts to package.json:

```json
{
  "scripts": {
    //...
    "lint": "eslint --ext .tsx,.ts src/",
    "check-types": "tsc",
    "prettier": "prettier --ignore-path .gitignore \"**/*.+(js|json|ts|tsx|css|html)\"",
    "format": "npm run prettier -- --write",
    "check-format": "npm run prettier -- --list-different",
    "validate": "run-p check-types check-format lint build"
  }
}
```

Then setup lint-staged to execute prettier and eslint on commited files. Add
following `"lint-staged"` section into package.json:

```json
{
  "lint-staged": {
    "*.+(js|ts|tsx)": ["eslint"],
    "**/*.+(js|json|ts|tsx|css|html)": ["prettier --write"]
  }
}
```

Finally, setup husky to execute scripts before any commit. Add folowing
`"husky"` section into package.json:

```json
{
  "husky": {
    "hooks": {
      "pre-commit": "npm run check-types && lint-staged && npm run build"
    }
  }
}
```
