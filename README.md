# Streaming IT Procurement: Automating Standard Laptop Orders for Flow Designers

## Problem Statement
Manual procurement and fulfillment processes for standard hardware requests lead to delays, errors in task assignment, and unnecessary manual intervention. This project streamlines the IT hardware procurement process using ServiceNow Flow Designer to automate task creation, assignment, and status updates for standard laptop orders.

## Objective
The objective of this project is to demonstrate how ServiceNow Flow Designer, Service Catalog, and Catalog Tasks integrate to achieve seamless procurement automation.

The implementation demonstrates how to:
- Automatically trigger workflows upon catalog item submission.
- Create and assign Catalog Tasks to the Hardware team.
- Ensure automated approval routing before task assignment.
- Maintain data integrity and visibility across procurement stages.
- Automate order status updates based on task completion.

## Skills
- Flow Designer
- Service Catalog Management
- Catalog Tasks & Approvals
- IT Service Management (ITSM)
- Automated Workflow Routing

## Implementation

### Task 1: Flow Creation & Trigger Configuration
- *Flow Name:* Standard Laptop Order Flow
- *Trigger:* Service Catalog (Catalog Item Submitted)
- *Active:* True
- *Description:* Triggers when a user places an order for a Standard Laptop catalog item.

### Task 2: Approval Routing
- *Action:* Ask For Approval
- *Item:* Catalog Request Item (RITM)
- *Rules:* Approve if Manager Approval is granted; Reject if Manager denies.

### Task 3: Catalog Task Creation & Assignment
- *Action:* Create Catalog Task
- *Table:* Catalog Task (sc_task)
- *Assignment Group:* Hardware Team
- *Priority:* Medium
- *Short Description:* Setup and configure standard laptop for user.

### Task 4: State & Status Updates
- *Action:* Update Record
- *Logic:* Automatically update RITM state to "Complete" upon task fulfillment.

## Testing

### Test 1: Successful Order & Task Creation
1. Open Service Catalog -> Select *Standard Laptop*.
2. Submit the Request.
3. Verify that the Flow Flow Designer triggers successfully.
4. Check that a Catalog Task is automatically created and assigned to the *Hardware Team*.

### Test 2: Approval Workflow Verification
1. Place a new Standard Laptop order.
2. Verify RITM status is set to *Waiting for Approval*.
3. Approve the request as the Manager.
4. Verify that the Catalog Task is generated only after approval.

### Test 3: Automated Fulfillment Closure
1. Open the created Catalog Task.
2. Mark the Task State as *Closed Complete*.
3. Verify that the parent RITM automatically updates to *Closed Complete*.

## Expected Results
- Automated workflow triggers seamlessly without manual intervention upon catalog submission.
- Approvals are routed correctly before task generation.
- Tasks are accurately created and assigned to the Hardware Team.
- Order state changes synchronously reflect task status.

## Conclusion
The *Streaming IT Procurement* project successfully demonstrates end-to-end automation of standard laptop procurement using ServiceNow Flow Designer. By replacing manual handoffs with automated task assignment and approval routing, this solution improves processing efficiency, reduces fulfillment errors, and ensures full operational transparency.
