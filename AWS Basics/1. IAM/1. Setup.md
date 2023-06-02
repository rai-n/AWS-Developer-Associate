* Set up a user to not use root user
* Attach user to a user group or manually assign permissions (not recommended)
* IAM Policy structure
1. Version e.g. 2022-10-17
2. Id optional identifier for policy e.g. S3-Permissions-Group
3. Statement e.g.
    * Sid - optional identifier for statement
    * Effect - "Allow" or "Deny" access 
    * Principal - account/user/role to which this policy is applied to
    * Action - list of actions this policy allows or denies
    * Resource - list of resources this action applies to 
    * Condition - optional condition for when this policy is in effect 

* MFA - can use virtual MFA device like google authenticator, or Universal 2nd factor security key like YubiKey, hardware key fob device or hardware key fob MDA device for AWS GovCloud
* You can set your own password policy
* Can use AWS CLI, SDK, CloudShell management console to access AWS service
1. For AWS CLI, use aws configure to log in 
2. For AWS SDK you can use programming language to interface with AWS API
3. For management console you can use the web interface to access services
4. For CloudShell you can upload, download, write shell scripts etc from a browser

* IAM Role for services, some AWS services will need permission too perform actions
* Roles allow entities in AWS to give services permissions for an amount of time
* Always aim for least privilege for users or services

* IAM security tools
1. IAM Credential Report (account level) - a report that lists all your account's users and the status of their various credentials
2. IAM Access Advisor (user level) - access advisor shows the service permissions granted to a user and when those services were last accessed, useful for revising policies