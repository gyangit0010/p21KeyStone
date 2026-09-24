# p21KeyStone
getting start with internship 


user log out after adding the task 
add user work order history 
user can update work order status (incomplete, complete,  in-process )
user can delete or update discription of work 
user can see the detiail of assign worker (technician)



Repository: p21KeyStone
Java: 21 LTS
Maven: 3.9.13
Node.js: 24.14.0
npm: 11.9.0
Git: 2.53.0


Customer reports problem
          ↓
Work Order created
          ↓
Dispatcher assigns technician
          ↓
Technician receives job
          ↓
Technician starts work
          ↓
Technician logs parts/time
          ↓
Technician completes work
          ↓
Manager reviews
          ↓
Manager closes work order
          ↓
Everything remains in history





1. Dispatcher

The dispatcher manages incoming jobs.

They can:

Create customers
Create sites
Create work orders
Assign technicians
Reassign technicians
View the work-order board
Track jobs

Example:

New Job
   ↓
Dispatcher
   ↓
Assign Technician
   ↓
Technician receives job
2. Technician

The technician works in the field.

They should see only their assigned jobs.

For example:

My Jobs

WO-1001
AC not cooling
HIGH
ABC Office
----------------
[Start Job]

WO-1008
Water leakage
MEDIUM
XYZ Building
----------------
[Start Job]

They can:

Start a job
Put it on hold
Resume it
Complete it
Log parts used
Log time spent
Upload photos

The brief specifically requires the technician interface to be responsive and usable on a phone.

3. Manager / Admin

The manager gets the bigger picture.

They can:

Manage users
Manage parts
View all work
Close completed jobs
View reports
Monitor SLA performance
See overdue work
Monitor technicians

For example:

Manager Dashboard

Open Jobs          24
In Progress        12
Overdue             4
Completed          38

SLA Compliance     91%

The goal is that the manager can understand the operational situation without calling everyone individually.

4. Customer

Customers get their own portal.

They can:

Raise a service request
Select their site
See their work orders
Track status
View history

But they must not be able to see another customer's information.

This is one of the important security requirements in the project.

6. The Work Order has a fixed lifecycle

This is probably the most important technical/business concept you need to understand for your internship.

A job cannot randomly jump from one status to another.

The Work Order has a fixed lifecycle

This is probably the most important technical/business concept you need to understand for your internship.

A job cannot randomly jump from one status to another.


The lifecycle is:

NEW
 ↓
ASSIGNED
 ↓
IN_PROGRESS
 ↓
ON_HOLD
 ↓
IN_PROGRESS
 ↓
COMPLETED
 ↓
CLOSED

There is also:

NEW / ASSIGNED → CANCELLED

For example:

A technician cannot directly do:

NEW → CLOSED

The backend must reject it.

The project specifically says these rules must be enforced in the service layer, not merely in React. Every valid status transition must also create a status-history record.

7. Why is this important?

Suppose a hacker/user sends this directly to your API:

POST /api/work-orders/15/status

with:

{
  "status": "CLOSED"
}

The React application shouldn't be the thing stopping them.

The Spring Boot backend must check:

Who is the user?
        ↓
What role do they have?
        ↓
Is this their work order?
        ↓
Is this transition allowed?
        ↓
Is the user allowed to perform it?
        ↓
If YES → update
If NO → reject

This is why the project puts significant emphasis on Spring Security, JWT, server-side authorization, and lifecycle enforcement.