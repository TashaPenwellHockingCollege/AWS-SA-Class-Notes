
# Securing the Root User
    * the account root user has a large amount of power.  Recommended security steps:
      1.  Create an IAM admin user
      2.  Lock away the root user credentials
      3.  Further access to account for most tasks via AWS Console, AWS CLI or AWS Tools and SDK
      

# AWS IAM
  * Securely control individual and group access to your AWS resources
  * Integrates w/ other AWS services
  * Federated identity management
  * Granular permissions
  * Support for MFA


# IAM Components:  Review
  * IAM User
      - defined in your AWS account.  
      - Use credentials to authenticate programmatically or via the AWS Console
  * IAM Group
      - collection of IAM users that are granted identical authorization
  * IAM Policy
      - defines which resources can be accessed and the level of access to each resource
      - Mechanism to grant temp access for making AWS service requests, Assumable by a person, application or service

# IAM Permissions
  * Permissions are specified in an IAM policy
  * Documented in JSON
  * Defines which resources and ops are allowed
  * Best Practice:  follw the principle of least privilege
  * Two types of policies:
        1.  Identity-Based:  attach to an IAM principal
        2.  Resource-based:  Attach to an AWS resource
  * How IAM determines permissions at the time of request
        Q:  is the permission explicitly denied?
        A:  Yes --> Deny access;  No --> Is the Permission Explicitly Allowed?
        A:  Yes --> Allow access; No --> Implicit deny

# Identity-Based vs Resource-Based Policies
  * Identity-Based Policies
      ** attached to a user, group, or role
      ** types of policies incluce
            1.  AWS Managed: created and managed by AWS; can attach them to multiple users groups and roles in your AWS account; provide a good way to start config access if new to policies; scale well for more advanced access control admin
            2.  Customer Managed:  polciies you create and manage; can be customized to meet your needs
            3.  Inline:  policies you create and manage but are different than customer managed policies becasue the policy is embedded directly into single user, group or role
            4.  
  * Resource Based Policies
      ** Attached to AWS Resources 
          *** example:  Attach to an S3 bucket
      ** always an inline policy
      ** control which actions a principle can perform on a resource and under what conditions
      ** there are no managed resource-based policies
      
# IAM Policy Document Structure
    * EFFECT:  Effect can be either Allow or Deny
    * ACTION:  Type of access that is allowed or denied
    * RESOURCE:  Resources that the action will act on
                  optional if create a resource-based policy
    * CONDITION:  Conditions that must be met for the rule to apply
    
    
# ARNs and Wildcards
    * Resources are identified by using ARN format
      **  Syntax - arn:partition:service:region:account:resource
      **  Example - "Resource":  "arn:aws:iam::123456789012:user/major"
    * You can use a wildcard to give access to all actions for a specific AWS service
      ** Examples:  
            "Action": "s3:*"
            "Action": "iam:*AccessKey*"

  
  
#  AWS CloudTrail
   * Logs and monitors user activity
         ** enables governance, compliance and auditing of AWS account
         ** enables you to track and automatically respons to account activity that threatens security of AWS resources
         ** can continuously monitor user activity on your AWS infrastructure
   * Provides event history of AWS account
        ** Actions taken through the AWS Mangement Console, SDK, CLI
        ** Increases visibility into your user and resource activity
        ** 90-day event history provided by default at no cost
   * Identify
         1.  Who accessed your account
         2.  When and from where
         3.  What action they took on an AWS service
   *  Helpful tool to
         1.  Perform security analysis
         2.  Discover which calls were block (for example, IAM policies


## Key Takeways
   * Don't use the account root user for common tasks.  Instead, create and use IAM user credentials
   * Permissions for accessing AWS account resources are defined in one or more IAM policy documents
         ** Attached IAM policies to IAM users, groups or roles
   * When IAM determines permission, an explicity Deny will always override any Allow statement
   * It is best pract to follow the principle of least privilege when granting access


## IAM Groups
   * Use IAM groups to grant the same access rights to multiple users
   * All users in the group inherit the permissions assigned to the group.
         ** makes it easier to manage access across multiple users
   * Combine approachs for fine-grained individual access
         ** Add the user to a group to apply standard access based on job function
         ** optionally attach an additional policy to the user for needed exeptions
         
## Example IAM Groups
   * Create groups that reflect job functions
      ** if a new developer is hired, add them to the Developer group
         *** immediately inherit the same access granted to other developers
   * users can below to more than one group; if one group has more lenient policies than another for a user, the access rights for the most restrictive policy applies


## Federating Users

# IAM Roles
   * IAM Role Characteristics
      ** provides temporary security credentials
      ** is not uniquely assocated w/ one person
      ** is assumable by a person, app or service
      ** is used to delegate access
   
   * Use Cases
      ** provide AWS resources w/ access to AWS services
      ** provide access to externally authenticated users
      ** provide access to third parties
      ** switch roles to access resources in your AWS account and any other AWS account (cross-account access)
      
# Grant Permissions to Assume a Role
   * for an IAM user, app or service to assume a role you must grant permissions to switch to the role
   * AWS Security Token Service (AWS STS)
         ** web service that enables you to request temp , limited-privilege credentials
         ** Credentials can be used by IAM users or for users that you authenticate (federated users)
   * Example policy - Allows an IAM user to assume a role
         {
            "version": "2012-10-17",
            "Statement": {
               "Effect": "Allow",
               "Action": "sts:AssumeRole",
               "Resource": "arn:aws:iam::123456789012:role/Test*"
             }
         }

# Role-Based Access Control (RBAC)
   * Traditional Approach to Access Control 
         1.  Grant users specific permissions based on job function (i.e. DBA)
         2.  Create a distince IAM role for each permission combination
         3.  Update permissions by adding access for each new resource (it can be time-consuming to keep updating policies)

# Best Practice:  Tagging
   * a tag consists of a name and (optionally) a value
         1.  Can be applied to resources across your AWS accounts
         2.  Tag keys and values are returned by many different API ops
   * Define custom tags
   * Multiple practice uses
         ** i.e. billing, filtered views, access controls, etc
   * Example tags applied to an EC2 instance
         Name = web server
         Project = unicorn
         Stack = dev
   * Tags can also be applied to IAM users or IAM roles
   * can create up to 50 tags per resource

# Attribute-Based Access Control (ABAC)
   * highly scalable approach to access control
         ** attributes are a key or a key-value pair, such as a tag
         ** Example attributes
               Team = Developers
               Project = Unicorn
   * Permissions (policy) rules are easier to maintain w/ ABAC than RBAC
   * Benefits
      ** permissions auto applied, based on attributes
      ** Granular permissions are possible w/o a permissions update for eveyr new user or resource
      ** fully auditable
      
# Applying ABAC to your Organization
   * How to apply ABAC to your organization
         1.  Set access control attributes on identities
         2.  Require attributes for new resources
         3.  Configure permissions based on attributes
         4.  Test
               a) Create new resources
               b) Verify that permissions auto apply

