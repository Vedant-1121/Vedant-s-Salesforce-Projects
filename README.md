# Enterprise Leave Management System on Salesforce

## Overview
This repository outlines the functional blueprint for an **Enterprise Leave Management System** built on Salesforce. The system supports end-to-end leave management, including submission, validation, approvals, balance updates, notifications, and reporting.

### High-Level Flow
1. Employee applies for leave.
2. System validates:
   - Leave balance
   - Date overlap
   - Weekends / holidays
3. Manager approves or rejects.
4. Leave balance updates automatically.
5. Notifications sent (Email + Chatter).
6. Leave visible on calendar & reports.

## Step 1: Data Model (Foundation)
### Custom Objects
#### 1. `Employee__c`
Represents an employee profile.
- **Name**
- **Email__c**
- **Manager__c** (Lookup → User)
- **Total_Leave_Balance__c** (Number)
- **Department__c** (Picklist)

#### 2. `Leave_Request__c`
Core transactional object for leave requests.
- **Employee__c** (Lookup → Employee__c)
- **Leave_Type__c** (Picklist: Casual, Sick, Earned)
- **Start_Date__c** (Date)
- **End_Date__c** (Date)
- **Number_of_Days__c** (Formula)
- **Status__c** (Picklist: Draft, Submitted, Approved, Rejected)
- **Reason__c** (Long Text)
- **Approver__c** (Lookup → User)

#### 3. `Holiday__c` (Optional but powerful)
- **Holiday_Date__c**
- **Description__c**

## Step 2: Automation Strategy
| Requirement             | Tool                    | Why                         |
| ----------------------- | ----------------------- | --------------------------- |
| Leave submission        | Flow                    | Declarative, easy to modify |
| Manager approval        | Approval Process / Flow | Native & trackable          |
| Leave balance deduction | Apex                    | Complex logic + bulk safety |
| Overlapping leave check | Apex                    | Needs date comparison       |
| Notifications           | Flow                    | Low code, flexible          |

## Step 3: Apex Logic (Business-Critical)
Apex will handle:
- Overlapping leave prevention
- Insufficient balance checks
- Invalid date ranges
- Bulk-safe updates
- Transaction rollback on error

**Pattern:** Trigger → Handler → Service Class

## Step 4: LWC Components (UI Value)
- **leaveRequestForm** – Apply leave
- **leaveCalendar** – Visual leave view
- **leaveDashboard** – Employee summary

## Step 5: Security & Access
- Role hierarchy (Employee → Manager)
- Sharing rules (Managers see team leaves)
- Field-level security
- Custom permission for HR override

## Step 6: Reports & Dashboards
- Leaves by department
- Approval turnaround time
- Employee leave balance report
