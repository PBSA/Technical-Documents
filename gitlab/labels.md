# Labels

[https://gitlab.com/groups/PBSA/-/labels](https://gitlab.com/groups/PBSA/-/labels)

## Scoped Labels

The scoped labels \(ending with ::\) are mutually exclusive. Only one of these labels can be assigned at a time to an issue.

### Priority::

* low 
* medium priority
* high priority

### State::

* pending 
* accepted
* in progress
* in review
* in testing
* completed
* on hold
* blocked
* revision needed

### Type::

* bug
* feature
* documentation
* question
* maintenance 

## Other Labels

* duplicate
* discussion
* security

## Issue Lifecycle

All issues should have a `state` and `type` scoped label attached to them. The following is a proposed lifecycle an issue should have:

1. Pending - Issues that are newly created and haven't been reviewed.
2. Accepted - Issues that are newly created and have been accepted after review.
3. In Progress - Issues that are currently being worked on.
4. In Review - Issues that require a proposed solution to be reviewed.
5. In Testing - Issues that require a proposed solution to be reviewed. 
6. Completed - Issues that are closed with a working solution.

This will cover a standard lifecycle of an issue. There are other `state` labels for issues that are not being worked on:

* On Hold - Issues that were at least accepted, that should not be worked on at the moment.
* Blocked - Issues that cannot be worked on due to other factors linked to it.
* Revision Needed - Issues that need to be revised before being accepted.

## Glossary

<table>
  <thead>
    <tr>
      <th style="text-align:left">Label</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p></p>
        <p>priority::low</p>
      </td>
      <td style="text-align:left">(optional) Issues that are marked as low priority.</td>
    </tr>
    <tr>
      <td style="text-align:left">priority::medium</td>
      <td style="text-align:left">(optional) Issues that are marked as medium priority.</td>
    </tr>
    <tr>
      <td style="text-align:left">priority::high</td>
      <td style="text-align:left">(optional) Issues that are marked as high priority.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::pending</td>
      <td style="text-align:left">Issues that are newly created and haven&apos;t been reviewed.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::accepted</td>
      <td style="text-align:left">Issues that are newly created and have been accepted after review.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::in progress</td>
      <td style="text-align:left">Issues that are currently being worked on.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::in review</td>
      <td style="text-align:left">Issues that require a proposed solution to be reviewed.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::in testing</td>
      <td style="text-align:left">Issues whose proposed solution is being tested.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::completed</td>
      <td style="text-align:left">Issues that are closed with a working solution.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::on hold</td>
      <td style="text-align:left">Issues that were at least accepted, that should not be worked on at the
        moment.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::blocked</td>
      <td style="text-align:left">Issues that cannot be worked on due to other factors linked to it.</td>
    </tr>
    <tr>
      <td style="text-align:left">state::revision needed</td>
      <td style="text-align:left">Issues that need to be revised before being accepted.</td>
    </tr>
    <tr>
      <td style="text-align:left">type::bug</td>
      <td style="text-align:left">Issues that are an unexpected, or incorrect result in the project.</td>
    </tr>
    <tr>
      <td style="text-align:left">type::feature</td>
      <td style="text-align:left">Issues that bring new functionality to the project.</td>
    </tr>
    <tr>
      <td style="text-align:left">type::documentation</td>
      <td style="text-align:left">Issues that relate to the documentation of the project.</td>
    </tr>
    <tr>
      <td style="text-align:left">type::question</td>
      <td style="text-align:left">Issues that are questions related to the project.</td>
    </tr>
    <tr>
      <td style="text-align:left">type::maintenance</td>
      <td style="text-align:left">Issues that require maintenance on the project.</td>
    </tr>
    <tr>
      <td style="text-align:left">duplicate</td>
      <td style="text-align:left">(optional) Issues that are duplicates of existing issues in the same project.</td>
    </tr>
    <tr>
      <td style="text-align:left">discussion</td>
      <td style="text-align:left">(optional) Issues that discuss specific project changes.</td>
    </tr>
    <tr>
      <td style="text-align:left">security</td>
      <td style="text-align:left">(optional) Issues that relate to the security of the project.</td>
    </tr>
  </tbody>
</table>



