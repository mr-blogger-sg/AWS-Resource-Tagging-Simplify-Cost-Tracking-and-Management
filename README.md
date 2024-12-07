# AWS Resource Tagging: Simplify Cost Tracking and Management

Tagging your AWS resources is a fundamental practice that can save you money, improve resource management, and streamline operations. Tags are key-value pairs that add metadata to your resources, allowing you to identify and organize them efficiently. This simple strategy enhances visibility and accountability in your cloud environment.

### Why Tagging Matters:
1. **Cost Tracking:** By tagging resources, you can allocate costs to specific departments, projects, or teams. This ensures accurate financial reporting and budget tracking.
2. **Resource Management:** Tags help identify resources, preventing unused or redundant services from accumulating.
3. **Automation:** Tags allow automation scripts and monitoring tools to function more effectively by targeting specific groups of resources.

Now let’s explore a hands-on experience with tagging by creating an AWS Lambda function that automatically tags EC2 instances using Python.

---

## Step-by-Step Guide: Creating a Lambda Function for Tagging EC2 Instances

### Services Used:
- **EC2 Instance** (for testing purposes)
- **AWS Lambda**
- **IAM** (for creating roles and attaching policies)

---

### Step 1: Create a Default EC2 Instance
Launch a basic EC2 instance from the AWS Management Console. This will serve as the resource to test your tagging function.
![image](https://github.com/user-attachments/assets/b54230d6-2ad3-4113-b789-de7863c649e2)

---

### Step 2: Create IAM Policies
1. Go to the **IAM Dashboard** → **Policies** → **Create Policy**.
2. Create a policy with permissions for EC2 tagging. Example policy name: **EC2Tags**.
![image](https://github.com/user-attachments/assets/fd6ec19e-b232-4f24-9eda-99496c28ddaa)
3. Add an appropriate description for clarity and click **Create Policy**.
![image](https://github.com/user-attachments/assets/75d80d43-d8ce-4f30-a5fa-17c648720168)

---

### Step 3: Create a Role and Attach Policies
1. Navigate to **IAM Dashboard** → **Roles** → **Create Role**.
2. Choose **AWS Service** → Select **Lambda** → Click **Next: Permissions**.
![image](https://github.com/user-attachments/assets/a19111a4-571d-4635-9def-2b40bc01a1fd)
3. Attach the following policies:
   - **AWSLambdaBasicExecutionRole**
   - **EC2Tags** (created in Step 2).
4. Provide a name (e.g., **RoleTags**) and description for the role. Click **Create Role**.
![image](https://github.com/user-attachments/assets/8c91da14-589d-45fe-80a6-dd59f80dac94)

*Note:* This permission allows Lambda to manage tags on EC2 instances.

---

### Step 4: Attach the IAM Role to Lambda
1. In the **Lambda Dashboard**, create or select an existing function.
   - Runtime: Python 3.8.
   - Execution Role: Choose the IAM role created (e.g., **RoleTags**).
![image](https://github.com/user-attachments/assets/8bcef709-9861-450d-9f8f-5dc5f614d627)

2. Scroll to the code editor and paste the following Python code:

```python
import boto3

def lambda_handler(event, context):
    instance_id = ['EC2 instance id']  # Replace with your EC2 instance ID
    tags = [
        {
            'Key': 'name',  # Replace with your key
            'Value': 'shubham'  # Replace with your value
        },
        {
            'Key': 'place',  # Replace with your key
            'Value': 'new-york'  # Replace with your value
        }
        # Add more tags as needed
    ]

    ec2_client = boto3.client('ec2')
    ec2_client.create_tags(Resources=[instance_id], Tags=tags)

    return {
        'statusCode': 200,
        'body': 'Tags added successfully to EC2 instance'
    }
```

![image](https://github.com/user-attachments/assets/554a157c-e56a-42b0-b122-a0f9e977ad1e)


---

### Step 5: Deploy and Verify
1. Deploy the Lambda function.
![image](https://github.com/user-attachments/assets/460bb3f3-2a83-4f51-8d5f-2aaeaae08b55)
2. Test the function and verify that the tags have been successfully added to your EC2 instance by checking the **Tags** section in the EC2 Dashboard.

---

### Final Thoughts
Tagging AWS resources not only simplifies management but also improves cost optimization and operational efficiency. By automating tagging with Lambda, you ensure consistent and reliable tagging practices across your organization.
