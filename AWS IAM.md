# AWS Identity and Access Management (AWS IAM)

## Objectives
By the end of this lab, you will be able to:
* Explore IAM Users and Groups.
* Inspect IAM policies applied to groups.
* Follow a real-world scenario that adds users to groups and explores group permissions.
* Locate and use the IAM sign-in URL.
* Experiment with policies and service access.

---

## Overview
The following IAM resources are provisioned for this lab:

### Users
* **user-1**
* **user-2**
* **user-3**

### Groups & Policies
| Group | Policy Name | Permissions |
| :--- | :--- | :--- |
| **S3-Support** | AmazonS3ReadOnlyAccess | Read-Only access to Amazon Simple Storage Service (Amazon S3). |
| **EC2-Support** | AmazonEC2ReadOnlyAccess | Read-Only access to Amazon Elastic Compute Cloud (Amazon EC2). |
| **EC2-Admin** | EC2-Admin-Policy (Inline) | Ability to View, Start, and Stop EC2 instances. |

---

## Task 1: Explore and Manage IAM Users and Groups

### 1.1: Explore IAM Users, Groups, and Policies
1. **Launch the Lab:** Choose **Start Lab** at the top of the page.
   > **Caution:** You must wait for the provisioned AWS services to be ready before you can continue.
2. **Open the Console:** Choose **Open Console**. You will be automatically signed in.
   > **Warning:** Do not change the Region unless instructed.
3. **Navigate to IAM:** Search for and choose **IAM** in the AWS Management Console search bar.
4. **Inspect Users:**
   * Choose **Users** in the left navigation pane.
   * Review `user-1`, `user-2`, and `user-3`.
   * Note that `user-1` initially has no permissions or group memberships.
5. **Inspect Groups:**
   * Choose **User groups** in the left navigation pane.
   * Choose the **EC2-Support** group and navigate to the **Permissions** tab.
   * Expand the **AmazonEC2ReadOnlyAccess** policy to see the JSON structure (Effect, Action, Resource).
   * Note that this managed policy allows listing and describing information for EC2, CloudWatch, and Auto Scaling.
6. **Compare Policy Types:**
   * **Managed Policies:** Pre-built by AWS or administrators (e.g., `AmazonS3ReadOnlyAccess`).
   * **Inline Policies:** Assigned to a specific user or group for one-off situations (e.g., `EC2-Admin-Policy` in the **EC2-Admin** group).

### 1.2: Manage Users and Groups
In this task, you will assign users to groups based on the following business scenario:

| User | Group | Permissions |
| :--- | :--- | :--- |
| `user-1` | **S3-Support** | Read-Only access to Amazon S3 |
| `user-2` | **EC2-Support** | Read-Only access to Amazon EC2 |
| `user-3` | **EC2-Admin** | View, Start, and Stop Amazon EC2 instances |

**Steps to add users to groups:**
1. In **User groups**, choose the group name (e.g., **S3-Support**).
2. In the **Users** tab, choose **Add users**.
3. Select the appropriate user (e.g., `user-1`) and choose **Add users**.
4. Repeat for `user-2` (to **EC2-Support**) and `user-3` (to **EC2-Admin**).
5. Verify that each group now shows a `1` in the **Users** column.

---

## Task 2: Use the IAM Sign-in URL

### 2.1: Locate and Access the IAM Sign-in URL
1. Choose **Dashboard** in the IAM left navigation pane.
2. Locate the **Sign-in URL for IAM users** in the **AWS Account** section.
3. Copy this URL (Format: `https://[Account_ID].signin.aws.amazon.com/console`) to a text editor.

### 2.2: Log in with Different IAM Users
To test permissions without logging out of the main console, use a **Private/Incognito window**.

#### Sign in as user-1 (S3 Support)
1. Paste the sign-in URL into a private window.
2. Log in as `user-1` using the provided password.
3. **Test S3 Access:** Search for **S3** and open the bucket with `s3bucket` in its name. You should be able to see the contents.
4. **Test EC2 Access:** Search for **EC2**. Navigate to **Instances**.
   * *Expected Result:* You should receive a "Not authorized" error.

#### Sign in as user-2 (EC2 Support)
1. Sign out of `user-1` and sign in as `user-2`.
2. **Test EC2 Access:** Navigate to **Instances**. You should be able to see the running instance.
3. **Test Permissions:** Select the instance and attempt to **Stop** it.
   * *Expected Result:* You should receive an error stating you are not authorized to perform this operation (Read-Only access).
4. **Test S3 Access:** Search for **S3**.
   * *Expected Result:* The console should return an empty list or "Access Denied."

#### Sign in as user-3 (EC2 Admin)
1. Sign out of `user-2` and sign in as `user-3`.
2. **Test Admin Permissions:** Navigate to **EC2 > Instances**.
3. Select the instance and choose **Instance state > Stop instance**.
4. Confirm by choosing **Stop**.
   * *Expected Result:* The instance successfully enters the "Stopping" state.

---

## Lab Complete
Congratulations! You have successfully:
* Managed IAM users and groups.
* Verified the difference between Managed and Inline policies.
* Tested granular service access using the IAM sign-in URL.