## FEDERATING USERS (PART 2)

# Externally Authenticated Users
   * Identity Federations
      * User authentication completed by a system that is external to the AWS account (i.e. corporate directory)
      * It provides a way to allow access through existing identities, w/o creating IAM users
      * Options
            1.  AWS STS
                  * public Identity Service Providers (IdPs)
                  * Custom identity broker application
            2.  Security Assertion Markup Language (SAML)
            3.  Amazon Cognito
   * IdP Authentication Overview
      User access identity broker via application --> identity broker authenicates users --> request temp credentials for AWS STS --> temp credentials returned to app

   * Identity Federation w/ an Identity Broker Process
   Step 1:  (Customomer System) User access broker via an application --> request goes to identity broker
   Step 2:  (Customer System) User is authenticated in the corporate identity store
   Step 3:  Broker retrieves temp security credentials from AWS STS --> passes those credentials to the user application (from Step 1)
   Step 4: User access AWS services/console   
   
# Identity Federation using SAML
   Step 1:  (Customer system) User navigates to a URL --> that connects to the IdP Portal that handles the SAML trust between your organization and AWS 
   Step 2:  (Customer system) IdP authenicates user on the Identity store (LDAP or MS Active Director)
   Step 3:  (Customer system) The IdP portal returns the authenication response as a SAML assertion
   Step 4:  client posts the SAML assertion to the AWS sign-in endpoint for SAML --> the sign-in endpoint communicates w/ AWS STS  to invoke the AssumeRole w/ SAML operation --> the op rquests temp security credentials and constructs a sign-in URL
   Step 5:  client receives temp security credentials from the SAML endpoint --> client is then redirected to the AWS console and authenticated w/ temp AWS security credentials
   
   
# Amazon Cognito
   * Third identity federation options
   * Cognito is a fully managed service
   * provides authenication, authorization and user management for web/mobile apps
   * Amazon Cognito provides web identity federation
      ** they can be used as the identity broker that supports IdPs that are compatiable w/ OpenID Connect (OIDC)
   * Federated Identities
      ** users sign in w/ social identity providers (Amazon, FB, Google, etc) or w/ SAML
   * User pools
      ** can maintain a directory w/ user profiles authentication tokens
   * Example
      Step 1:  User uses a web/mobile app --> authentiates and gets tokens through Amazon Cognito User Pool (AWS Cloud)
      Step 2:  Cognito redirects to the Federating IdP --> the request comes back from the Federating IdP to the Cognito user pool w/ an IdP token --> The token goes from Cognito user pool to the web/app requested
      Step 3:  Web/app exchanges tokens for AWS credentials w/ the Cognito identity pool -- the Cognito identity pool send AWS credentials back to the web/mobile app
      Step 4:  The web/mobile app accesses AWS services w/ credentials to the AWS services

# Key Takeaways
   * IAM roles provide a temp security credentials assumable by a person, app or service
   * the AWS Security Token Service (AWS STS) enables you to request temp AWS credentials
   * w/ identity federation, user authenication is external to the AWS acccount
      * accompblished by using AWS STS, SAML or Cognito
      
