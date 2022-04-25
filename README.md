# Setting up a front-end web dev project

## Table of Contents

- [Git](#git)
- [Node.js + npm](#nodejs--npm)
- [Node.js package](#nodejs-package)
- [EditorConfig, Prettier & ESLint](#editorconfig-prettier--eslint)

## Git

### 1. Installation

[https://git-scm.com/](https://git-scm.com/)

### 2. Verification

```sh
git --version

# Output -> git version xx.xx.xx
```

### 3. Initialisation

In the root of the project directory:

```sh
git init

# Output -> Initialized empty Git repository in /[...]/.git/
```

### 4. Ignoring files

1. Create a **`.gitignore`** file in the **root of the repository**.
2. Go to [gitignore.io](http://gitignore.io) to get templates specific for the project (e.g. `node, react`) and add the generated content into the `.gitignore` file. Example:

    ```gitignore
    # Ignore installed npm modules
    node_modules/
    ```

### 5. Normalizing line endings

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

## Node.js + npm

Download and install Node.js and npm (npm is included in Node.js).

ℹ️ If **Node.js and npm** are already installed using **nvm**, skip this section.
How to verify it:

```sh
command -v nvm
```

```sh
node -v
```

```sh
npm -v
```

[Downloading and installing Node.js and npm | npm Docs](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

1. Verify that Git version is v1.7.10+

    ```sh
    git --version
    # output -> git version xx.xx.xx
    ```

2. Install nvm (globally)

    [https://github.com/nvm-sh/nvm#installing-and-updating](https://github.com/nvm-sh/nvm#installing-and-updating)

3. Verify installation and version

    [https://github.com/nvm-sh/nvm#verify-installation](https://github.com/nvm-sh/nvm#verify-installation)

    ```sh
    command -v nvm
    # output -> nvm
    ```

    ```sh
    nvm -v
    # output -> xx.xx.xx
    ```

4. Install the latest LTS version of Node.js + npm

    [https://github.com/nvm-sh/nvm#usage](https://github.com/nvm-sh/nvm#usage)

    ```sh
    nvm install --lts
    ```

5. Verify installation and version
    - Which Node.js and npm versions are being used

        ```sh
        node -v
        # output -> vxx.xx.xx
        ```

        ```sh
        npm -v
        # output -> xx.xx.xx
        ```

    - Listing Node.js versions installed

        ```sh
        nvm ls
        ```

## Node.js package

Set up a new Node.js package.

[https://docs.npmjs.com/cli/v8/commands/npm-init](https://docs.npmjs.com/cli/v8/commands/npm-init)

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

- [Why You Shoold Use ESLint, Prettier & EditorConfig](https://blog.theodo.com/2019/08/why-you-should-use-eslint-prettier-and-editorconfig-together/)

- [Set up ESlint, Prettier & EditorConfig without conflicts](https://blog.theodo.com/2019/08/empower-your-dev-environment-with-eslint-prettier-and-editorconfig-with-no-conflicts/)

The pattern:

- All configuration related to the editor (end of line, indent style, indent size...) should be handled by **EditorConfig**.
- Everything related to code formatting should be handled by **Prettier**.
- The rest (code quality) should be handled by **ESLint**.

### 1. Installation

#### EditorConfig

- Install the Code Editor extension: [EditorConfig for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig).

#### Prettier and ESLint

1. Install the Code Editor extensions:

   - [Prettier for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
   - [ESLint for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

2. Install Prettier and ESLint locally using npm:

   ℹ️ Prerequisites: [Node.js + npm](#nodejs--npm).

   Prettier:

   ```sh
   npm install prettier --save-dev --save-exact
   ```

   ESLint:

   ```sh
   npm install eslint --save-dev --save-exact
   ```

3. Verify that Prettier and ESLint are added as entries to the `"devDependencies"` attribute of the `package.json` file:

   ```json
   {
       "devDependencies": {
           "eslint": "x.x.x",
           "prettier": "x.x.x"
       }
   }
   ```

### 2. Configuration

Prerequisites: [a `package.json` file](#nodejs-package).

#### 1. Set up an ESLint configuration file

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

#### 2. Turn off all ESLint rules that are unnecessary or might conflict with Prettier

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

#### 3. Integrate Prettier with ESLint

Integrate Prettier with ESLint to only have to run one command instead of two to lint and format our files.

Install the [`eslint-plugin-prettier`](https://github.com/prettier/eslint-plugin-prettier) package:

```sh
npm install eslint-plugin-prettier --save-dev --save-exact
```

We will now rewrite our .eslintrc.json file by adding the prettier plugin in the plugins array and setting the newly established prettier rule to error so that any prettier formatting error is considered as an ESLint error.

```json
{
  "extends": ["eslint:recommended", "prettier"],
  "env": {
    "es6": true,
    "node": true
  },
  "rules": {
    "prettier/prettier": "error"
  },
  "plugins": [
    "prettier"
  ]
}
```

Replace its content with this:

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
    "rules": {
        "eol-last": [
            "error",
            "never"
        ],
        "indent": [
            "error",
            "tab",
            {
                "SwitchCase": 1
            }
        ],
        "linebreak-style": ["error", "unix"],
        "quotes": ["error", "double"],
        "semi": ["error", "never"]
    }
}
```

ℹ️ **ESLint implicit ignore rules:**

- `node_modules/` is ignored.
- dot-files (except for `.eslintrc.*`), as well as dot-folders and their contents, are ignored.

### 3. Usage

- **Option 1:**  CLI

    ```sh
    npx eslint .
    ```

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

ℹ️ **Prerequisites:**  Node.js  +  npm

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
