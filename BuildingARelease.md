# Overview #

Releasing a new download requires a few steps to be performed. For each release, the changes are listed in the release notes, pydocs are updated to reflect changes in the code, and the library source code, tests, etc. are packaged up in tar.gz files and .zip archives. The archives are then added to the Downloads section of this project. See detailed step by step instructions below.

# Steps #

  1. Create a new release tracking bug to track this release.
    1. Browse to the [New Issue page](http://code.google.com/p/httplib2/issues/entry).
    1. Select the Release template.
    1. In the summary, enter "Release X.Y.Z", replacing X.Y.Z appropriately.
    1. In the description, enter in the version you're releasing, the freeze date at which you'll cut the release, and the date which you'll do the release and publish the actual release packages.
    1. Make sure that you're marked as the owner, since you're doing the release.
    1. If the release is blocked on any other bugs, make sure they're listed in the Blocked On field.
    1. Submit the issue.
  1. Once the issue is submitted, and you're ready to cut the release, add a comment to the issue giving the exact commit hash at which you're cutting the release.  This ensures people know exactly what is and is not in the release.
  1. Write release notes for the new release.
  1. Update the version number in `setup.py`.
  1. Update the version number in version in `__init__.py` for both Python 2.x and 3.x.
  1. Regenerate the pydoc documentation pages.
  1. Add new packages and files to the indexes so that they will be included in the archives and installed.
    1. Edit the list of packages in `setup.py`. This list determines which packages will be compiled and installed on the user's system.
  1. Run the unit tests.
  1. Tag the version in mercurial: `$ hg tag 0.x.yy`
  1. Commit everything that's changed in your local copy.  Push your changes.
  1. Generate the archives, upload the packages, and update PyPi.
    1. `$ make release`
  1. Go to the downloads page and mark the new uploads as Featured, and remove that designation from older downloads.
  1. Update the release bug you created at the beginning of this, noting the tag of the commit containing all of the updated release files.
  1. Update the release bug, marking it as fixed.
  1. Send an email to the list announcing the release.