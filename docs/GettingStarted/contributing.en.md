# Guide

Thanks for participating in the edit!

The following will guide you on how to participate in the writing of this document; every small contribution is acknowledged and encouraged.

In order to participate in the writing, you will need to sign up for a GitHub account. If you don't have one, please register at [here](https://github.com/signup).

## Adding links

If you want to add a link, you need to submit a PR. Readme links are added by the repository maintainers themselves and no PRs are accepted.
For contributors, the following entries are required to add a link.

**Content requirements**

* Current

* Content on a topic that is consistent with the content of the wiki and that is not obviously commercially promoted.

**Access**

* Material must be backable (e.g. web to PDF, etc.) or provide a transparent download of the original material.

* Must be publicly viewable.

## Formatting requirements

Accuracy is understood first, coverage second.

### Commit Specification

- The commit message should briefly describe the changes.

- The commit summary should be no more than 45 characters long. If necessary, please provide details in the body of the message.

For commit summaries, the following format is recommended.

```
<modification type>(<filename>): <what was changed>
```

There are several types of modification.

    feat: add content

    fix: fixes content errors

    refactor: refactor a page (larger changes)

    revert: roll back changes

### Pull Request specification

The title states the purpose of the PR (what was done). The content should briefly describe what was changed.

If you fixed an issue, add sovle #xxxx to the content, where xxxx is the issue number.

The recommended format for the title is as follows.
```
<modification type> (<filename>): <modified content> (<issue number>)
```

There are several types of modification.

    feat: add content

    fix: fixes a content error

    refactor: refactor a page (larger changes)

    revert: roll back changes



### Read friendly

Consider the reader's feelings. Offbeat aliases should not appear. To use standard terminology, standardise. If new terms need to be added, add a description, or if they are more complex, add them to the glossary.

- Reject short set questions

Eliminate some unnecessary short question and answer forms

For example
```
questions
Short answers
```
should be changed to ``If xxx, read xxx``

- Formatting

Do not format the content, it is difficult to review content changes when merging.

If needed, make a special request for formatting optimisation.

- Sorting, with important sections first

The higher the frequency, the more important information should come first.

- Responsible for

If the content is only linked, please make a brief description or extract the main content. Please do not lose such short content.

```

### Added content
[content link]()

### Next content

```

## Submitted content

Make sure your content complies with [Github terms](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies) first, and until your changes are merged into the main repository Any changes you make will not appear on the page and can be safely edited.

### Notification of progress

Before you start writing a piece of content, check [Issues](https://github.com/sudoskys/StableDiffusionBook/issues) to make sure there are no conflicting jobs (if there are, you can communicate them under Issues), and then create a new issue to record the content to be written.

!!! tip
    Please turn off the page translation plugin when editing.

### Editing individual pages

You can edit individual pages directly by clicking on the edit button at the top right of the text.

Write the changes you want to make in the edit box, then scroll to the bottom of the page and fill in the `commit` information according to the `commit` specification in this article, then click the green button to commit the changes. Once you click the button, GitHub will automatically clone a branch of the repository for you and then jump to the repository page.

Click the green `Create pull request` button at the top of the page to jump to the Create `Pull Request` page. Check your changes again, write your commit message as described in the `Pull Request` commit specification section of this article, and then click the `Create pull request` button to create the merge request.

Have a hot cup of tea! Just wait for the audit and the collaborators will merge it into the main repository when it is complete.

### Edit multiple pages

Go to the code repository page and click `Fork` to clone this repository.

On your repository page, click the `. ` button to go to the GitHub Dev editor

Use the `Source Control` tab (icon like `V`) on the left when you're done making changes, and commit according to the commit specification in this article.

Then click on the Branch tab (like the `n` icon), create a pull request, and set INTO the main repository address. Fill in the edit box below with the title and description in the format, then click the `Create` button to create the request. (If you are on an unfinished branch, check `Create as draft`).

To add changes, simply edit the original branch again and it will automatically be added to the request.

### Adding an index

Add an index to `mkdocs.yml` with the translation of the outline and the corresponding big title.


