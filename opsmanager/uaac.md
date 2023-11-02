## Add Ops Manager Users
This section describes how to add or remove users with UAAC. If you do not already have the UAAC installed, run gem install cf-uaac on the command line.
Note: You can only manage users on the Ops Manager UAA module if you chose to use Internal Authentication instead of an external Identity Provider when configuring Ops Manager.

To add Ops Manager users, do the following:

1. Target your Ops Manager UAA:
```
uaac target https://YOUR-OPSMANAGER-FQDN/uaa/
```
Where:
`YOUR-OPSMANAGER-FQDN` is the fully qualified domain name of your Ops Manager installation.

Get your token:
```
uaac token owner get
Client ID: opsman
Client Secret:
Username: OPSMANAGER-ADMIN-USERNAME
Password: OPSMANAGER-ADMIN-PASSWORD
 
Successfully fetched token via client credentials grant.
Target https://YOUR-OPSMANAGER-FQDN/uaa/
```

Where:
YOUR-OPSMANAGER-FQDN is the fully qualified domain name of your Ops Manager installation.
OPSMANAGER-ADMIN-USERNAME and OPSMANAGER-ADMIN-PASSWORD are the username and password for the Ops Manager admin user.
Note: The Client Secret field does not require a value.

Add a user.
```
uaac user add USER-NAME -p USER-PASSWORD --emails USER-EMAIL@EXAMPLE.COM
```
Where:
`USER-NAME` is the username of the user you are adding.
`USER-PASSWORD` is the password with which this user authenticates.
`USER-EMAIL` is the email address associated with this user.
(Optional) Set the Role-Based Access Control (RBAC) permissions for your user. For more information, see Configuring Role-Based Access Control (RBAC) in Ops Manager.

