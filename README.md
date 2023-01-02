# My setting up for simple front-end projects

> **Warning**
>
> **Work in progress** 🚧

## Table of Contents

- [**0. About formatters and linters**](#0-about-formatters-and-linters)
- [**1. Git**](#1-git)
  - [1.1. Initialize a Git repository](#11-initialize-a-git-repository)
  - [1.2. Ignore files](#12-ignore-files)
  - [1.3. Normalize line endings](#13-normalize-line-endings)
- [**2. npm package**](#2-npm-package)
  - [2.1. npmrc](#21-npmrc)
- [**3. EditorConfig**](#3-editorconfig)
  - [3.1. The code editor plugin](#31-the-code-editor-plugin)
  - [3.2. The *per-repository, local* file](#32-the-per-repository-local-file)
- [**4. Code formatter: Prettier**](#4-code-formatter-prettier)
  - [4.1. Installation](#41-installation)
  - [4.2. Prettier and EditorConfig](#42-prettier-and-editorconfig)
  - [4.3. Usage](#43-usage)
- [**5. CSS linter: Stylelint**](#5-css-linter-stylelint)
  - [5.1. Installation](#51-installation)
  - [5.2. Configuration](#52-configuration)
  - [5.3. Usage](#53-usage)
- [**6. JavaScript linter: ESLint**](#6-javascript-linter-eslint)
  - [6.1. Installation](#61-installation)
  - [6.2. Configuration](#62-configuration)
  - [6.3. Usage](#63-usage)

## 0. About formatters and linters

- All configuration related to the editor (end of line, indent style, indent size...) should be handled by **EditorConfig**.
- Everything related to code formatting should be handled by **Prettier**.
- The code quality should be handled by **Stylelint** and **ESLint**.

References:

- [Why You Shoold Use ESLint, Prettier & EditorConfig](https://blog.theodo.com/2019/08/why-you-should-use-eslint-prettier-and-editorconfig-together/)

- [Set up ESlint, Prettier & EditorConfig without conflicts](https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/)

## 1. Git

> **Note**
>
> **Prerequisites: [Git installed](https://github.com/germanfrelo/my-frontend-web-development-setup#git)**

### 1.1. Initialize a Git repository

In the root of the project, execute:

```sh
git init
```

Output: `Initialized empty Git repository in /[...]/.git/`

### 1.2. Ignore files

1. Create a **`.gitignore`** file in the **root of the repository**.
2. Go to [gitignore.io](http://gitignore.io) to get templates specific for the project (e.g. `node, react`) and add the generated content into the `.gitignore` file. Example:

    ```gitignore
    # Ignore installed npm modules
    node_modules/
    ```

### 1.3. Normalize line endings

- **Reference**

    [https://www.aleksandrhovhannisyan.com/blog/crlf-vs-lf-normalizing-line-endings-in-git/](https://www.aleksandrhovhannisyan.com/blog/crlf-vs-lf-normalizing-line-endings-in-git/)

- **Short explanation**

    **Newline**, **line ending**, **end of line (EOL)** or **line break** is a control character or sequence of control characters in a character encoding specification (ASCII) that is used to signify the end of a line of text and the start of a new one.

  - **LF:** Line Feed character (`\n`)  →  Unix/Unix-like O.S.: Linux and macOS.
  - **CRLF:** Carriage Return (`\r`) + Line Feed (`\n`) characters (`\r\n`)  →  Windows.

    Newline characters often cause problems in Git when you have developers working on different operating systems (Windows, macOS and Linux).

    `CRLF` is redundant by today's standards. With a file, it suffices to define the newline character as implicitly doing the job of both a line feed and a carriage return under the hood; we have no need for an explicit carriage return *in addition* to a line feed—one symbol can do the job of both.

    By normalizing line ending, `CRLF` will be converted to `LF` for all text files checked into your repo (when you commit your changes), while leaving local line endings untouched in the working tree. When you push those files to your remote, they'll use `LF`. Anyone who later pulls or checks out that code will see `LF` line endings locally for those files.

    On summary, this is to assure that text files will use `LF` (and never `CRLF`) in the *remote* copy of your code.

1. **Create** a **`.gitattributes`** file in the **root of the repository** and add this content:

    ```gitattributes
    # Normalize line endings of all text files checked into the repo with LF
    * text=auto
    ```

2. **Commit** the file and **push** it to the repo.
3. After committing the file, the changes won't take effect immediately for files checked into Git *prior* to the addition of `.gitattributes`. To **force an update**, use:

    ```sh
    git add --renormalize .
    ```

    This updates all tracked files in your repo according to the rules defined in your `.gitattributes` config. If previously committed text files used `CRLF` in your repo and are converted to `LF` during the renormalization process, those files will be staged for a commit. You can then check if any files were modified like you would normally:

    ```sh
    git status
    ```

    The only thing left to do is to commit those changes (if any) and push them to your repo. In the future, anytime a new file is checked into Git, it'll use `LF` for line endings.

4. To **verify** that the files in your repo are using the correct line endings after all of these steps:

    ```sh
    git ls-files --eol
    ```

    For text files, you should see something like this:

    On macOS:

    ```sh
    i/lf    w/lf  attr/text=auto  file.txt
    ```

    On Windows:

    ```sh
    i/lf    w/crlf  attr/text=auto  file.txt
    ```

    Git Line Endings: Working Tree vs. Index

    The `text` attribute doesn't change line endings for the **local copies** of your text files (i.e., the ones in Git's working tree)—it only changes line endings for files in the repo. The text files you just renormalized may still continue to use `CRLF` locally (on your file system) if that's the line ending with which they were originally created/cloned on your system.

## 2. npm package

> **Note**
>
> **Prerequisites: [Node.js + npm](https://github.com/germanfrelo/my-frontend-web-development-setup#nodejs--npm)**

npm docs reference:
[https://docs.npmjs.com/cli/v8/commands/npm-init](https://docs.npmjs.com/cli/v8/commands/npm-init)

Set up a new npm package in the root of the project diectory:

```sh
npm init
```

This creates a `package.json` file like this:

```json
{
    "name": "",
    "version": "",
    "description": "",
    "keywords": [],
    "homepage": "",
    "bugs": {
        "url": ""
    },
    "license": "",
    "author": "",
    "main": "",
    "repository": {
        "type": "",
        "url": ""
    },
    "scripts": {
    },
    "dependencies": {
    },
    "devDependencies": {
    }
}
```

### 2.1. `.npmrc`

Set dependencies saved to `package.json` to be configured with an exact version rather than using npm's default semver range operator:

1. Create a file named `.npmrc` in the root of the project.

2. Add the following content and save it.

   ```ini
   # Dependencies saved to package.json will be configured with an exact version rather than using npm's default semver range operator
   save-exact=true
   ```

## 3. EditorConfig

[editorconfig.org](https://editorconfig.org)

### 3.1. The code editor plugin

Install [the EditorConfig plugin/extension for your Code Editor](https://editorconfig.org/#download):

- For Visual Studio Code: [EditorConfig extension for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

### 3.2. The *per-repository, local* file

> **Note**
>
> **Recommended prerequisites: [a global `.editorconfig` file](https://github.com/germanfrelo/my-frontend-web-development-setup#the-personal-global-file).**

Create an `.editorconfig` file in the root of the repository. This file:

- Is for team settings.
- Gets committed to source control.
- Should **only include** properties that affect the physical contents of the file, and that you want enforced in the repository, not just how it appears in an editor.
- Should **not include** any presentation-only properties, such as `indent_size` or `tab_width`.
- Should have `root = false` defined, so that presentation-only (and other) properties can be inherited from [a personal, global `.editorconfig` file](https://github.com/germanfrelo/my-frontend-web-development-setup#the-personal-global-file).

This is the list of [EditorConfig properties](https://editorconfig-specification.readthedocs.io/#supported-pairs).

See **[my local `.editorconfig` file](https://gist.github.com/germanfrelo/a71698d5c4592220a0fa4915f32182ce)**.

## 4. Code formatter: Prettier

[prettier.io](https://prettier.io)

### 4.1. Installation

**Step 1:**

Install and configure [the Prettier plugin/extension for your Code Editor](https://prettier.io/docs/en/editors.html):

- For Visual Studio Code, see [my installation and configuration of the Prettier extension for VS Code](https://github.com/germanfrelo/my-frontend-web-development-setup#prettier-extension-for-vscode).

**Step 2:**

Install Prettier in the project (locally, as a dev dependency, and with its version pinned):

> **Note**
>
> **Prerequisites: [a `package.json` file](#2-npm-package).**

```sh
npm install prettier --save-dev --save-exact
```

Verify that it is added as `devDependencies` in the `package.json` file:

```json
{
    "devDependencies": {
        "prettier": "x.x.x"
    }
}
```

> **Note**
>
> **Prettier implicit ignore rules:**
>
> By default, Prettier ignores:
>
> - files in version control systems directories (".git", ".svn" and ".hg")
> - `node_modules`
>
> ```gitignore
> **/.git
> **/.svn
> **/.hg
> **/node_modules
> ```

### 4.2. Prettier and EditorConfig

The objective is to avoid redundant configuration between EditorConfig and Prettier.

**Prettier and EditorConfig share some configuration options that we do *not* want to repeat in two separate configuration files and keep them in sync** (e.g. end of line configuration).

The latest versions of Prettier address this issue by parsing the `.editorconfig` file to determine what configuration options to use. Those (EditorConfig) options are limited to:

```editor-config
end_of_line
indent_style
indent_size/tab_width
max_line_length
```

The previous (EditorConfig) configuration options will override the following Prettier options (if they are not defined in the `.prettierrc`):

```json
"endOfLine"
"useTabs"
"tabWidth"
"printWidth"
```

**The previous configuration options should be written only in `.editorconfig`.**

If you wish to change the configuration, **the rule is to check whether it is a EditorConfig or Prettier relevant configuration and then change it in the appropriate file**.

### 4.3. Usage

- [ ] To-do

## 5. CSS linter: Stylelint

[Stylelint](https://stylelint.io).

### 5.1. Installation

Install Stylelint in the root of the project (locally, as a dev dependency, and with its version pinned):

```sh
npm install -D -E stylelint
```

#### Editor integrations

It's also recommended to use the [Stylelint extension for the code editor](https://stylelint.io/user-guide/integrations/editor).

- For VS Code: [my installation and configuration of the Stylelint extension for VS Code](https://github.com/germanfrelo/my-frontend-web-development-setup#stylelint-extension-for-vscode).

### 5.2. Configuration

Create a `.stylelintrc.json` configuration file in the root of the project.

> No rules are turned on by default and there are no default values. You must explicitly configure each rule to turn it on.

There is a [list of existing configurations](https://github.com/stylelint/awesome-stylelint#configs) that turn on some rules. The ones that I use are:

- [`stylelint-config-standard`](https://www.npmjs.com/package/stylelint-config-standard)
- [`stylelint-config-recess-order`](https://www.npmjs.com/package/stylelint-config-recess-order)
- [`stylelint-config-prettier`](https://www.npmjs.com/package/stylelint-config-prettier)

#### 5.2.1. Standard configuration

> Extends recommended one by turning on rules that enforce conventions.

Install [`stylelint-config-standard`](https://www.npmjs.com/package/stylelint-config-standard) in the root of the project (locally, as a dev dependency, and with its version pinned):

```sh
npm install -D -E stylelint-config-standard
```

Then, add `"stylelint-config-standard"` to the `"extends"` array in `.stylelintrc.json`:

```json
{
    "extends": [
        "stylelint-config-standard"
    ]
}
```

#### 5.2.2. Recess order

> Sorts CSS properties the way Recess did and Bootstrap did/does. *With some modifications & additions for modern properties.

Install [`stylelint-config-recess-order`](https://www.npmjs.com/package/stylelint-config-recess-order) in the root of the project (locally, as a dev dependency, and with its version pinned):

```sh
npm install -D -E stylelint-config-recess-order
```

Then, add `"stylelint-config-recess-order"` to the `"extends"` array in `.stylelintrc.json`:

```json
{
    "extends": [
        "...",
        "stylelint-config-recess-order"
    ]
}
```

#### 5.2.3. Stylelint and Prettier

Last but not least, turn off all Stylelint rules that are unnecessary or might conflict with Prettier.

Install [`stylelint-config-prettier`](https://www.npmjs.com/package/stylelint-config-prettier) in the root of the project (locally, as a dev dependency, and with its version pinned):

```sh
npm install -D -E stylelint-config-prettier
```

Then, append `"stylelint-config-prettier"` to the `"extends"` array in the `.stylelintrc.json` file. Make sure to put it **last**, so it will override other configs:

```json
{
    "extends": [
      "...",
      "stylelint-config-prettier"
    ]
}
```

This config is shipped with a CLI tool to check if the Stylelint configuration contains any rules that are in conflict with Prettier.

Add the following script to `package.json`:

```json
"scripts": {
    "stylelint-check": "stylelint-config-prettier-check"
}
```

The, run:

```sh
npm run stylelint-check
```

#### 5.2.4. Rules to override

These are the rules that I use to override the existing configurations:

```json
"rules": {
    "indentation": "tab",
    "no-missing-end-of-source-newline": null,
    "custom-media-pattern": null,
    "custom-property-pattern": null,
    "keyframes-name-pattern": null,
    "selector-class-pattern": null,
    "selector-id-pattern": null,
    "value-keyword-case": [
        "lower",
        {
            "ignoreKeywords": [
                "Arial",
                "Consolas",
                "Helvetica",
                "Menlo",
                "Roboto",
                "SFMono-Regular"
            ],
            "ignoreProperties": [
                "--ff-sans",
                "--ff-mono"
            ]
        }
    ],
    "max-line-length": [
        120,
        {
            "ignorePattern": []
        }
    ]
}
```

### 5.3. Usage

On the command line using an npm script.

First, create the script in `package.json`:

> **Note**:
> `\"` is needed to escape the quotation marks around file globs.

```json
"scripts": {
    "lint:css": "stylelint \"**/*.css\" --formatter verbose"
}
```

> **Note**:
> The [`--formatter` option](https://stylelint.io/user-guide/usage/options#formatter) is optional.

Then, run the script command on the CLI:

```sh
npm run lint:css
```

## 6. JavaScript linter: ESLint

[ESLint](https://eslint.org).

### 6.1. Installation

**Step 1:**

Install and configure [the ESLint plugin/extension for your Code Editor](https://eslint.org/docs/user-guide/integrations#editors):

- For Visual Studio Code, see [my installation and configuration of the ESLint extension for VS Code](https://github.com/germanfrelo/my-frontend-web-development-setup#eslint-extension-for-vscode).

**Step 2:**

Install ESLint in the project (locally, as a dev dependency, and with its version pinned):

> **Note**
>
> **Prerequisites: [a `package.json` file](#2-npm-package).**

```sh
npm install eslint --save-dev --save-exact
```

Verify that it is added as `devDependencies` in the `package.json` file:

```json
{
    "devDependencies": {
        "eslint": "x.x.x"
    }
}
```

### 6.2. Configuration

```sh
npm init @eslint/config
```

Steps to answer:

- How would you like to use ESLint? · problems

- Where does your code run? · browser, node

- What format do you want your config file to be in? · JSON

Output:

```sh
# Successfully created .eslintrc.json configuration file in ...
```

This creates a `.eslintrc.json` configuration file in the project's root directory, like this:

```json
{
    "env": {
        "browser": true,
        "es2021": true,
        "node": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": "latest"
    },
    "rules": {}
}
```

> **Note**
>
> **ESLint implicit ignore rules:**

By default, ESLint ignores:

- dot-files (except for `.eslintrc.*`), as well as dot-folders and their contents
- `node_modules/`

#### 6.2.1. ESLint and Prettier

Turn off all ESLint rules that are unnecessary or might conflict with Prettier (code formatting rules)

**Step 1:**

Install the Prettier's [`eslint-config-prettier`](https://github.com/prettier/eslint-config-prettier) package in the project (locally, as a dev dependency, and with its version pinned):

```sh
npm install eslint-config-prettier --save-dev --save-exact
```

Verify that it is added as `devDependencies` in the `package.json` file:

```json
{
    "devDependencies": {
        "eslint": "x.x.x",
        "eslint-config-prettier": "x.x.x",
        "prettier": "x.x.x"
    }
}
```

**Step 2:**

Add `prettier` to the `extends` array in the `.eslintrc.json` file:

```json
{
    "extends": ["eslint:recommended", "prettier"],
}
```

**Make sure to put it last**, so that it will override any prior configuration in the `extends` array, disabling all ESLint code formatting rules. With this configuration, Prettier and ESLint can be run separately without any issues.

**Step 3:**

Finally, remove any code formatting rules that you might have in the `.eslintrc.json` file:

- If there are no `rules`, skip this step.

- If there are `rules`, to find if any of them are unnecessary or conflict with Prettier, run the [CLI helper tool](https://github.com/prettier/eslint-config-prettier#cli-helper-tool).

My code quality ESLint rules:

```json
{
    "rules": {

    }
}
```

### 6.3. Usage

- **Option 1:**  CLI

    ```sh
    npx eslint .
    ```

    The lines that have formatting errors will be marked by `prettier/prettier` as errors within the ESLint error output.

    To fix errors:

    ```sh
    npx eslint --fix .
    ```

    The files will get formatted the same way Prettier did.

- **Option 2:**  Script

    Add this to the `package.json` file:

    ```json
    "scripts": {
        "lint:js": "eslint ."
    }
    ```

    Run:

    ```sh
    npm run lint:js
    ```
