## AWS IAM
IAM or Identity and Access Management is a system in AWS which allows authentication and authorization within a AWS account. This can be used to allow certain users of the account to gain control over certain resources as well as setup permissions for AWS resources such as EC2 to gain access and interact with other services within the account.

## IAM User
An IAM user is essentially an account with certain permissions that can be used to access the larger AWS account. This sub account can allow or restrict access to certain resources depending on how the IAM user is created.

## User Credentials
Each [[#IAM User|user]] also consists of user credentials that allow that specific [[#IAM User|user]] to login to the AWS account. It would have different credientials for each [[Interacting with AWS|way of access]] to the AWS resources.

## IAM Groups
When creating [[##IAM Policies|policies]] for [[##IAM User|users]] it is possible to just create [[##IAM Policies|policies]] for each and every user. However, it is usually best practice to assign [[##IAM Policies|policies]] to IAM groups which are essentially clusters of [[#IAM User|users]]. When a [[##IAM Policies|policy]] is assigned to a group, all the [[#IAM User|users]] under that group inherit that [[##IAM Policies|policy]]. Each group can contain multiple [[#IAM User|users]], and each [[#IAM User|user]] can belong to many groups.

## IAM Policies
IAM policies determine the specific action that whatever the policy is assign to can or cannot perform.

### Examples
Policies are stored as JSON within AWS:
```JSON
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": "*",
			"Resource": "*",
			"Condition": {
				// Conditions Here
			}
		}
	]
}
```
**Details**
- Version - Version of the policy language
- Statement - Contains an array of policies
- Effect - Specify if the specified API call should be allowed or denied
- Action - Describes the specific action or API call that the policy is set for
- Resource - Limits this policy to a specific resource such as a specific EC2 instance, usually through the ARN
- Condition - Further conditions to limit the specified policy

Example of a more granular IAM policy:
```JSON
{  
	"Version": "2012-10-17",  
	"Statement":
	[
		{  
			"Effect": "Allow",  
			"Action": [  
				"iam: ChangePassword",  
				"iam: GetUser"  
			],
			"Resource": "arn:aws:iam::123456789012:user/${aws:username}"  
		}
	]  
}
```
