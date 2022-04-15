# Contributing

[issue]: https://github.com/GitCredentialManager/git-credential-manager/issues
[fork]: https://github.com/GitCredentialManager/git-credential-manager/fork
[pr]: https://github.com/GitCredentialManager/git-credential-manager/compare
[code-of-conduct]: CODE_OF_CONDUCT.md

Hi there! We're thrilled that you'd like to contribute to this project. Your help is essential for keeping it great.

Contributions to this project are [released](https://help.github.com/articles/github-terms-of-service/#6-contributions-under-repository-license) to the public under the [project's open source license](LICENSE).

Please note that this project is released with a [Contributor Code of Conduct][code-of-conduct]. By participating in this project you agree to abide by its terms.

## Start with an issue

1. Open an [issue][issue] to discuss the change you want to see.
This helps us coordinate and reduce duplication.
2. Once we've had some discussion, you're ready to code!

## Submitting a pull request

1. [Fork][fork] and clone the repository
2. Configure and install the dependencies: `dotnet restore`
3. Make sure the tests pass on your machine: `dotnet test`
4. Create a new branch: `git switch -c my-branch-name`
5. Make your change, add tests, and make sure the tests still pass
6. For UI updates, test your changes by executing a `dotnet run` in applicable UI-related project directories:
    - `Atlassian.Bitbucket.UI.Avalonia`
    - `GitHub.UI.Avalonia`
    - `Atlassian.Bitbucket.UI.Windows`
    - `GitHub.UI.Windows`
7. Push to your fork and [submit a pull request][pr]
8. Pat your self on the back and wait for your pull request to be reviewed and merged.

Here are a few things you can do that will increase the likelihood of your pull request being accepted:

- Match existing code style.
- Write tests.
- Keep your change as focused as possible. If there are multiple changes you would like to make that are not dependent upon each other, consider submitting them as separate pull requests.
- Write a [good commit message](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html).

## Resources

- [How to Contribute to Open Source](https://opensource.guide/how-to-contribute/)
- [Using Pull Requests](https://help.github.com/articles/about-pull-requests/)
- [GitHub Help](https://help.github.com)
