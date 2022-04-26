# Setting up a front-end web dev project

## Table of Contents

- [Git](#git)
  - [1. Initialize a Git repository](#1-initialize-a-git-repository)
  - [2. Ignore files](#2-ignore-files)
  - [3. Normalize line endings](#3-normalize-line-endings)
- [npm package](#npm-package)
- [EditorConfig, Prettier & ESLint](#editorconfig-prettier--eslint)
  - [1. Installation](#1-installation)
  - [2. Configuration](#2-configuration)
  - [3. Usage](#3-usage)

## Git

ℹ️ **Prerequisites: [Git installed](https://github.com/germanfrelo/my-frontend-web-development-setup/blob/main/README.md#git)**

### 1. Initialize a Git repository

In the **root of the project directory**, execute:

```sh
git init

# Output: Initialized empty Git repository in /[...]/.git/
```

### 2. Ignore files

1. Create a **`.gitignore`** file in the **root of the repository**.
2. Go to [gitignore.io](http://gitignore.io) to get templates specific for the project (e.g. `node, react`) and add the generated content into the `.gitignore` file. Example:

    ```gitignore
    # Ignore installed npm modules
    node_modules/
    ```

### 3. Normalize line endings

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
    # Normalize line endings of all text files checked into the repo with LF.
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

## npm package

ℹ️ **Prerequisites: [Node.js + npm](https://github.com/germanfrelo/my-frontend-web-development-setup/blob/main/README.md#nodejs--npm)**

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

## EditorConfig, Prettier & ESLint

Websites:

- [EditorConfig](https://editorconfig.org)
- [Prettier](https://prettier.io/)
- [ESLint](https://eslint.org/)

References:

- [Reasons to use both a local and global editorconfig file](https://blog.danskingdom.com/Reasons-to-use-both-a-local-and-global-editorconfig-file/)

- [Why You Shoold Use ESLint, Prettier & EditorConfig](https://blog.theodo.com/2019/08/why-you-should-use-eslint-prettier-and-editorconfig-together/)

- [Set up ESlint, Prettier & EditorConfig without conflicts](https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/)

The pattern:

- All configuration related to the editor (end of line, indent style, indent size...) should be handled by **EditorConfig**.
- Everything related to code formatting should be handled by **Prettier**.
- The rest (code quality) should be handled by **ESLint**.

### 1. EditorConfig

#### 1. Installation

Install [the extension/plugin for your Code Editor](https://editorconfig.org/#download).

- VS Code: [EditorConfig for VS Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

#### 2. Configuration

Create a `.editorconfig` file in the root of the project.

Example: [My local .editorconfig file](https://gist.github.com/germanfrelo/a71698d5c4592220a0fa4915f32182ce)

Supported properties:

- `indent_style`
- `indent_size`
- `tab_width`
- `end_of_line` (appplied only on file save)
- `insert_final_newline` (appplied only on file save)
- `trim_trailing_whitespace` (appplied only on file save)

Characteristics:

- It's for team settings (one per repository).
- Gets committed to source control in every repository.
- Location: root directory of every repository.
- Should only include settings that affect the physical contents of the file, and that you want enforced in the repository, not just how it appears in an editor.
- Should have `root = false` defined so that presentation-only (and other) properties can be inherited from the global `.editorconfig` file.
- Should not contain any presentation-only properties, such as `tab_width` or `tab_width`.

### 2. Prettier

1. Install the Code Editor extensions:

   - [Prettier for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)

2. Install Prettier using npm:

   ℹ️ **Prerequisites: [a package.json file](#npm-package)**

   ```sh
   npm install prettier --save-dev --save-exact
   ```

3. Verify that it is added as `devDependencies` in the `package.json` file:
   
   ```json
   {
       "devDependencies": {
           "prettier": "x.x.x"
       }
   }
   ```

### 3. ESLint

1. Install the Code Editor extensions:

   - [ESLint for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

2. Install ESLint using npm:

   ℹ️ **Prerequisites: [a package.json file](#npm-package)**

   ```sh
   npm install eslint --save-dev --save-exact
   ```

3. Verify that it is added as `devDependencies` in the `package.json` file:
   
   ```json
   {
       "devDependencies": {
           "eslint": "x.x.x",
           "prettier": "x.x.x"
       }
   }
   ```

4. Set up an ESLint configuration file.

    ℹ️ **Prerequisites: [a package.json file](#npm-package)**

    ```sh
    npm init @eslint/config
    ```

    Steps:

    - How would you like to use ESLint? · problems

    - Where does your code run? · browser, node

    - What format do you want your config file to be in? · JSON

    Output:

    ```sh
    # Successfully created .eslintrc.* configuration file in ...
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

    ℹ️ **ESLint implicit ignore rules:**

    - `node_modules/` is ignored.
    - dot-files (except for `.eslintrc.*`), as well as dot-folders and their contents, are ignored.

### 4. EditorConfig and Prettier

### 5. Prettier and ESLint

#### 1. Turn off all ESLint rules that are unnecessary or might conflict with Prettier

Install the [`eslint-config-prettier`](https://github.com/prettier/eslint-config-prettier) package locally as a dev dependency:

```sh
npm install eslint-config-prettier --save-dev --save-exact
```

Verify that `"eslint-config-prettier"` is added as an entry to the `"devDependencies"` attribute of the `package.json` file:

```json
{
    "devDependencies": {
        "eslint": "x.x.x",
        "eslint-config-prettier": "x.x.x",
        "prettier": "x.x.x"
    }
}
```

Then, add `"prettier"` to the `"extends"` array in the `.eslintrc.json` file.

```json
{
    "extends": ["eslint:recommended", "prettier"],
}
```

Make sure to put it last, so it gets the chance to override any prior configuration in the extends array disabling all ESLint code formatting rules. With this configuration, Prettier and ESLint can be run separately without any issues.

Finally, remove any code formatting rules you had in the `.eslintrc.json` file. If `"rules"` is empty, go to the next step. If no, to find if the `"rules"` section of the ESLint configuration file (`.eslintrc.json`) contains any rules that are unnecessary or conflict with Prettier, run the [CLI helper tool](https://github.com/prettier/eslint-config-prettier#cli-helper-tool). (Remember, `"rules"` always "wins" over `"extends"`!)

#### 2. Integrate Prettier with ESLint

In order to lint and format the files by using only one command instead of two, integrate Prettier with ESLint by adding the [`eslint-plugin-prettier`](https://github.com/prettier/eslint-plugin-prettier) package.

Install the `eslint-plugin-prettier` package:

```sh
npm install eslint-plugin-prettier --save-dev --save-exact
```

Verify that `"eslint-plugin-prettier"` is added as an entry to the `"devDependencies"` attribute of the `package.json` file:

```json
{
    "devDependencies": {
        "eslint": "x.x.x",
        "eslint-config-prettier": "x.x.x",
        "eslint-plugin-prettier": "x.x.x",
        "prettier": "x.x.x"
    }
}
```

Then, modify the `.eslintrc.json` file:

- Add the **Prettier plugin** in the `"plugins"` array.
- Set the newly established **Prettier rule** to `"error"` so that any Prettier formatting error is considered as an ESLint error.

```json
{
    "rules": {
        "prettier/prettier": "error"
    },
    "plugins": [
        "prettier"
    ]
}
```

You can replace all the Prettier relevant configuration we made in our `.eslintrc.json` file by adding the `plugin:prettier/recommended` configuration in the extends array.

Here is what it would look like:

`.eslintrc.json`:

```json
{
    "extends": ["eslint:recommended", "plugin:prettier/recommended"]
}
```

This configuration is the same as before but shorter and less explicit.

My code quality ESLint rules:

```json
{
    "rules": {
        "quotes": ["error", "double"],
        "semi": ["error", "never"]
    }
}
```

### 3. Using all three

- **Option 1:**  CLI

    ```sh
    npx eslint .
    ```

    The lines that have formatting errors will be marked by `prettier/prettier` as errors within the ESLint error output.

    Fix:

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

## Stylelint

[Home | Stylelint](https://stylelint.io/)

ℹ️ **Prerequisites: [a package.json file](#npm-package)**

### Installation

Install the Code Editor extension: [Stylelint for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=stylelint.vscode-stylelint).

Install Stylelint with its standard configuration (<https://github.com/stylelint/stylelint-config-standard>):

```sh
npm install stylelint stylelint-config-standard --save-dev --save-exact
```

### Verification

Verify that Stylelint and its standard configuration are added as entries to the `"devDependencies"` attribute of the `package.json` file, like this:

```json
"devDependencies": {
    "stylelint": "x.x.x",
    "stylelint-config-standard": "x.x.x"
}
```

### Configuration

Create a `.stylelintrc.json` configuration file in the project's root directory.

```sh
touch .stylelintrc.json
```

Add the following content to the `.stylelintrc.json` to *extend* the standard configuration:

```json
{
    "extends": [
        "stylelint-config-standard"
    ]
}
```

### Additional configurations

**To sort CSS properties (**<https://github.com/stormwarning/stylelint-config-recess-order>**)**

1. Installation

    ```sh
    npm install --save-dev --save-exact stylelint-config-recess-order
    ```

2. Verification

    In the `package.json` file:

    ```json
    "devDependencies": {
        "stylelint-config-recess-order": "x.x.x"
    }
    ```

3. Configuration

    In the `.stylelintrc.json` file:

    Add the following content to *extend* the **Recess Property Order** configuration:

    ```json
    "extends": [
        "stylelint-config-recess-order"
    ]
    ```

    Add these rules to override the existing configurations:

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

### Usage

- **Option 1:**  CLI

    ```sh
    npx stylelint "**/*.css"
    ```

- **Option 2:**  Script

    Add this to the `package.json` file:

    ```json
    "scripts": {
        "lint:css": "npx stylelint \"**/*.css\" --formatter verbose"
    }
    ```

    Run:

    ```sh
    npm run lint:css
    ```
