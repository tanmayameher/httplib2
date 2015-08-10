# How to become a contributor and submit patches #

## Submitting Patches ##

  1. Join the [discussion group](https://groups.google.com/group/httplib2-dev/).
  1. Decide which code you want to submit. A submission should be a set of changes that addresses one issue in the  [issue tracker](http://code.google.com/p/httplib2/issues/list). Please don't mix more than one logical change per submittal, because it makes the history hard to follow. If you want to make a change that doesn't have a corresponding issue in the issue tracker, please create one.
  1. Also, coordinate with team members that are listed on the issue in question. This ensures that work isn't being duplicated and communicating your plan early also generally leads to better patches.
  1. Ensure that your code adheres to the PEP 8 style, with the exception of spacing, which should be two spaces.
  1. Ensure that there are unit tests for your code.
  1. Use the `upload.py` script to upload your patches to [codereview.appspot.com](http://codereview.appspot.com) for review.

### Using `upload.py` and the code review site ###

In most cases running

```
$ python upload.py
```

will Do The Right Thing without any additional input. See the [upload.py documentation](http://code.google.com/p/rietveld/wiki/UploadPyUsage) for more help. Make sure to send the review to joe.gregorio@gmail.com and CC httlib2-dev@googlegroups.com.  There is also an article [here](http://code.google.com/p/rietveld/wiki/CodeReviewHelp) on how to go through the code review process.

Once the code review has started you may be asked to make changes to your code. Once you have made those changes run `upload.py` again to update the patch. You will need to know the codereview issue number.

Assuming it is 123456 you would run:

```
$ python upload.py -i 123456
```

### Reviewing ###

If you are doing a review the easiest way to work with patches is [mercurial queues](http://mercurial.selenic.com/wiki/MqExtension). If we assume the codereview issue is again 123456, in the upper right-hand corner of the code review page is a "Download raw patch set" link, which is the URL of the patchset to import:

```
$ hg qimport http://codereview.appspot.com/download/issue123456_10000.diff
```

You can then `hg qpush` the patch into your repository and when you are done `hq qpop` it back out again. Once the patch is in good shape you can `hg qfinish` it to commit it to the repository.