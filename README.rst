==================
 git-pull-request
==================

.. image:: https://travis-ci.org/jd/git-pull-request.png?branch=master
    :target: https://travis-ci.org/jd/git-pull-request
    :alt: Build Status

.. image:: https://badge.fury.io/py/git-pull-request.svg
    :target: https://badge.fury.io/py/git-pull-request

git-pull-request is a command line tool to send GitHub or Pagure_ pull-request from your
terminal.

Installation
============

Use the standard Python installation method::

  pip3 install git-pull-request

Although it might not be up to date with the `latest code on GitHub <https://github.com/jd/git-pull-request>`_
`Fedora <https://getfedora.org>`_ 28+ users can also download straight via package manager
`DNF <https://fedoraproject.org/wiki/DNF>`_::

  sudo dnf install git-pull-request

Usage
=====
Once you've made your commits into a branch, just type::

  git pull-request

This will:

1. Fork the upstream repository into your account (if needed)
2. Add your forked repository as a remote named "github" (if needed)
3. Force push your current branch to your remote
4. Create a pull-request for your current branch to the remote matching branch,
   or master by default.

If you add more commits to your branch later, or need to rebase your branch to
edit commits, you'll just need to run `git pull-request` to update your
pull-request. git-pull-request automatically detects that a pull-request has
been opened for your current working branch.

Workflow advice
===============

Caching Credentials
-------------------

GitHub has a good documentation about using `git credential
<https://help.github.com/en/articles/caching-your-github-password-in-git>`_.

The full documentation is available on the `git-scm.com website
<https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage>`_.

Pagure Token
------------

Pagure uses API key, you need to create one by visiting `your settings
<https://pagure.io/settings#nav-api-tab>`_
and requesting the `Fork a project` and `Modify an existing project` ACL.

Creating Branches
-----------------

When sending pull-requests, it's preferable to do so from your own branch. You
can create your own branch from `master` by doing::

  $ git checkout -b myownbranch --track origin/master

This will checkout a new branch called `myownbranch` that is a copy of master.
Using the `--track` option makes sure that the upstream source branch is
written in your `.git/config` file. This will allow git-pull-request to know to
which branch send the pull-request.

Since this is long to type, you can use an alias in git to make it faster::

  $ git config --global alias.nb '!git checkout --track $(git config branch.$(git rev-parse --abbrev-ref HEAD).remote)/$(git rev-parse --abbrev-ref HEAD) -b'

This will create a `git nb` alias that will create a new branch tracking the
current branch and checking it out. You can then use it like that::

  $ git nb foobar
  Branch foobar set up to track remote branch master from origin.
  Switched to a new branch 'foobar'

Difference with hub
===================
The wrapper `hub`_ provides `hub fork` and `hub pull-request` as
command line tools to fork and create pull-requests.

Unfortunately, it's hard to combine these tools in an automated implementation for a
complete workflow.

For example:
If you need to update your pull-request, there's no way to identify existing pull requests, so
calling `hub pull-request` would just open a new pull-request.

git-pull-request wraps all of these operations into one convenient tool.

.. _hub: https://hub.github.com/
.. _Pagure: https://pagure.io/pagure
