################
Git Fundamentals
################

How works Git underneath? The explanations belows are mostly coming from
"The entire Pro Git" book, written by Scott Chacon and Ben Straub, published
by Apress and available at `<https://git-scm.com/book/en/v2>`_. A print version
(PDF format) of the book is also available locally (see ``progit.pdf``).

What is Git?
============

Snapshots, Not Differences
--------------------------

The major difference between Git and any other VCS (Subversion and friends
included) is the way Git thinks about its data. Conceptually, most other systems
store information as a list of file-based changes. These other systems
(CVS, Subversion, Perforce, Bazaar, and so on) think of the information they
store as a set of files and the changes made to each file over time
(this is commonly described as ``delta-based`` version control).

.. figure:: pictures/deltas.png
    :alt: Storing data as changes to a base version of each file

    Storing data as changes to a base version of each file

Git doesn't think of or store its data this way. Instead, Git thinks of its data
more like a series of snapshots of a miniature filesystem. With Git, every time
you commit, or save the state of your project, Git basically takes a picture of
what all your files look like at that moment and stores a reference to that
snapshot. To be efficient, if files have not changed, Git doesn't store the file
again, just a link to the previous identical file it has already stored. Git
thinks about its data more like a **stream of snapshots**.

.. figure:: pictures/snapshots.png
    :alt: Storing data as snapshots of the project over time

    Storing data as snapshots of the project over time

The Three States
----------------

Git has three main states that your files can reside in: ``modified``
``staged``, and ``committed``:

  1. Modified means that you have changed the file but have not committed it
     to your database yet.
  2. Staged means that you have marked a modified file in its current version to
     go into your next commit snapshot.
  3. Committed means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project:

  1. The working tree is a single checkout of one version of the project. These
     files are pulled out of the compressed database in the Git directory and
     placed on disk for you to use or modify.
  2. The staging area is a file, generally contained in your Git directory, that
     stores information about what will go into your next commit. Its technical
     name in Git parlance is the "index", but the phrase "staging area" works
     just as well.
  3. The Git directory is where Git stores the metadata and object database for
     your project. This is the most important part of Git, and it is what is
     copied when you clone a repository from another computer.

.. figure:: pictures/areas.png
    :alt: Working tree, staging area, and Git directory

    Working tree, staging area, and Git directory

The lifecyle of the status of your files
----------------------------------------

Each file in your working directory can be in one of two states: ``tracked`` or
``untracked``.

  * Tracked files are files that were in the last snapshot, as well as any newly
    staged files; they can be unmodified, modified, or staged. In short, tracked
    files are files that Git knows about.

  * Untracked files are everything else — any files in your working directory
    that were not in your last snapshot and are not in your staging area.

.. figure:: pictures/lifecycle.png
    :alt: The lifecycle of the status of your files

    The lifecycle of the status of your files
