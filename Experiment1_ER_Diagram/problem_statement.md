# ER Diagram Workshop – Submission Template

## Objective
To understand and apply ER modeling concepts by creating ER diagrams for real-world applications.

## Purpose
Gain hands-on experience in designing ER diagrams that represent database structure including entities, relationships, attributes, and constraints.

---

# Scenario A: City Fitness Club Management

**Business Context:**  
FlexiFit Gym wants a database to manage its members, trainers, and fitness programs.

**Requirements:**  
- Members register with name, membership type, and start date.  
- Each member can join multiple programs (Yoga, Zumba, Weight Training).  
- Trainers assigned to programs; a program may have multiple trainers.  
- Members may book personal training sessions with trainers.  
- Attendance recorded for each session.  
- Payments tracked for memberships and sessions.

### ER Diagram:
<img width="781" height="763" alt="image" src="https://github.com/user-attachments/assets/ce1db649-1237-436b-87a6-1f6dba9bf06c" />

### Entities and Attributes

| Entity            | Attributes (PK, FK) | Notes |
|-------------------|----------------------|-------|
| Member            | member_id (PK), first_name, last_name, email, phone_number, membership_start_date, expected_end_date | Represents gym members who register. |
| Membership_Type   | member_id (PK, FK), type_name, duration_months, membership_start_date, schedule_details | Defines membership categories for members. |
| Program           | program_id (PK), program_name, description, schedule_details | Fitness programs offered (Yoga, Zumba, etc.). |
| Trainer           | trainer_id (PK), first_name, specialization, contact_number | Trainers responsible for programs or sessions. |
| Member_Program    | attendance_id (FK), status, enrollment_date, member_id (FK), program_id (FK) | Resolves M:N between Member and Program. |
| Program_Trainer   | trainer_id (FK), program_id (FK), attendance_date, session_id (FK), member_id (FK) | Resolves M:N between Trainer and Program. |
| Personal_Program  | trainer_id (FK), first_name, session_time, duration_minutes | Tracks personal training programs. |
| Personal_Training_Session | session_id (PK), payment_id (FK), date, amount, payment_type, member_id (FK) | Represents one-to-one personal training sessions. |
| Attendance        | attendance_id (PK), status, session_id (FK), member_id (FK), trainer_id (FK) | Tracks attendance for members in sessions. |
| Payment           | payment_id (PK), amount, payment_date, payment_type, member_id (FK), attendance_id (FK), session_id (FK) | Records payments made for memberships or sessions. |

---

### Relationships and Constraints

| Relationship   | Cardinality | Participation | Notes |
|----------------|-------------|---------------|-------|
| has_type       | 1:M         | Membership_Type (Total), Member (Total) | Each member has exactly one membership type. |
| enrolls_in     | M:N         | Member (Total), Program (Partial) | Members can enroll in many programs; programs have many members. |
| has_trainer    | M:N         | Program (Total), Trainer (Partial) | A program can have multiple trainers; a trainer can handle multiple programs. |
| registers      | M:N         | Member (Partial), Session (Partial) | A member may register for many sessions; sessions may have many members. |
| includes       | 1:M         | Program (Total), Session (Partial) | Programs include many sessions. |
| has_attendance | 1:M         | Session (Total), Attendance (Total) | Attendance is tracked for each session. |
| makes          | 1:M         | Member (Total), Payment (Total) | Each member makes multiple payments. |

---

### Assumptions
- A member must have exactly one membership type.  
- A program can exist without members but must have at least one trainer.  
- Payments are always tied to members and sessions/programs.  
- Attendance is recorded per session for both members and trainers.  
- Personal training sessions are treated separately from regular programs.

---

# Scenario B: City Library Event & Book Lending System

**Business Context:**  
The Central Library wants to manage book lending and cultural events.

**Requirements:**  
- Members borrow books, with loan and return dates tracked.  
- Each book has title, author, and category.  
- Library organizes events; members can register.  
- Each event has one or more speakers/authors.  
- Rooms are booked for events and study.  
- Overdue fines apply for late returns.

### ER Diagram:
<img width="743" height="647" alt="image" src="https://github.com/user-attachments/assets/2345ea16-6193-4927-a96e-726634add70e" />

### Entities and Attributes

| Entity       | Attributes (PK, FK) | Notes |
|--------------|----------------------|-------|
| Member       | member_id (PK), first_name, email, phone_number | Represents library members. |
| Book         | book_id (PK), title, ISBN, author, publisher, publication_year, category, due_date, is_overdue (boolean) | Books available in the library. |
| Loan         | loan_id (PK), member_id (FK), book_id (FK), loan_date, return_date | Represents borrowing transactions. |
| Room         | room_id (PK), room_type, capacity | Rooms available for events or study. |
| Event        | event_id (PK), event_name, start_date, start_time, end_time, description, return_date, room_id (FK), is_overdue (boolean) | Cultural or study events held in the library. |
| Speaker      | speaker_id (PK), first_name, last_name, contact_info, bio | Speakers/authors invited for events. |
| Member_Event | member_event_id (PK), member_id (FK), event_id (FK), registration_date | Resolves M:N between Member and Event. |
| Event_Speaker| event_id (FK), speaker_id (FK) | Resolves M:N between Event and Speaker. |
| Overdue_Fine | fine_id (PK), loan_id (FK), amount, date_assessed, is_paid | Fine details for late returns. |

