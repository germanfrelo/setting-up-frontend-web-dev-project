# My setting up for simple front-end projects <!-- omit from toc -->

> [!NOTE]
> This is a **work in progress**.

## Table of Contents <!-- omit from toc -->

- [0. About formatters and linters](#0-about-formatters-and-linters)
- [1. Git](#1-git)
  - [1.1. Initialize a Git repository](#11-initialize-a-git-repository)
  - [1.2. Ignore files](#12-ignore-files)
  - [1.3. Normalize line endings](#13-normalize-line-endings)
- [2. npm package](#2-npm-package)
  - [2.1. `.npmrc`](#21-npmrc)
- [3. EditorConfig](#3-editorconfig)
  - [3.1. The code editor plugin](#31-the-code-editor-plugin)
  - [3.2. The *per-repository, local* file](#32-the-per-repository-local-file)
- [4. Code formatter: Prettier](#4-code-formatter-prettier)
  - [4.1. Installation](#41-installation)
  - [4.2. Prettier and EditorConfig](#42-prettier-and-editorconfig)
  - [4.3. Usage](#43-usage)
- [5. CSS linter: Stylelint](#5-css-linter-stylelint)
  - [5.1. Installation](#51-installation)
    - [Editor integrations](#editor-integrations)
  - [5.2. Configuration](#52-configuration)
    - [Rules](#rules)
    - [Existing configurations](#existing-configurations)
  - [5.3. Usage](#53-usage)
- [6. JavaScript linter](#6-javascript-linter)

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

Set up a new npm package in the root of the project directory:

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

➡️ See **[my local `.editorconfig` file](https://gist.github.com/germanfrelo/a71698d5c4592220a0fa4915f32182ce)**.

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

It's also recommended to use a [Stylelint extension for the code editor](https://stylelint.io/user-guide/integrations/editor).

- For VS Code: [my installation and configuration of the Stylelint extension for VS Code](https://github.com/germanfrelo/my-frontend-web-development-setup#stylelint-extension-for-vscode).

### 5.2. Configuration

Create a `.stylelintrc.json` configuration file in the root of the project.

#### Rules

3 categories:

- Rules that **[avoid errors](https://stylelint.io/user-guide/rules#avoid-errors)**
- Rules that **[enforce non-stylistic conventions](https://stylelint.io/user-guide/rules#enforce-non-stylistic-conventions)**
- Rules that **[enforce stylistic conventions](https://stylelint.io/user-guide/rules#enforce-stylistic-conventions)** (frozen: will be deprecated and removed; use Prettier instead)

> **Note**
> No rules are turned on by default and there are no default values. You must explicitly configure each rule to turn it on.

#### Existing configurations

There is a [list of existing configurations](https://github.com/stylelint/awesome-stylelint#configs) that turn on some rules.

To use (*extend*) one (or more), first add an `"extends"` array to `.stylelintrc.json`:

```json
{
    "extends": []
}
```

Then, add the existing configuration(s) to the array.

> **Warning**
> The order matters! Each item in the array takes precedence over the previous item (so the second item overrides rules in the first, the third item overrides rules in the first and the second, and so on; the last item overrides everything else).

The configurations that I use are:

1. **[`stylelint-config-recommended`](https://www.npmjs.com/package/stylelint-config-recommended)**: turns on just [all the rules that avoid errors](https://github.com/stylelint/stylelint-config-recommended/blob/main/index.js).
2. **[`stylelint-config-standard`](https://www.npmjs.com/package/stylelint-config-standard)**: extends recommended one (with rules that just avoid errors) by turning on [rules that enforce (non-stylistic) conventions](https://github.com/stylelint/stylelint-config-standard/blob/main/index.js).
3. **[`stylelint-config-recess-order`](https://www.npmjs.com/package/stylelint-config-recess-order)**: sorts CSS properties the way Recess did and Bootstrap did/does (with some modifications & additions for modern properties).
4. **[`stylelint-config-prettier`](https://www.npmjs.com/package/stylelint-config-prettier)**: turns off all Stylelint rules that are unnecessary or might conflict with Prettier ([stylistic rules](https://stylelint.io/user-guide/rules/#enforce-stylistic-conventions)). Besides, it's shipped with a CLI tool to check that: [prettier/stylelint-config-prettier#cli-helper-tool](https://github.com/prettier/stylelint-config-prettier#cli-helper-tool).

Install them in the root of the project (locally, as dev dependencies, and with its version pinned):

```sh
npm install -D -E stylelint-config-recommended stylelint-config-standard stylelint-config-recess-order stylelint-config-prettier
```

Then, add them to the `"extends"` array in `.stylelintrc.json` (**in this exact order**):

```json
{
    "extends": [
        "stylelint-config-recommended",
        "stylelint-config-standard",
        "stylelint-config-recess-order",
        "stylelint-config-prettier",
    ]
}
```

Make sure to put `stylelint-config-prettier` **last**, so it will override other configs.

Rules to override:

```json
"rules": {
    "custom-media-pattern": null, // no kebab-case
    "custom-property-pattern": null, // no kebab-case
    "indentation": null,
    "keyframes-name-pattern": null, // no kebab-case
    "max-line-length": null,
    "selector-class-pattern": null, // no kebab-case
    "selector-id-pattern": null, // no kebab-case
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

## 6. JavaScript linter

[ESLint](https://eslint.org).
