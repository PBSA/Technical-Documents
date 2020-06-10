# Change Password

The Change Password screen is opened by clicking on the `Change Password` menu item in the [account menu.](dashboard/account-menu.md)

![](../../.gitbook/assets/image%20%2819%29.png)

**Captions**

| Text | Type | Comments |
| :--- | :--- | :--- |
| Change Password | Static |   |
| Required fields\* | Static |   |

**Inputs**

<table>
  <thead>
    <tr>
      <th style="text-align:left">Name</th>
      <th style="text-align:left">Constraints</th>
      <th style="text-align:left">Placeholder Text</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Current Password</td>
      <td style="text-align:left">
        <p>Max Length: 40</p>
        <p>Min Length: 8</p>
      </td>
      <td style="text-align:left">Current Password</td>
    </tr>
    <tr>
      <td style="text-align:left">New Password</td>
      <td style="text-align:left">
        <p>Max Length: 40</p>
        <p>Min Length: 8</p>
      </td>
      <td style="text-align:left">New Password</td>
    </tr>
    <tr>
      <td style="text-align:left">Confirm Password</td>
      <td style="text-align:left">
        <p>Max Length: 40</p>
        <p>Min Length: 8</p>
      </td>
      <td style="text-align:left">Confirm Password</td>
    </tr>
  </tbody>
</table>

**Actions**

| Caption | Type | Action |
| :--- | :--- | :--- |
| CHANGE PASSWORD | Button | Validate all fields then update the password and return to the [Dashboard](dashboard/) |
| X | Image | Close the screen without adding a new account and return to the [Dashboard](dashboard/) |

**Validation**

| **Exception** | Error Message |
| :--- | :--- |
| No current password | Current password not entered |
| Current password is wrong | Current password is incorrect. |
| No new password | New password not entered |
| New password too short | New password must be at least 8 characters |
| No confirm new password | Confirm new password not entered. |
| Confirm new password too short | Confirm new password must be at least 8 characters |
| New and confirm new don't match | New password and confirm new password are different |

{% hint style="warning" %}
**Note**: For the first release there will be no additional validation on the password format for strength or special characters etc. The only constraint is that the length must be &gt;=8 and &lt;= 40 characters.
{% endhint %}