---

### Relationships and Constraints

| Relationship       | Cardinality | Participation | Notes |
|--------------------|-------------|---------------|-------|
| borrows            | 1:N         | Member (Total), Loan (Total) | A member can borrow many books, each loan is tied to one book. |
| loan_of            | 1:N         | Book (Total), Loan (Total) | A book can be loaned many times, but each loan is for one book. |
| registers_for      | M:N         | Member (Partial), Event (Partial) | Members can register for many events; events can have many members. |
| booked_in          | 1:M         | Room (Total), Event (Total) | Each event takes place in one room, a room can host multiple events. |
| event_speaker      | M:N         | Event (Total), Speaker (Partial) | An event can have multiple speakers, and a speaker can participate in many events. |
| fine_applies       | 1:N         | Loan (Total), Overdue_Fine (Partial) | Fines are generated for overdue loans. |

---

### Assumptions
- Each book must have a unique book_id and may be borrowed multiple times.  
- Members can attend multiple events, and events can have many members.  
- Rooms can be used for multiple purposes (study or events).  
- A fine is only generated when a book is overdue.  
- Each event can have multiple speakers, but must be linked to at least one room.
---

# Scenario C: Restaurant Table Reservation & Ordering

**Business Context:**  
A popular restaurant wants to manage reservations, orders, and billing.

**Requirements:**  
- Customers can reserve tables or walk in.  
- Each reservation includes date, time, and number of guests.  
- Customers place food orders linked to reservations.  
- Each order contains multiple dishes; dishes belong to categories (starter, main, dessert).  
- Bills generated per reservation, including food and service charges.  
- Waiters assigned to serve reservations.

### ER Diagram:
<img width="768" height="756" alt="image" src="https://github.com/user-attachments/assets/44994a1b-d267-445a-b61c-9bb451c1b586" />


### Entities and Attributes

| Entity            | Attributes (PK, FK) | Notes |
|-------------------|----------------------|-------|
| Customer          | customer_id (PK), first_name, email, phone_number | Represents customers making reservations. |
| Reservation       | reservation_id (PK), reservation_datetime, num_guests, status, customer_id (FK) | Stores booking details for customers. Status = confirmed, seated, completed. |
| Restaurant_Table  | table_number (PK), capacity, location, reservation_id (FK) | Tables available in the restaurant, linked to reservations. |
| Order_Item        | order_id (PK), order_datetime, status, reservation_id (FK) | Stores orders made during a reservation. Status = preparing, delivered. |
| Category          | category_id (PK), category_name | Categories of dishes (e.g., Starter, Main Course, Dessert). |
| Dish              | dish_id (PK), dish_name, price, category_id (FK) | Menu items offered by the restaurant. |
| Order_Dish        | order_id (FK), dish_id (FK) | Resolves M:N between Order_Item and Dish. |
| Bill              | bill_id (PK), total_amount, service_charge, is_paid (boolean), reservation_id (FK) | Billing details generated per reservation. |
| Waiter            | waiter_id (PK), first_name, last_name, employee_number | Waiters serving reservations. |
| Waiter_Reservation| waiter_id (FK), reservation_id (FK), service_charge, tax, is_paid (boolean) | Resolves M:N between Waiter and Reservation. |

---

### Relationships and Constraints

| Relationship       | Cardinality | Participation | Notes |
|--------------------|-------------|---------------|-------|
| books              | 1:N         | Customer (Total), Reservation (Total) | A customer can make many reservations, each reservation belongs to one customer. |
| assigned_to        | 1:N         | Reservation (Total), Restaurant_Table (Total) | Each reservation is assigned one table; a table can serve many reservations (over time). |
| places             | 1:N         | Reservation (Total), Order_Item (Total) | A reservation can generate many orders; each order belongs to one reservation. |
| contains           | M:N         | Order_Item (Partial), Dish (Total) | An order can contain multiple dishes; a dish can appear in multiple orders. |
| categorized_as     | 1:N         | Category (Total), Dish (Total) | Each dish belongs to one category; each category can have many dishes. |
| generates          | 1:1         | Reservation (Total), Bill (Partial) | Each reservation generates one bill; not all reservations may be billed (e.g., cancelled). |
| served_by          | M:N         | Waiter (Total), Reservation (Partial) | Multiple waiters can serve one reservation; one waiter can serve multiple reservations. |

---

### Assumptions

- Each customer must have a unique customer_id.  
- A reservation must be linked to exactly one customer, but can involve multiple guests.  
- Tables are reused; one reservation = one table assignment, but tables can serve different reservations at different times.  
- Orders are always tied to an active reservation.  
- Bills are only generated once a reservation is completed.  
- Waiters may share service responsibilities across reservations.
---
