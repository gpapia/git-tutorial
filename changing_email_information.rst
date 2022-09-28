###############################################
Changing author/committer(/tagger?) information
###############################################

If you messed up and need to change the email of the author, you should proceed
like explained here.

The ``filter-repo`` git tool (`<https://github.com/newren/git-filter-repo/>`_)
will be used which is a versatile tool for rewriting history. It roughly falls
into the same space of tool as ``filter-branch`` but without the
capitulation-inducing poor performance, with far more capabilities, and with a
design that scales usability-wise beyond trivial rewriting cases.
``filter-repo`` is now recommended by the git project instead of
``filter-branch``.

While most users will probably just use ``filter-repo`` as a simple command line
tool (and likely only use a few of its flags), at its core ``filter-repo``
contains a library for creating history rewriting tools. As such, users with
specialized needs can leverage it to quickly create entirely new history
rewriting tools.

Warning
=======

Using ``filter-repo`` is relatively simple, but rewriting history is part of a
larger discussion in terms of collaboration. When you rewrite history, the old
and new histories are no longer compatible; if you push this history somewhere
for others to view, it will look as though you’ve done a rebase of all branches
and tags. Make sure you are familiar with the "RECOVERING FROM UPSTREAM REBASE"
section of
`git-rebase(1) <https://htmlpreview.github.io/?https://raw.githubusercontent.com/newren/git-filter-repo/docs/html/git-rebase.html>`_
(and in particular, "The hard case") before proceeding.

Manual Installation
===================

``filter-repo`` is a single-file python script, which was done to make
installation for basic use on many systems trivial: just place the script into
your ``$PATH``.

``filter-repo`` only consists of a few files that need to be installed:

  * ``git-filter-repo``

    This is the only thing needed for basic use.

    This can be installed in the directory pointed to by ``git --exec-path``, or
    placed anywhere in ``$PATH``.

    If your python3 executable is named ``python`` instead of ``python3`` (this
    particularly appears to affect a number of Windows users), then you'll also
    need to modify the first line of ``git-filter-repo`` to replace ``python3``
    with ``python``.

  * ``git_filter_repo.py``

    This is needed if you want to make use of one of the scripts in
    ``contrib/filter-repo-demos/``, or want to write your own script making use
    of ``filter-repo`` as a python library.

    You can create this symlink to (or copy of) ``git-filter-repo`` named
    ``git_filter_repo.py`` and place it in your python site packages;
    ``python -c "import site; print(site.getsitepackages())"`` may help you find
    the appropriate location for your system. Alternatively, you can place this
    file anywhere within ``$PYTHONPATH``.

  * ``git-filter-repo.1``

    This is needed if you want ``git filter-repo --help`` to succeed in
    displaying the manpage, when ``help.format`` is ``man`` (the default on
    Linux and Mac).

    This can be installed in the directory pointed to by
    ``$(git --man-path)/man1/``, or placed anywhere in ``$MANDIR/man1/`` where
    ``$MANDIR``  is some entry from ``$MANPATH``.

    Note that ``git filter-repo -h`` will show a more limited built-in set of
    instructions regardless of whether the manpage is installed.

  * ``git-filter-repo.html``

    This is needed if you want ``git filter-repo --help`` to succeed in
    displaying the html version of the help, when ``help.format`` is set to
    ``html`` (the default on Windows).

    This can be installed in the directory pointed to by ``git --html-path``.

    Note that ``git filter-repo -h`` will show a more limited built-in set of
    instructions regardless of whether the html version of help is installed.

So, installation might look something like the following:

.. code-block:: console

    $ git clone https://github.com/newren/git-filter-repo.git
    $ cd git-filter-repo
    $ make snag_docs  # Copy the generated documentation files from ``docs`` branch
    $ sudo cp -a git-filter-repo $(git --exec-path)
    $ sudo cp -a git-filter-repo.1 $(git --man-path)/man1 && mandb
    $ sudo cp -a git-filter-repo.html $(git --html-path)  # Only html version of help
    $ sudo ln -s $(git --exec-path)/git-filter-repo \
          $(python -c "import site; print(site.getsitepackages()[0])")/git_filter_repo.py

The first path from ``(site.getsitepackages()`` was used, but you should use
the most appropriate one for your case.

You should also change the owner, group and permission of the copied files.

Changing author/committer(/tagger?) information
===============================================

Changing the email information from ``root@localhost`` to ``john@example.com``
is done by running the following command:

.. code-block:: console

    $ git filter-repo --email-callback \
          'return email if email != b"root@localhost" else b"john@example.com"'

``filter-repo`` developers don’t want users accidentally pushing back to the
original repo. It also reminds users that since history has been rewritten,
this repo is no longer compatible with the original. Finally, another minor
benefit is this allows users to push with the ``--mirror`` option to their new
home without accidentally sending remote tracking branches.

For example, the easiest way with GitHub is to first remove the repository
inside GitHub, second create a new one, third add the remote directory and
finally push back the corrected git directory.
