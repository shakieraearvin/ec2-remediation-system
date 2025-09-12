# ec2-remediation-system
# EC2 Monitoring and Remediation System – ServiceNow Implementation

## System Overview
This project delivers a **semi-automated incident response system** for Netflix’s DevOps team to quickly detect and remediate failing AWS EC2 instances.  
The solution integrates **AWS Integration Server**, **ServiceNow custom tables**, **Flow Designer workflows**, **AI-powered knowledge retrieval**, and **Slack notifications**.  

When an EC2 instance goes down:
- The **AWS Integration Server** updates ServiceNow every minute with instance health status.  
- If an instance is marked **OFF**, the **Flow Designer workflow** automatically:  
  - Creates an **incident record**.  
  - Runs **AI Search** to retrieve remediation guidance from the Knowledge Base.  
  - Sends a **Slack notification** to the DevOps channel with remediation steps.  
- Engineers can take immediate action using a **one-click remediation interface** in ServiceNow, which calls the **AWS Integration Server API** to restart the instance.  
- All remediation attempts are logged in the **Remediation Log table** for auditing.  

---

## Implementation Steps
Key implementation decisions and integrations include:

1. **Scoped Application Setup**
   - Application name: `EC2 Monitoring and Remediation`  
   - Auto-generated scope: `x_snc_ec2_monito_0`  

2. **Custom Tables**
   - **EC2 Instance**: Stores instance ID, name, and status (`ON`/`OFF`).  
   - **Remediation Log**: Tracks all remediation attempts with payloads, responses, and results.  

3. **AWS Integration**
   - **Connection & Credential Alias**: `AWS Integration Server C C Alias`  
   - **Connection**: `AWS Integration Server Connection`  
   - **Credentials**: Basic Authentication  

4. **UI Action and Script Include**
   - **UI Action**: `Trigger EC2 Remediation` (adds a button to the EC2 Instance form).  
   - **Script Include**: `EC2RemediationHelper` (calls AWS API via Integration Server).  

5. **Flow Designer Workflow**
   - Trigger: EC2 Instance status changes to **OFF**.  
   - Actions:  
     - Create incident.  
     - Run AI Search to retrieve KB article.  
     - Send Slack notification with remediation guidance.  

6. **Knowledge Base Content**
   - Article: *“Run the UI Action ‘Trigger EC2 Remediation’ on the associated record.”*  
   - Keywords: EC2, AWS, server, restart, reboot, instance, cloud.  

---

## Architecture Diagram
Below is the system workflow diagram (see `Diagram.png` in repo):  

![Architecture Diagram](Diagram.png)

---

## Optimization
To ensure reliability and efficiency, the following improvements were applied:  
- **AI Search Logging** enabled for transparency of KB retrieval.  
- **Audit trail** implemented through System Logs, HTTP Logs, and the Remediation Log table.  
- **Force Save** used in Flow Designer to capture all workflow components in the update set.  
- **Slack webhook security** ensured by removing the token before pushing to GitHub.  

---

## DevOps Usage
For Netflix DevOps engineers:  

1. **Monitor EC2 Status**
   - The EC2 Instance table updates every minute with the latest status.  

2. **Receive Notifications**
   - If an instance goes **OFF**, you’ll receive a **Slack notification** with remediation guidance and an **incident will be created** in ServiceNow.  

3. **Perform Remediation**
   - Open the affected EC2 Instance record in ServiceNow.  
   - Click **Trigger EC2 Remediation**.  
   - The system calls the AWS Integration Server API to restart the instance.  

4. **Review Logs**
   - Remediation attempts are tracked in the **Remediation Log**.  
   - System/HTTP logs provide additional traceability.  

5. **Knowledge Base Support**
   - Use AI Search to find recommended remediation steps during incidents.  

---

## Screenshots
*(Insert screenshots here of your ServiceNow tables, Flow Designer workflow, Slack notifications, incident records, and remediation logs.)*
