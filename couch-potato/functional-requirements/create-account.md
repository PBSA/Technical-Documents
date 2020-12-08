# Create Account



The create account screen is opened from the [Home Page](home-page.md) and is the screen where every new account is created/registered.

![](../../.gitbook/assets/screen-shot-2020-03-25-at-2.57.58-pm%20%281%29.png)

**Captions**

| Text | Type | Comments |
| :--- | :--- | :--- |
| Create New Account | Static |   |
| Data Proxy | Static |   |
| \[proxy name\] | Dynamic | Value set in config-dataproxy.json |
| \*Required fields | Static |   |

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
      <td style="text-align:left">User Name</td>
      <td style="text-align:left">
        <p>Max Length: 24</p>
        <p>Min Length: 8</p>
      </td>
      <td style="text-align:left">User name</td>
    </tr>
    <tr>
      <td style="text-align:left">Password</td>
      <td style="text-align:left">
        <p>Max Length: 40</p>
        <p>Min Length: 8</p>
      </td>
      <td style="text-align:left">Password</td>
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
| REGISTER | Button | Validate all fields and then return to the [Home Page](home-page.md) |
| X | Image | Close the screen without adding a new account and return to the [Home Page](home-page.md) |

**Validation**

| **Exception** | Error Message |
| :--- | :--- |
| No user name | Username not entered |
| No password | Password not entered |
| Password too short | Password must be at least 8 characters |
| No confirm password | Confirm password not entered |
| Password and confirm password not the same | Password and Confirm Password are different |

{% hint style="warning" %}
**Note**: For the first release there will be no additional validation on the password format for strength or special characters etc. The only constraint is that the length must be &gt;=8 and &lt;= 40 characters.
{% endhint %}

