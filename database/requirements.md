## Entities and Attributes

User
user_id: Primary Key, UUID, Indexed
first_name: VARCHAR, NOT NULL
last_name: VARCHAR, NOT NULL
email: VARCHAR, UNIQUE, NOT NULL
password_hash: VARCHAR, NOT NULL
phone_number: VARCHAR, NULL
role: ENUM (guest, host, admin), NOT NULL
created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Property
property_id: Primary Key, UUID, Indexed
host_id: Foreign Key, references User(user_id)
name: VARCHAR, NOT NULL
description: TEXT, NOT NULL
location: VARCHAR, NOT NULL
pricepernight: DECIMAL, NOT NULL
created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
updated_at: TIMESTAMP, ON UPDATE CURRENT_TIMESTAMP

Booking
booking_id: Primary Key, UUID, Indexed
property_id: Foreign Key, references Property(property_id)
user_id: Foreign Key, references User(user_id)
start_date: DATE, NOT NULL
end_date: DATE, NOT NULL
total_price: DECIMAL, NOT NULL
status: ENUM (pending, confirmed, canceled), NOT NULL
created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Payment
payment_id: Primary Key, UUID, Indexed
booking_id: Foreign Key, references Booking(booking_id)
amount: DECIMAL, NOT NULL
payment_date: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP
payment_method: ENUM (credit_card, paypal, stripe), NOT NULL

Review
review_id: Primary Key, UUID, Indexed
property_id: Foreign Key, references Property(property_id)
user_id: Foreign Key, references User(user_id)
rating: INTEGER, CHECK: rating >= 1 AND rating <= 5, NOT NULL
comment: TEXT, NOT NULL
created_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

Message
message_id: Primary Key, UUID, Indexed
sender_id: Foreign Key, references User(user_id)
recipient_id: Foreign Key, references User(user_id)
message_body: TEXT, NOT NULL
sent_at: TIMESTAMP, DEFAULT CURRENT_TIMESTAMP

## Constraints

User Table
Unique constraint on email.
Non-null constraints on required fields.

Property Table
Foreign key constraint on host_id.
Non-null constraints on essential attributes.

Booking Table
Foreign key constraints on property_id and user_id.
status must be one of pending, confirmed, or canceled.

Payment Table
Foreign key constraint on booking_id, ensuring payment is linked to valid bookings.

Review Table
Constraints on rating values (1-5).
Foreign key constraints on property_id and user_id.

Message Table
Foreign key constraints on sender_id and recipient_id.

Indexing
Primary Keys: Indexed automatically.
Additional Indexes:
email in the User table.
property_id in the Property and Booking tables.
booking_id in the Booking and Payment tables.

## Relationship

Booking.user_id → User.id
Booking.property_id → Property.id
Payment.booking_id → Booking.id
Property.user_id → User.id (host)
Property.location_id → Location.id
Review.user_id → User.id
Review.property_id → Property.id

## Entities and Attributes 
USER
- user_id (PK)
- name
- email

PROPERTY
- property_id (PK)
- title
- user_id (FK → USER.user_id)
- location_id (FK → LOCATION.location_id)

LOCATION
- location_id (PK)
- name

BOOKING
- booking_id (PK)
- check_in_date
- check_out_date
- user_id (FK → USER.user_id)
- property_id (FK → PROPERTY.property_id)

PAYMENT
- payment_id (PK)
- amount
- booking_id (FK → BOOKING.booking_id)

REVIEW
- review_id (PK)
- user_id (FK → USER.user_id)
- property_id (FK → PROPERTY.property_id)
- rating
- comment


<img width="1024" height="1024" alt="Copilot_20251027_121814" src="https://github.com/user-attachments/assets/4993c328-6347-4fa1-a03d-bef725b6e36c" />

## 🧠 Database Normalization: Airbnb Clone

This document outlines the normalization process applied to the Airbnb database design to ensure it adheres to **Third Normal Form (3NF)**.

---

## 🔹 First Normal Form (1NF)

**Goal**: Eliminate repeating groups and ensure atomicity.

**Steps Taken**:
- All attributes contain atomic values (e.g., `email`, `price_per_night`, `start_date`)
- No multi-valued fields or arrays
- Each table has a defined primary key

✅ Achieved: All tables meet 1NF requirements.

---

## 🔹 Second Normal Form (2NF)

**Goal**: Remove partial dependencies (i.e., attributes depending on part of a composite key).

**Steps Taken**:
- All tables use single-column primary keys (e.g., `user_id`, `property_id`)
- Non-key attributes fully depend on the entire primary key
- Composite keys avoided where unnecessary

✅ Achieved: No partial dependencies exist.

---

## 🔹 Third Normal Form (3NF)

**Goal**: Remove transitive dependencies (i.e., non-key attributes depending on other non-key attributes).

**Steps Taken**:
- Attributes like `host_id` in `Property` reference `User(user_id)` — no duplication of host details
- `Payment` references `Booking`, which already links to `User` and `Property` — no redundant user/property info in `Payment`
- `Review` links to both `User` and `Property` without duplicating booking details

✅ Achieved: All non-key attributes depend only on the primary key.

---

## 🧾 Outcome

- ✅ Minimal redundancy
- ✅ High data integrity
- ✅ Efficient querying and scalability
- ✅ Schema is normalized to **3NF**

---

## 📌 Notes

- Foreign keys are used to maintain referential integrity
- ENUMs used for controlled values (e.g., `role`, `status`, `payment_method`)
- Indexes applied to frequently queried fields (e.g., `email`, `property_id`)

