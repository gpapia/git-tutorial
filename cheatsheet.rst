##########
Cheatsheet
##########

This is my personal cheatsheet mostly coming from "The entire Pro Git" book,
written by Scott Chacon and Ben Straub, published by Apress, and, available at
`<https://git-scm.com/book/en/v2>`_. A print version (PDF format) of the book
is also available locally (see ``progit.pdf``).

Getting Help
============

If you ever need help while using Git, there are three equivalent ways to get
the comprehensive manual page (manpage) help for any of the Git commands:

.. code-block:: console

    $ git help <verb>
    $ git <verb> --help
    $ man git-<verb>

For example, you can get the manpage help for the ``git config`` command by
running one of these:

.. code-block:: console

    $ git help config
    $ git config --help
    $ man git-config

In addition, if you don't need the full-blown manpage help, but just need a
quick refresher on the available options for a Git command, you can ask for the
more concise "help" output with the ``-h`` option:

.. code-block:: console

    $ git <verb> -h

Git Init: Create an empty Git repository or reinitialize an existing one
========================================================================

``git init`` command creates an empty Git repository - basically a ``.git``
directory with subdirectories for objects, ``refs/heads``, ``refs/tags``, and
``template`` files. An initial ``HEAD`` file that references the ``HEAD`` of the
master branch is also created.

If the ``$GIT_DIR`` environment variable is set then it specifies a path to use
instead of ``./.git`` for the base of the repository.

Running ``git init`` in an existing repository is safe. It will not overwrite
things that are already there.

If you have a project directory that is currently not under version control and
you want to start controlling it with Git, you first need to go to that
project's directory and type:

.. code-block:: console

    $ git init

At this point, nothing in your project is tracked yet.

Git Clone: Clone a repository into a new directory
==================================================

``git clone`` clones a repository into a newly created directory,
creates remote-tracking branches for each branch in the cloned repository
(visible using ``git branch --remotes``), and creates and checks out an initial
branch that is forked from the cloned repository's currently active branch.

After the clone, a plain git fetch without arguments will update all the
remote-tracking branches, and a git pull without arguments will in addition
merge the remote master branch into the current master branch, if any.

This default configuration is achieved by creating references to the remote
branch heads under ``refs/remotes/origin`` and by initializing
``remote.origin.url`` and ``remote.origin.fetch`` configuration variables.

You clone a repository with ``git clone <repository> <directory>``. For example,
if you want to clone this Git tutorial, you can do so like this:

.. code-block:: console

    $ # Clone into `mygit-tutorial` directory (should not exist or be empty)
    $ git clone git@github.com:gpapia/git-tutorial.git mygit-tutorial
    $ # Clone into `git-tutorial` directory
    $ git clone git@github.com:gpapia/git-tutorial.git

.. code-block:: man

    <repository>
        The (possibly remote) repository to clone from.

    <directory>
        The name of a new directory to clone into. The "humanish" part of the
        source repository is used if no directory is explicitly given (``repo``
        for ``/path/to/repo.git`` and ``foo`` for ``host.xz:foo/.git``). Cloning
        into an existing directory is only allowed if the directory is empty.

Git Status: Show the working tree status
========================================

``git status`` displays paths that have differences between the index file and
the current ``HEAD`` commit, paths that have differences between the working
tree and the index file, and paths in the working tree that are not tracked by
Git (and are not ignored by ``gitignore(5)``). The first are what you would
commit by running ``git commit``; the second and third are what you could
commit by running ``git add`` before running ``git commit``.

You check the status of your files by running ``git status <pathspec>``.

.. code-block:: console

    $ git status
    On branch master
    Your branch is ahead of 'origin/master' by 1 commit.
      (use "git push" to publish your local commits)

    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            modified:   cheatsheet.rst
            modified:   fundamentals.rst
            new file:   pictures/lifecycle.png

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   cheatsheet.rst

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
            pictures/branch-and-history.png

    $ git status *rst
    On branch master
    Your branch is ahead of 'origin/master' by 1 commit.
      (use "git push" to publish your local commits)

    Changes to be committed:
      (use "git restore --staged <file>..." to unstage)
            modified:   cheatsheet.rst
            modified:   fundamentals.rst

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git restore <file>..." to discard changes in working directory)
            modified:   cheatsheet.rst

Short Status
------------

While the ``git status`` output is pretty comprehensive, it’s also quite wordy.
Git also has a short status flag so you can see your changes in a more compact
way. If you run ``git status -s`` or ``git status --short`` you get a far more
simplified output from the command:

.. code-block:: console

   $ git status -s
   MM cheatsheet.rst
   M  fundamentals.rst
   A  pictures/lifecycle.png
   ?? pictures/branch-and-history.png

There are two columns to the output that indicate (if no merge conflict):

  1. The left-hand columns indicates the status of the staging area.
  2. The right-hand column indicates the status of the working tree.

New files that aren't tracked have a ``??`` next to them. The other
meanings are the following:

  * ``M`` = modified
  * ``A`` = added
  * ``D`` = deleted
  * ``R`` = renamed
  * ``C`` = copied
  * ``U`` = updated but unmerged

