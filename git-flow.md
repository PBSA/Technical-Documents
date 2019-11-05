# Git-Flow

The purpose of this document is to describe and define how changes flow into our code and through the various stages of development until it finally goes into production.

The general idea is based on [git-flow](https://datasift.github.io/gitflow/IntroducingGitFlow.html).

For our purposes, the general concept behind gitflow has been extended to allow for these additional needs:

1. We have two different types of releases, mainnet and testnet, with a master-like branch for each one.
2. We have to distinguish consensus-breaking changes \(aka hardforks\) from

   non-consensus-breaking changes.

## Goals To Achieve:

1. Maintain two independent release versions, testnet and mainnet.
2. Decouple development from releases, i. e. maintain the ability to create

   emergency bugfixes for current release without bringing incomplete new

   features into production.

3. Separate consensus-related changes from non-consensus-related changes.
4. Keep development branches compatible with mainnet.

## Basic Rules:

1. Development always happens in private feature-branches. The only exception is

   a change that must be distinguished in the destination branch \(typical

   example: hardfork date in testnet\).

2. Features are merged after they are reasonably complete, i. e. they come with

   unit tests that provide reasonable coverage and do not report any errors.

   1. "Completed" features that are not consensus-related are merged into

      "develop".

   2. "Completed" features that are consensus-related are merged into the

      "hardfork" branch, with a hardfork date in the far future.

   3. All merges into "develop" or "hardfork" are performed via github PR's and

      require review and approval from core developers \(if the PR is created

      by a core dev at least one other core dev must review and approve\).

   4. To maintain a clean history and make reviewing and merging easier,

      feature branches must be rebased onto current "develop" \(or "hardfork"\)

      before creating a PR.

   5. Merges are always done as real merges, not as fast-forwards, and not

      squashed.

3. Core devs coordinate regular merges from "develop" into "hardfork".
4. Both "develop" and "hardfork" should always remain compatible with mainnet,

   i. e. a full replay must be possible.

## How To Create a Release

For a release,

1. Bump stuff
   1. Check whether need to bump `DB_VERSION` to force a replay after upgraded:

      if there is a data schema change, or logic change that affects historical

      data, the answer is yes.

   2. FC version usually has been bumped already during development, but it

      doesn't harm if check again.

   3. Bump docs sub-module which links to wiki.
2. A "release" branch is created based on "develop" or "hardfork".
3. Update Doxyfile with the last version tag in "release" branch.
4. The "release" branch is merged into "testnet".
5. For a hardfork release, the hardfork date is adapted directly on the

   testnet branch.

6. The "testnet" branch is tagged as test-version.
7. Bugfixes for the release are created on the "release" branch and merged into

   "testnet". Additional test-versions are tagged as needed.

8. After sufficient testing, the release must be approved. In the case of a

   hardfork release, witness approval is required.

9. After approval, the mainnet hardfork date is decided and set in the "release"

   branch.

10. The "release" branch is merged into "master", and a _version_ tag is created

    on "master". Create tag by git command line and then point the release to the created tag in github. This is needed for Docker to catch the new code.

11. The "release" branch is merged back into "develop" and "hardfork".
12. The "release" branch is merged into "testnet". This will produce a merge

    conflict for the hardfork dates, which must be resolved without changing the

    testnet hardfork date.

13. Update online code documentation by using updated `Doxyfile` as config file in the `master` branch. Send pull request to [https://github.com/bitshares/doxygen.bitshares.org](https://github.com/bitshares/doxygen.bitshares.org) with new content in html format. Send pull to [https://github.com/bitshares/dev.bitshares.works](https://github.com/bitshares/dev.bitshares.works) with new content in xml format.
14. Update date of last core [download page of bitshares.org site](https://github.com/bitshares/bitshares.org/blob/master/download.php).
15. Create binaries for linux, macos and windows. Once the tag name is known create binaries for this 3 OS. Attach them to release notes. 

    Example: [https://github.com/bitshares/bitshares-core/releases/tag/3.0.0](https://github.com/bitshares/bitshares-core/releases/tag/3.0.0)

    Binaries names for this release: 

16. Linux: BitShares-core-3.0.0-Linux-cli-tools.tar.gz
17. Windows: BitShares-Core-3.0.0-Windows-x64-cli-tools.zip
18. macOS: BitShares-Core-3.0.0-macOS-cli-tools.tar.gz

**Note:** Solving conflicts by github\(web\) will merge branches in unintended directions. Avoid solving this way, merge and resolve conflicts manually through the git command line. Conflicts generally occur when merging release to testnet.

**Note 2:** Follow command line github suggestion to resolve conflicts but at the end of the process you will not have permission to merge directly to `testnet`, never push the fix to `release`. Create a new branch and push there, then create a new pull request between `testnet` and `new_branch`, merge `new_branch` to `testnet` and `release` will be automatically added to the merge.

**Note 3:** When creating tag for testnet do it from the command line with `git tag`. Github don't have the option to create a tag without a release.

**Note 4:** the tag commit can be changed. Don't change tags on github. This is a source of confusion, and of irreproducible bug reports. Make new one is better \(ex: test-3.0.1\).

**Note 5:** Do not mark releases as "pre release" unless there is a real new version coming immediately after. Never upgrade "pre release" to "release" as new emails to subscribers will not be sent when doing so.

## How To Create an Emergency Fix

An emergency fix may become necessary when a serious problem in mainnet is discovered. The goal here is to fix the problem as soon as possible, while keeping the risk for creating additional problems as low as possible.

First of all, the problem must be analyzed and debugged. This happens, naturally, directly on the release version.

Presumably the developer who creates the fix will work on his private master branch. That is OK. But for publishing the fix, the following steps should be taken:

1. The fix is applied to the version of the "release" branch that was merged

   into "master" when creating the broken release version.

2. The "release" branch is merged into "master", and a _version_ tag is created

   on "master".

3. Witnesses update to the new _version_, and production continues.
4. A unit test is created on "develop" that reproduces the problem.
5. The "release" branch is merged into "develop", and it is verified that the

   fix resolves the problem, by running the unit test.

6. The "release" branch is merged into "hardfork" and "testnet".

## How To Update the README.md

In general we want changes in the README to be visible fast in `master`.

1. Update the README.md of the `release` candidate.
2. Merge `release` to `master`.
3. Merge back `release` to `develop`.
4. Merge `develop` into `hardfork` and `testnet` if changes need to be visible in all the branches or wait until this merges occur naturally.

