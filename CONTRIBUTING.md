# Contributing modules to CFEngine Build

## Getting started guide

If you want to contribute to CFEngine Build, we highly recommend that you go through our getting started guide first.

https://docs.cfengine.com/docs/master/guide-getting-started-with-cfengine-build.html

In that guide you learn how to install CFEngine, use modules, write policy and even develop modules.
After implementing your module, you can read below to see what is necessary for submitting it to the official CFEngine Build Index with a Pull Request (PR).

### Requirements

In order to submit a module to CFEngine build, you need:

* Some reusable module code you want to share
  * Can be a policy file, [a promise type written in python](https://cfengine.com/blog/2020/how-to-implement-cfengine-custom-promise-types-in-python/), etc.
  * Must be reusable, other users must be able to use it without editing it. [Augments](https://docs.cfengine.com/docs/master/reference-language-concepts-augments.html) provide a way for users to override defaults in policy.
* A `cfbs.json` project file
  * Enables others to do `cfbs add <your repo url>` to test your module even before it is in the index
* A README file
  * Must contain some basic information about what your module does and how to use it
  * If your module requires some data or additional steps clearly tell your users what they need to do
  * Can use [this README](https://github.com/cfengine/modules/blob/master/promise-types/git/README.md) as an example to follow

### Naming your module

In order for other users to more easily find your module and understand what it does, we have some guidelines on how to name it:

* Module names are lowercase, with dashes (`-`) to separate words
* Some prefixes are commonly used:
  * `cve-...` - (followed by ID), modules used to address CVEs (either by making changes, or just identifying vulnerable hosts) 
  * `inventory-...` - modules which add inventory (reporting) data and don't make significant changes to the system
  * `promise-type-...` - modules which provide a promise type
  * `library-...` - modules which don't do much on their own, but are normally used as dependencies for other modules
  * `compliance-report-...` - modules which provide a compliance report in mission portal
    * In many cases it is natural to put both inventory attributes and a compliance report in 1 module
      * In this case, use the `compliance-report-...` prefix, not `inventory-...`
      * If the inventory attributes will be used across multiple reports / separate modules (or seem very useful and valuable on their own), putting them in a separate `inventory-` module and adding that as a dependency is natural.
  * `example-...` - modules which will always be just an example, never intended to become a real / supported module
  * `ssh-...` - modules relating to SSH
  * `sudo-...` - modules relating to sudo
* Even though CFEngine is a declarative language, we prefer module names which focus on what action / change (imperative) the user wants to achieve:
  * `delete-file`, not `file-is-not-present`. (aliases can be used when renaming modules or to provide a shortcut or alternate name)
  * In many cases, you don't need to have a verb in the name: `lynis`, `tmp-nosuid`, `ntp-maxpoll`, `surf-cfengine-library`, `ssh-max-auth-tries` (set/manage/edit is often implied).

## Publishing your module for others to reuse

When your module is working and ready for others to use, you can submit a pull request to change the index of CFEngine Build modules:

https://github.com/cfengine/build-index/blob/master/cfbs.json

(Submit a pull request to master branch, either by editing the file in GitHub UI, or cloning and editing locally).

Your entry inside the `index` should have the following contents, in this order:
* A unique key - the name of the module
* `"description"` (string) - Explains what the module does
* `"tags"` (list of strings) - Used to group the modules on the website
* `"repo"` (string) - URL to the repository to fetch your module from
* `"by"` (string) - URL to the GitHub user which should show up as the author
* `"version"` (string) - Version number of your module, 3 numbers separated by dots
  an optional release number may be included at the end after a `-` (dash) e.g. "1.0.1-2"
* `"commit"` (string) - Commit SHA of your module
* `"subdirectory"` (optional, string) - Subdirectory inside repo which has that module
* `"dependencies"` (optional, list of strings) - What other modules in the index are required for your module to work
* `"steps"` (list of strings) - What steps should be run when a user runs `cfbs build` after adding your module

People from the CFEngine team will review your module, and might have some feedback on things you could improve.
After being reviewed and approved, the change in the index will be merged, and your new version will be available shortly.

Note that the CFEngine team may in some cases decide to not include a module in the index and on our website.
In this case you can still share it with others, they just have to add it using the full URL.

## Publishing updates

When your module is already in the CFEngine Build index and you've been working on a new update, you can submit an update to your module.
This is done by editing [the index file](https://github.com/cfengine/build-index/blob/master/cfbs.json) and submitting a pull request.
You must update both `version` and `commit` (you can also update other parts if necessary, such as `steps`).
It should look something like this:

https://github.com/cfengine/build-index/pull/52/files

You can write a short changelog describing what was changed.

## Getting help

If you have a question, find something unclear, or need help with CFEngine Build, feel free to submit a question to our GitHub Discussions:

https://github.com/cfengine/core/discussions/categories/q-a
