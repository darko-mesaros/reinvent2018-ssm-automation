# reinvent2018-ssm-automation

Here be automations

### Safely and securely perform disruptive tasks at scale

### Description
Use either **AWS-RestartEC2Instance, AWS-StopEC2Instance or AWS-StartEC2Instance** Automation document to target an AWS Resource Group that includes multiple instances. The Automation workflow must be configured to use rate controls. 

### Prerequisites

- IAM Instance profiles
- Amazon EC2 Instance(s)
- Resource Group

### Create an IAM instance profile for Systems Manager managed instances
By default, Systems Manager doesn't have permission to perform actions on your instances. You must grant access by using an IAM instance profile. Complete the following steps to create an IAM instance profile role with Systems Manager permissions.

1.	Open the IAM console at https://console.aws.amazon.com/iam/.

2.	In the navigation pane, choose **Roles**, and then choose Create **role**.

![iam_roles](images/IAMRoles.png)

3.	On the Select type of trusted entity page, under AWS Service, choose **EC2**, and then choose **Next: Permissions**.

![iam_ec2](images/IAMEC2.png)

4.	On the Attached permissions policy page, find and add **AmazonEC2RoleforSSM** policy, and then choose **Next: Review**.

5.	On the Review page, type a name in the Role name box, and optionally type a description. Then choose **Create role**. In the example below role name is **builder1ec2role**.

![iam_rolereview](images/IAMRoleReview.png)

### To create an instance that uses the Systems Manager instance profile
Ensure you have at least two Amazon EC2 instances.	

1.	Open the Amazon EC2 console at https://console.aws.amazon.com/ec2/.

2.	Choose **Launch Instance**.

![ec2_Launch](images/EC2Launch.png)

3.	On the **Choose an Amazon Machine Image (AMI)** page, locate the AMI for the instance type you want to create, and then choose **Select**. In the example I selected **Microsoft Windows Server 2016 Base**.

![ec2_windowsami](images/EC2WindowsAmi.png)

4.	Choose **Next: Configure Instance Details**.

![ec2_nextconfigure](images/EC2NextConfigure.png)

5.	On the **Configure Instance Details** page, in the IAM role drop-down list, choose the instance profile you previously created, in the example the instance profile is **builder1ec2role**. Then choose **Next: Add Storage**.

![ec2_iamrole](images/EC2IAMRole.png)

6.	Choose **Next: Add Tags**.

7.	On the **Add Tags** page, choose **Add Tag**.

![ec2_addtag](images/EC2Tag.png)

8.	Fill in the **Key** and **Value** for your tag. In the example, Key is set to **environment** and Value set to **builder1**.

![ec2_addtag](images/EC2AddTag.png)

9.	Complete the remaining steps in the wizard.

### Create a Resource Group

1.	Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2.	In the left navigation pane, under **Resource Groups**, choose **Saved Resource Group**.

![rg_saved](images/RGSaved.png)

3.	In the menu bar, choose **Create resource group**.

![rg_create](images/RGCreate.png)

4.	In the dropdown list for **Select resource types**, choose **AWS::EC2::Instance**.

5.	Type in the Tag key and value of the tag created on the EC2 Managed Instance. In the example, Tag key is **environment** and value is **builder**. Choose the **+** button to add the tag.

![rg_tag](images/RGTag.png)

6.	In the **Group details** section, fill in the Group name, in my example Group name is **Builder1-AllManagedEC2Instances**. 

![rg_group](images/RGGroup.png)

7.	Choose **Create group**.

![rg_creategroup](images/RGCreateGroup.png)

### Execute Automation Document

1.	Open the AWS Systems Manager console at https://console.aws.amazon.com/systems-manager/.

2.	In the left navigation pane, choose **Automation**.

![at_actions](images/AT-Actions.png)

3.	On the menu bar, choose **Execute Automation**.

![at_execute](images/AT-Execute.png)

4.	In the Automation document list, Use either the Search bar or the numbers to the right of the Search bar to find the Automation document named **AWS-RestartEC2Instance**.

![at_document](images/AT-Document.png)

5.	Select the Automation document **AWS-RestartEC2Instance**. Then, under **Document details**, select **Default version at runtime** as the Document version and choose **Next**.

![at_documentdetails](images/AT-DocumentDetails.png)

6. Choose **Rate control**. 

7. Under Targets, for Parameter select **InstanceId**, and then for Resource Group select the **Resource Group** you created as part of the prerequisites.

8. Under **Rate Control**, for **Concurrency**, select **1** for the target.

![at_concurrency](images/AT-Concurrency.png)

9. Under **Error threshold**, select errors, then **1**.

![at_error](images/AT-Error.png)

10.	Now we ready to run the Automation document, choose **Execute**.

![at_executeautomation](images/AT-ExecuteAutomation.png)

11.	You should now see, Automation execution has been initiated and in progress. 

![at_inprogress](images/AT-InProgress.png)

12.	Automation execution has successfully completed. 

![at_success](images/AT-Success.png)
