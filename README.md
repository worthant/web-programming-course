<p align="center">
  <a href="https://ibb.co/FszDc14">
    <picture>
      <img src="https://i.ibb.co/FszDc14/global-network.png" height="80">
    </picture>
    <h1 align="center">Web programming course</h1>
  </a>
</p>

<p align="center">
  <a aria-label="Last commit" href="https://github.com/worthant/web-programming-course/commits/main">
    <img alt="" src="https://img.shields.io/github/last-commit/worthant/web-programming-course?style=for-the-badge&logo=git">
  </a>
  <a aria-label="Repo size" href="https://github.com/worthant/web-programming-course">
    <img alt="" src="https://img.shields.io/github/repo-size/worthant/web-programming-course?style=for-the-badge&logo=github">
  </a>
  <a aria-label="License" href="./LICENSE">
    <img alt="" src="https://img.shields.io/github/license/worthant/web-programming-course?style=for-the-badge">
  </a>
  <a aria-label="Translation" href="./README_RU.md">
    <img alt="" src="https://img.shields.io/badge/translation-RU-red?style=for-the-badge">
  </a>
</p>

> `Welcome` to the Web-Programming Course repository! This repo contains a `series of lab work projects` that span across various concepts in `Web Programming`.  

> `Each` lab work resides in its `own directory` with dedicated `README files` containing the respective instructions and requirements.

👇 An overview of the labs included in this course:

| Laboratory Work              | Grade | Difficulty | Time Spent | Key Concepts | Materials |
| :----------------------------: | :-----: | :----------: | :----------: | :------------: | :---------: |
| [Laboratory Work №1](https://github.com/worthant/simple-one-page-website) |    92%   | `3/10` |  `6/10`   |  `HTML, CSS (+ flexbox, grid), Js, PHP`  | [theory](https://github.com/worthant/simple-one-page-website/blob/main/theory.md) ; [preview](https://se.ifmo.ru/~s368090/lab1/index.html)|
| [Laboratory Work №2](./lab2) |       |            |            |              |           |
| [Laboratory Work №3](./lab3) |       |            |            |              |           |
| [Laboratory Work №4](./lab4) |       |            |            |              |           |

_I'll be updating the Grade, Difficulty, Time Spent, Key Concepts, and Materials for each lab work as I progress through the course._

## 📝 Commit Message Convention

> Consistency is key, especially when it comes to git commits.  
> For ease of readability and understanding, this repository follows a somewhat beautiful :sparkles: and outstanding :rocket: notation for commits!

Commit message should be in the following format:

```bash
type[!] message
```

> That is `type` optionally followed by exclamation mark, strictly followed by _**one**_ space, followed by a message.

> Total message length may exceed `80 characters` (type is counted as one character), but it is recommended to make messages expressive and small.

> Exclamation mark denotes `breaking change`. It is correlates with [`MAJOR`](https://semver.org/#summary) in Semantic Versioning.

### Type should be one of the following

1. 🚀 Starting new great things  

> things, like `new repository`, `subproject`, adding `submodule` to huge things, e.t.c

```bash
:rocket: Start labX
```

```bash
:rocket: Add labX submodule
```

2. :sparkles: Indicates new feature  

> analogue to `feat` tag in conventional commits

```bash
:sparkles: Add new feature
```

```bash
:sparkles: Introduce new functionality
```

3. :bug::wrench: Bug Fix

> improve or fix something

```bash
:bug: Fix MUI Drawer bug
```

```bash
:wrench: Resolve table of contents issue
```

4. 📚 Adding or Updating Documentation:

```bash
:books: Add table of contents to README
```

```bash
:books: Add documentation for backend logic
```

5. :test_tube: DevOps things

> add `CI/CD`, anything related to docker/deploy/e.t.c.

```bash
:test_tube: Add CI
```

```bash
:test_tube: Update deploy.sh
```

6. :recycle::yarn::hammer: Refactoring and improving repo

```bash
:recycle: Refactor code
```

```bash
:yarn: Improve classes structure
```

```bash
:hammer: Move dir1, delete kek1337 directory
```

7. :heavy_check_mark::white_check_mark: Tests:

```bash
:white_check_mark: Add unit tests
```

```bash
:heavy_check_mark: Add e2e tests
```

8. 🙈 Updating Gitignore:

```bash
:see_no_evil: Update .gitignore for LabX
```
  
9. 🗑️ Removing Code or Files:

```bash
:wastebasket: Remove unused code
```

> By following these conventions, I maintain a `clean` and `structured` git history which facilitates easier `readability` and project `maintenance`.

<p align="center">Happy coding! 🎉</p>
