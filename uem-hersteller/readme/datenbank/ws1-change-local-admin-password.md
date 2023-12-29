# WS1 - Change Local Admin Password

Falls LDAP nicht mehr funktioniert und der Kunde den Login mit einem lokalen Admin nicht mehr weiß, lässt sich mit folgendem Script das Passwort und das Salt eines Users in der Datenbank ändern.

User CoreID Finden

`Select` `* from` `CoreUser where` `UserName = '<USERNAME>';`

Password und Salt ändern

| <p><code>-- This Updates the password to 'a123b456c789'</code></p><p><code>UPDATE</code> <code>CoreUser SET</code> <code>Password</code> <code>= 'c2otxl1SURGxVibCX1K9IJvyizYl4ylnIIfXhwNtfe1iCuuVM8LNPK1oWWSwE3C3BB3AYxspGqrfXaVnryxjzw=='</code> <code>,PasswordSalt = '6eklCHP5ixaMT9RREfQbmi4Z2jc='</code> <code>WHERE</code> <code>CoreUserID = &#x3C;COREUSERID></code></p> |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