.. code-block:: man

    -s, --short
        Give the output in the short-format.

    Short Format
        In the short-format, the status of each path is shown as one of these
        forms

            XY PATH
            XY ORIG_PATH -> PATH

        where ``ORIG_PATH`` is where the renamed/copied contents came from.
        ``ORIG_PATH`` is only shown when the entry is renamed or copied. The
        ``XY`` is a two-letter status code.

        [...]

        For paths with merge conflicts, ``X`` and ``Y`` show the modification
        states of each side of the merge. For paths that do not have merge
        conflicts, ``X`` shows the status of the index, and ``Y`` shows the
        status of the work tree. For untracked paths, ``XY`` are ``??``.
        Other status codes can be interpreted as follows:

        *   ' ' = unmodified

        *   M = modified

        *   A = added

        *   D = deleted

        *   R = renamed

        *   C = copied

        *   U = updated but unmerged

Verbose Status
--------------

In some cases, it may be usefull to also show the textual differences between
files while checking the status. Git has a verbose status flag so you can
see the textual changes. If you run ``git status -v`` or
``git status --verbose`` you get the textual changes of the staged files
ready to be commited, and if you use the ``-v`` option twice, you get
also the changes of the working files ready to be staged.

.. code-block:: console

    $ git status -v
    [Too long to copy/paste, just test it.]

    $ git status -vv
    [Even longer, just test it.]

.. code-block:: man

    -v, --verbose
        In addition to the names of files that have been changed, also show the
        textual changes that are staged to be committed (i.e., like the output
        of ``git diff --cached``). If ``-v`` is specified twice, then also show
        the changes in the working tree that have not yet been staged (i.e.,
        like the output of git diff).

Git Add: Add file contents to the index
=======================================

``git add`` command updates the index using the current content found in the
working tree, to prepare the content staged for the next commit. It typically
adds the current content of existing paths as a whole, but with some options it
can also be used to add content with only part of the changes made to the
working tree files applied, or remove paths that do not exist in the working
tree anymore.

The "index" holds a snapshot of the content of the working tree, and it is this
snapshot that is taken as the contents of the next commit. Thus after making any
changes to the working tree, and before running the commit command, you **must**
use the ``add`` command to add any new or modified files to the index.

This command can be performed multiple times before a commit. It only adds the
content of the specified file(s) at the time the add command is run; if you want
subsequent changes included in the next commit, then you must run ``git add``
again to add the new content to the index.

The ``git add`` command will not add ignored files by default. If any ignored
files were explicitly specified on the command line, git add will fail with a
list of ignored files. Ignored files reached by directory recursion or filename
globbing performed by Git (quote your globs before the shell) will be silently
ignored. The ``git add`` command can be used to add ignored files with the
``-f`` (force) option.

You add files with ``git add <pathspec>``. For example, if you want to add
the ``.gitignore`` file, all files ending with ``.rst``, and all files
inside the ``pictures`` directory, you can do so like this:

.. code-block:: console

    $ git add .gitignore
    $ git add *.rst
    $ git add pictures/

.. code-bloc:: man

    <pathspec>...
        Files to add content from. Fileglobs (e.g. ``*.c``) can be given to add
        all matching files. Also a leading directory name (e.g. ``dir`` to add
        ``dir/file1`` and ``dir/file2``) can be given to update the index to
        match the current state of the directory as a whole (e.g. specifying
        ``dir`` will record not just a file ``dir/file1`` modified in the
        working tree, a file ``dir/file2`` added to the working tree, but also a
        file ``dir/file3`` removed from the working tree).

Git Config: Get and set repository or global options
====================================================

``git config`` lets you get and set configuration variables that control all
aspects of how Git looks and operates. These variable can be stored in three
different places:

  1. ``[path]/etc/gitconfig`` file: Contains values applied to every user on the
     system and all their repositories. If you pass the option ``--system`` to
     ``git config``, it reads and writes from this file specifically. Because
     this is a system configuration file, you would need administrative or
     superuser privilege to make changes to it.
  2. ``~/.gitconfig`` or ``~/.config/git/config`` file: Values specific
     personally to you, the user. You can make Git read and write to this file
     specifically by passing the ``--global`` option, and this affects all of
     the repositories you work with on your system.
  3. ``config`` file in the Git directory (that is, ``.git/config``) of whatever
     repository you’re currently using: Specific to that single repository. You
     can force Git to read from and write to this file with the ``--local``
     option, but that is in fact the default. Unsurprisingly, you need to be
     located somewhere in a Git repository for this option to work properly.

Each level overrides values in the previous level, so values in ``.git/config``
trump those in ``[path]/etc/gitconfig``.

Checking your settings and where they are coming from
-----------------------------------------------------

If you want to check your configuration settings, you can use the following
command to list all the settings Git can find at that point:

.. code-block:: console

    $ git config --list --show-origin

You can also check what Git thinks a specific key’s value is by typing
``git config <key>``, e.g.,:

.. code-block:: console

    $ git config --show-origin user.name
    file:/home/papiag/.gitconfig    Gianluigi Papia

.. code-block:: man

    -l, --list
        List all variables set in config file, along with their values.

    --show-origin
        Augment the output of all queried config options with the origin type
        (file, standard input, blob, command line) and the actual origin
        (config file path, ref, or blob id if applicable).

Identity Configuration
----------------------

.. code-block:: console

    $ git config --global user.name "Gianluigi Papia"
    $ git config --global user.email johndoe@example.com

Editor Configuration
--------------------

.. code-block:: console

    $ git config --global core.editor emacs

Default Branch Name
-------------------

By default Git will create a branch called ``master`` when you create a new
repository with ``git init``. From Git version 2.28 onwards, you can set a
different name for the initial branch.

GitHub changed the default branch name from ``master`` to ``main`` in mid-2020,
and other Git hosts followed suit. To set ``main`` as the default branch name
do:

.. code-block:: console

    $ git config --global init.defaultBranch main
