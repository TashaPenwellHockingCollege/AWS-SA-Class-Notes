
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

      - 
