# Task 20: Create IAM Users and Assign Basic Permissions

## Cloud Computing Internship — Veda Technology

## 1. Aim

To create and examine IAM identities with different permission levels using AWS Identity and Access Management (IAM), and verify permission-based access control using the AWS IAM Policy Simulator.

## 2. Objectives

- Understand AWS IAM users, groups, and policies.
- Compare read-only and operator permission levels.
- Apply the principle of least privilege.
- Verify allowed and denied EC2 actions using the IAM Policy Simulator.
- Demonstrate resource-specific permissions for EC2 instances.

## 3. Tools and Technologies

- Amazon Web Services (AWS)
- AWS Identity and Access Management (IAM)
- AWS IAM Policy Simulator
- Amazon EC2
- GitHub for documentation and evidence

## 4. Overview

AWS IAM manages identities and controls access to AWS resources. IAM policies define which actions an identity can perform and which resources those actions apply to.

For this practical, existing test identities and custom policies were examined:

| IAM identity | Permission configuration |
|---|---|
| `veda-readonly-user` | `veda-ec2-readonly-policy` and `IAMUserChangePassword` |
| `veda-operator-user` | `veda-ec2-operator-policy` and `IAMUserChangePassword` |

The custom policies are attached through their respective IAM groups.

## 5. Implementation

### Step 1: Inspect IAM users

Opened AWS IAM and reviewed the existing users and their assigned permissions.

### Step 2: Examine the read-only policy

Reviewed `veda-ec2-readonly-policy`. It permits the following EC2 actions on all resources:

- `ec2:DescribeInstances`
- `ec2:DescribeInstanceStatus`
- `ec2:DescribeTags`

### Step 3: Test read-only permissions

Selected `veda-readonly-user` in the IAM Policy Simulator and evaluated its EC2 permissions.

|      EC2 action     |  Result |        Explanation               |
|---------------------|---------|----------------------------------|
| `DescribeInstances` | Allowed | Explicitly allowed by the policy |
| `StartInstances`    | Denied  | No matching allow statement      |
 
### Step 4: Examine the operator policy

Reviewed `veda-ec2-operator-policy`.

The policy contains two permission statements:

1. Read-only EC2 describe actions on all resources.
2. `ec2:StartInstances` and `ec2:StopInstances` restricted to a specific EC2 instance ARN in the `us-east-1` region.

### Step 5: Test operator permissions

Selected `veda-operator-user` in the IAM Policy Simulator.

|     EC2 action      |        Resource used          |  Result |
|---------------------|-------------------------------|---------|
| `DescribeInstances` | All resources (`*`)           | Allowed |
| `StartInstances`    | All resources (`*`)           | Denied  |
| `StopInstances`     | All resources (`*`)           | Denied  |
| `StartInstances`    | Policy-specified instance ARN | Allowed |
| `StopInstances`     | Policy-specified instance ARN | Allowed |

The initial start and stop tests were denied because the wildcard resource did not match the policy's resource restriction. After using the exact resource ARN from the policy, both actions were allowed by the simulator.

**Note:** These were authorization simulations only. No actual EC2 instance was started or stopped.

## 6. Screenshot Evidence

The `screenshots/` directory contains the practical evidence.

| Screenshot | Description |
|---|---|
| `01_Operator_User_Permissions` | Operator user permissions |
| `02_Operator_Policy_Details` | Operator policy details |
| `03_ReadOnly_User_Permissions` | Read-only user permissions |
| `04_ReadOnly_Policy_Summary` | Read-only policy summary |
| `05_ReadOnly_Policy_Simulator` | Selected read-only identity and policy |
| `06_ReadOnly_DescribeInstances_Test` | Read action test setup |
| `07_ReadOnly_Action_Allowed` | Read action allowed result |
| `08_ReadOnly_StartInstances_Denied` | Read-only start action denied |
| `09_Operator_StartInstances_Denied` | Operator start action denied for wildcard resource |
| `10_Operator_Action_Test_Results` | Operator action test results |
| `11_Operator_StopInstances_Test` | Operator stop action test |
| `12_Operator_Policy_JSON` | Operator policy JSON |
| `13_Operator_Resource_Permission_Result` | Resource-specific operator permission results |

## 7. Principle of Least Privilege

The principle of least privilege means granting only the permissions required to perform a task.

This practical demonstrates that:

- Read-only access permits viewing information without granting instance start or stop permissions.
- Operator permissions can be limited to a particular resource.
- IAM policy evaluation depends on the requested action and resource.
- A denied action does not necessarily mean that the action is absent from the policy; the resource scope must also match.

## 8. Security Considerations

- Use separate test identities for permission testing.
- Avoid granting unnecessary permissions.
- Enable MFA for IAM users with console access.
- Do not share passwords, access keys, secret keys, or private credentials.
- Review and remove unused test identities when safe to do so.
- Preserve existing policies unless a change is required and authorized.

## 9. Result

Successfully examined two IAM identities with different EC2 permission levels and verified allowed and denied actions using the AWS IAM Policy Simulator. Resource-specific permissions were confirmed without performing actual EC2 start or stop operations.

## 10. Conclusion

This practical demonstrated how AWS IAM policies control access to cloud resources. It also showed the importance of resource restrictions and the principle of least privilege in cloud security.

