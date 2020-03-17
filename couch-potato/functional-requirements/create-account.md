# Create Account



The create account screen is opened from the [Home Page]() and is the screen where every new account is created/registered.

![](../../.gitbook/assets/image%20%2816%29.png)

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
    <tr>
      <td style="text-align:left">Email Address</td>
      <td style="text-align:left">Max Length: 60</td>
      <td style="text-align:left">Email Address</td>
    </tr>
  </tbody>
</table>**Actions**

| Caption | Type | Action |
| :--- | :--- | :--- |
| REGISTER | Button | Validate all fields and then return to the [Home Page]() |
| X | Image | Close the screen without adding a new account and return to the [Home Page]() |

**Validation**

| **Exception** | Error Message |
| :--- | :--- |
| No user name | Username not entered |
| No password | Password not entered |
| Password too short | Password must be at least 8 characters |
| No confirm password | Confirm password not entered |
| Password and confirm password not the same | Password and Confirm Password are different |
| No email | Email not entered |
| Invalid email, format must be xxx@xxx.xxx | Email is not valid |

{% hint style="warning" %}
**Note**: For the first release there will be no additional validation on the password format for strength or special characters etc. The only constraint is that the length must be &gt;=8 and &lt;= 40 characters.
{% endhint %}

