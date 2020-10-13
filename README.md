# [Flutter][]'s website ![Flutter logo][]

[![Build Status][]][Repo on Travis]

## Issues, bugs, and requests

We welcome contributions and feedback on our website!
Please file a request in our
[issue tracker](https://github.com/flutter/website/issues/new)
and we'll take a look.

> NOTE: For simple changes (such as to CSS and text), you probably don't need to
> build this site. Often, you can make changes using the GitHub UI.
> If you want/need to build, read on.

## Before you build this site

### 1. Get the prerequisites

Install the following tools, if you don't have them already.

- **bash**, the Bourne shell. These instructions assume you're using `bash` -- setup might not work if you use another shell.
- **[nvm][]**, the Node Version Manager.

  > **Note:** To make `nvm` immediately available in your current shell, run `source <PATH_TO_INSTALLATION>`. For example:
  > ```console
  > $ source ~/.nvm/nvm.sh
  > ```

- **[rvm][]**, the Ruby Version Manager.

  > NOTE: To make `rvm` immediately available in your current shell, run `source <PATH_TO_INSTALLATION>`. For example:
  > ```console
  > $ source ~/.rvm/bin/rvm
  > ```

- **[Flutter][Flutter install]**
- **[Dart SDK][Dart install]**
- **[GNU diffutils][]** version 3.6 or later.
  > **Note:** `diff` v3.6+ is required to ensure that in-page code diffs are
  > consistently refreshed across macOS and Linux. [Issue #3076][] was due to
  > the default macOS `diff` being at v2.x. To upgrade `diffutils`, run:<br>
  > ```console
  > $ brew install diffutils
  > ```
  > [issue #3076]: https://github.com/flutter/website/issues/3076

> IMPORTANT: Follow the installation instructions for each of the tools
> carefully. In particular, configure your shell/environment so
> that the tools are available in every terminal/command window you create.

### 2. Clone this repo _and_ its submodules

> NOTE: This repo has git _submodules_, which affects how you clone it.

To **clone [flutter/website]()** (this repo), follow the instructions given in the
GitHub help on [Cloning a repository][], then _choose one_ of the following
submodule-cloning techniques:

- Clone this repo and its submodule _at the same_, use the
  `--recurse-submodules` option:<br>
  
  ```console
  $ git clone --recurse-submodules https://github.com/flutter/website.git
  ```

  OR
- If you've already cloned this repo without its submodule, then run
  this command from the repo root:<br>

  ```console
  $ git submodule update --init --remote
  ```

> NOTE: At any time during development you can use the `git submodule` command to
> refresh submodules:<br>
> ```console
> $ git pull; git submodule update --init --remote
> ```

### 3. Run installation scripts

> NOTE: It is safe to (re-)run all of the commands and scripts given below even
> if you already have the required packages installed.

**Open a bash terminal/command window** and execute the following commands:

1. After you cloned this repo, change to
   the _root of this repo_:

   ```console
   $ cd <PATH_TO_REPO>
   ```
1.  Run the `env-set.sh` script to initialize environment variables, and to install/use required Node & Ruby version:

    ```console
    $ source ./tool/env-set.sh
    ```
1.  Run `before-install.sh` to install the  core set of required tools:

    ```console
    $ ./tool/before-install.sh
    ```
1.  Run `install.sh` to install everything else needed to build this site:

    ```console
    $ ./tool/install.sh
    ```

> IMPORTANT:
> - Any time you create a **new terminal/command window** to work on
>   this repo, **repeat steps 1 and 2** above.
> - If you upgrade Dart, then rerun all of the steps above.

## Developing and serving changes

 1. After you clone this repo, create a branch. For example:
 
    ```console
    $ git checkout -b <BRANCH_NAME>
    ```
 
 1. Make your changes, then commit them to the branch.
 1. Test your changes by serving the site locally.
    To serve locally, run _either one_ of these commands:
    - ```console
      $ ./tool/serve.sh # or npm run start)
      ```

    OR
    - ```console
      $ bundle exec jekyll serve --incremental --watch --livereload --port 4002
      ```

      > NOTE: Unless you're editing files under `site-shared`, you can safely
      > ignore `ERROR: directory is already being watched` messages.
      > For details, see [#1363](https://github.com/flutter/website/issues/1363).

      > NOTE: The first time you run either one of these commands,
      > Jekyll takes anywhere between 10-20 seconds to generate static
      > content inside the `_sites` directory. If you try to verify the
      > site locally, but aren't able to see the content right away,
      > wait 20 seconds before stopping the
      > server or concluding that something is wrong.
 1. Before submitting, validate site links:<br>
    `./tool/shared/check-links.sh`

> TIP: Sometimes Jekyll gets confused and seems to be out-of-sync. (This might
> happen, for example, when you pull from master and lots of files have moved.)
> To fix Jekyll, stop the `serve.sh` script, remove the generated site files:
> hand, and then restart the `serve.sh` script:

> ```terminal
> $ npm run clean
> ```
> OR
> ```terminal
> $ jekyll clean
> ```
> OR
> ```terminal
> $ rm -Rf ./_site/* ./.jekyll*
> ```

> Next, restart the `serve.sh` script:

> ```terminal
> $ npm run start
> ```
> OR
> ```terminal
> $ ./tool/serve.sh
> ``

## Creating and/or editing DartPad example code

Most of the code used to create DartPad examples is hosted on GitHub. However, this repo also contains
some `.dart` files responsible for DartPad example code.

### DartPad example code in GitHub gists

A typical DartPad example takes the form of an `iframe`, for example, within a codelab's markdown file:

```markdown
<iframe
  src="{{site.custom.dartpad.embed-flutter-prefix}}?id=d7b09149ffee2f0535bb0c04d96987f5"
  style="border: 1px solid lightgrey; margin-top: 10px; margin-bottom: 25px"
  frameborder="no" height="500" width="100%"
></iframe>
```

This `iframe` depends on the following GitHub gist url:

```none
https://gist.github.com/d7b09149ffee2f0535bb0c04d96987f5
```

For detailed instructions on how to use this approach to DartPad examples, see the [DartPad embedding guide].

### DartPad example code in this repo - `src/_packages/dartpad_picker`

Some DartPad example code remains in `.dart` files in this repo, and must be compiled via `src/_packages/dartpad_picker/compile.sh`. For an example, consult `src/_packages/dartpad_picker/web/dartpad_picker_main.dart`.

In order to create or change example code using `dartpad_picker`, you must regenerate the JavaScript:

```sh
  $ cd src/_packages/dartpad_picker
  $ ./compile.sh
```

## Deploy to a staging site

You can deploy your local edits to a personal staging site as follows.

 1. Serve your changes locally, as previously instructed.
    Keep the `serve.sh` script running in its own `bash` shell.

 1. In the [Firebase Console](https://console.firebase.google.com),
    create your own Firebase project (e.g. `my-foo`). You only need
    to do this step once.

 1. In a separate `bash` shell, change to the repo directory and
    initialize Firebase:
 
    ```console
    $ npx firebase init
    ```

 1. Tell Firebase about your project with the
    [`firebase use` command](https://firebase.googleblog.com/2016/07/deploy-to-multiple-environments-with.html)
    You only need to do this step once:

      ```console
      $ npx firebase use --add
      ? Which project do you want to add? <select the project you created>
      ? What alias do you want to use for this project? (e.g. staging) my-foo
      ```

 1. Tell Firebase that you want to deploy to staging:

    ```console
    $ npx firebase use my-foo
    Now using alias staging (my-foo)
    ```

 1. Tell Firebase to execute deployment of your project:

   ```console
   $ npx firebase deploy
   ```

   Your personal version of the Flutter website is now deployed to Firebase.
   Copy the serving URL from the command output.

### Alternate deployment script

Alternatively, you can skip the previous steps and just use the deploy script:

```console
$ ./tool/shared/deploy.sh --local my-foo

=== Deploying to '<your project name>'...

i  deploying hosting
i  hosting: preparing _site directory for upload...
✔  hosting: 213 files uploaded successfully
i  starting release process (may take several minutes)...

✔  Deploy complete!
```

## Deploying to the official site

Usually, official site deploys are performed by Travis. In the event that you
need to manually deploy, use the deploy script and the `default` project:

```
$ ./tool/shared/deploy.sh --local --robots ok default
```

## Writing for flutter.dev

The [site-shared](https://github.com/dart-lang/site-shared) repo
contains infrastructure shared by most of our Dart and Flutter websites.
Some of this README is in the
[doc](https://github.com/dart-lang/site-shared/tree/master/doc)
directory in the site-shared repo.

For more information on using and writing for this repo,
refer to the following docs:

* [Infrastructure](https://github.com/dart-lang/site-shared/blob/master/doc/infrastructure.md)
* [Markdown](https://github.com/dart-lang/site-shared/blob/master/doc/markdown.md)
* [Examples](https://github.com/dart-lang/site-shared/blob/master/doc/examples.md)
* [Code excerpts](https://github.com/dart-lang/site-shared/blob/master/doc/code-excerpts.md)

Also check out the site-shared
[wiki](https://github.com/dart-lang/site-shared/wiki):

* [Images](https://github.com/dart-lang/site-shared/wiki/Images)
* [Mobile friendly pages: tips & tricks](https://github.com/dart-lang/site-shared/wiki/Mobile-friendly-pages:-tips-&-tricks)
* [Writing for Dart and Flutter websites](https://github.com/dart-lang/site-shared/wiki/Writing-for-Dart-and-Flutter-websites)

[Flutter]: https://flutter.dev
[Build Status]: https://travis-ci.org/flutter/website.svg?branch=master
[Cloning a repository]: https://help.github.com/articles/cloning-a-repository
[Dart install]: https://dart.dev/get-dart
[Flutter install]: https://flutter.dev/docs/get-started/install
[Flutter logo]: https://github.com/dart-lang/site-shared/blob/master/src/_assets/image/flutter/icon/64.png?raw=1
[Firebase]: https://firebase.google.com/
[first-timers SVG]: https://img.shields.io/badge/first--timers--only-friendly-blue.svg?style=flat-square
[first-timers]: https://www.firsttimersonly.com/
[GNU diffutils]: https://www.gnu.org/software/diffutils
[DartPad embedding guide]: https://github.com/dart-lang/dart-pad/wiki/Embedding-Guide
[Jekyll]: https://jekyllrb.com/
[nvm]: https://github.com/nvm-sh/nvm/blob/master/README.md#installing-and-updating
[Repo on Travis]: https://travis-ci.org/flutter/website
[rvm]: https://rvm.io/rvm/install#installation
[this repo]: https://github.com/flutter/website
