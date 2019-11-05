# Style-Guide

### Standardization

This Cording Style Guide are specific to BitShares-Core codebase. It will save time to the programmer and reviewer.

### Discussion

* [Define coding style guidelines \#1318](https://github.com/bitshares/bitshares-core/issues/1318)
* Additional Context \(optional\)
  * [https://isocpp.org/wiki/faq/coding-standards](https://isocpp.org/wiki/faq/coding-standards)

## General

* Use `FC_ASSERT` instead of `assert` \(reason: `assert()` checks are executed conditionally depending on compile-time settings, which endangers consensus\)
* Use `using` instead of `typedef`

## Formatting

* Maximum line length: 118 \(\*That's what you can see on github without side-scrolling\)
* newline at the end of each file.
* Avoid multiple empty new lines in code, in general 1 empty line is enough to separate blocks of code.
* Long calls, definitions or declarations are recommended to use this style: [496c622](https://github.com/bitshares/bitshares-core/commit/496c6229e13bd511c2380f9c8d540e68bd65a65d)
* TBD spaces

### Discussion - _spaces_ and _padding_

* 3 spaces indentation
  * [Comment](https://github.com/bitshares/bitshares-core/issues/1318#issuecomment-472376797)
* [4 spaces of indentation](https://github.com/bitshares/bitshares-core/issues/1318#issuecomment-468077506), and do away with padding the insides of parentheses.
  * [Comment](https://github.com/bitshares/bitshares-core/issues/1318#issuecomment-472125824)
* for function and method calls, there is _no_ space between function name and opening parenthesis
* for control statements there _is_ a space between statement and opening parenthesis

**Suggestions**

* We should
  * come to a consensus regarding formatting rules
  * find a tool that can do it automatically
  * make a big whitespace-only change

## Comments

* Remove commented code. Comments can be made when code seems not enough to explain an idea, that comments are generally used in test cases and explain in plain English what is happening. Commented code should not be submitted, just delete it. 
* 
## Naming

* Method names should be all lowercase with "\_" as word separator: [\#1271 \(comment\)](https://github.com/bitshares/bitshares-core/pull/1271#discussion_r224833813)
* 
## Scoping

* lambda expressions must capture variables explicitly if what you need is not too many.
* In a huge refactoring please do it in small steps in individual commits that are easily verifiable. This makes the reviewer life a bit easier. - [From: \#1413 \(comment\)](https://github.com/bitshares/bitshares-core/pull/1413#issuecomment-437932230)
* Avoid global variables. From [\#1324 \(comment\)](https://github.com/bitshares/bitshares-core/pull/1324#issuecomment-439715251)

## Classes

* * 
## Functions

* When defining a function:  `{` should always be in a new line.
* 
## Exception Rules

* * 
## Architecture

* Create/modify/delete of the database inside operation evaluator must be done in `do_apply` and never in `do_evaluate` - [From \#1449 \(comment\)](https://github.com/bitshares/bitshares-core/pull/1449#discussion_r236381016)  
* When new fields are added or removed to objects; a bump of the database is needed. [From: \#1449 \(review\)](https://github.com/bitshares/bitshares-core/pull/1449#pullrequestreview-185101298) 

## Pull Requests

* Pull requests should add/modify/delete the minimum amount code possible to develop 1 and only 1 feature or concept. [From \#1467](https://github.com/bitshares/bitshares-core/pull/1467) - Pull doing several stuff at once.
* Pull requests in bitshares can only be made against 2 branches: `develop` if there is a non consensus related change or `hardfork` if it is modifying consensus.

