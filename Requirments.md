## 📊 Entity Relationship Diagram (ERD)

-User Management
User Registration:
Allow users to sign up as guests or hosts.
Use secure authentication methods like JWT (JSON Web Tokens).
User Login and Authentication:
Implement login via email and password.
Include OAuth options (e.g., Google, Facebook).
Profile Management:
Enable users to update their profiles, including profile photos, contact info, and preferences.

-Property Listings Management
Add Listings:
Hosts can create property listings by providing details such as title, description, location, price, amenities, and availability.
Edit/Delete Listings:
Hosts can update or remove their property listings.
3. Search and Filtering
Implement search functionality to allow users to find properties by:
Location
Price range
Number of guests
Amenities (e.g., Wi-Fi, pool, pet-friendly)
Include pagination for large datasets.

-Booking Management
Booking Creation:
Guests can book a property for specified dates.
Prevent double bookings using date validation.
Booking Cancellation:
Allow guests or hosts to cancel bookings based on the cancellation policy.
Booking Status:
Track booking statuses such as pending, confirmed, canceled, or completed.

-Payment Integration
Implement secure payment gateways (e.g., Stripe, PayPal) to handle:
Upfront payments by guests.
Automatic payouts to hosts after a booking is completed.
Include support for multiple currencies.

-Reviews and Ratings
Guests can leave reviews and ratings for properties.
Hosts can respond to reviews.
Ensure reviews are linked to specific bookings to prevent abuse.

-Notifications System
Implement email and in-app notifications for:
Booking confirmations
Cancellations
Payment updates

-Admin Dashboard
Create an admin interface for monitoring and managing:
Users
Listings
Bookings
Payments

##🛠️ Technical Requirements

-Database Management
Use a relational database such as PostgreSQL or MySQL.
Required tables:
Users (guests and hosts)
Properties
Bookings
Reviews
Payments

-API Development
Use RESTful APIs to expose backend functionalities to the frontend.
Include proper HTTP methods and status codes for:
GET (retrieve data)
POST (create data)
PUT/PATCH (update data)
DELETE (remove data)
Use GraphQL for complex data fetching scenarios (optional but recommended).

-Authentication and Authorization
Use JWT for secure user sessions.
Implement role-based access control (RBAC) to differentiate permissions between:
Guests
Hosts
Admins

-File Storage (Scenario based)
Store property images and user profile photos in cloud storage solutions such as AWS S3 or Cloudinary. For implementation, we will use file storage

-Third-Party Services
Use email services like SendGrid or Mailgun for email notifications.
6. Error Handling and Logging
Implement global error handling for APIs.

##🚀 Non-Functional Requirements

-Scalability
Use a modular architecture to ensure the app scales easily as traffic increases.
Enable horizontal scaling using load balancers.

-Security
Secure sensitive data (e.g., passwords, payment information) using encryption.
Implement firewalls and rate limiting to prevent malicious activities.

-Performance Optimization
Use caching tools like Redis to improve response times for frequently accessed data (e.g., search results).
Optimize database queries to reduce server load.

-Testing
Implement unit and integration tests using frameworks like pytest .
Include automated API testing to ensure endpoints function as expected.


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

Constraints
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
booking_id in the Booking and P

## Objective
Design a visual representation of the Airbnb database system using an ERD.

## Entities
- User
- Property
- Booking
- Review
- Payment

## Relationships
- A User can book multiple Properties.
- A Booking links a User to a Property.
- A Review is written by a User for a Property.
- A Payment is tied to a Booking.

## Tool
*****Use [Draw.io](https://draw.io) or similar to create your ERD and upload the image or link here.*****

## 

# 🧠 Database Normalization

## Objective
Apply normalization principles to ensure the database is in Third Normal Form (3NF).

## Steps Taken
1. **1NF**: Removed repeating groups and ensured atomicity.
2. **2NF**: Eliminated partial dependencies by separating composite keys.
3. **3NF**: Removed transitive dependencies to isolate non-key attributes.

## Outcome
The final schema ensures:
- Minimal redundancy
- High data integrity
- Efficient querying

-- Airbnb Clone Schema Definition

CREATE TABLE User (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    role ENUM('guest', 'host')
);

CREATE TABLE Property (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    location VARCHAR(255),
    price_per_night DECIMAL(10,2),
    host_id INT,
    FOREIGN KEY (host_id) REFERENCES User(id)
);

CREATE TABLE Booking (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    property_id INT,
    start_date DATE,
    end_date DATE,
    FOREIGN KEY (user_id) REFERENCES User(id),
    FOREIGN KEY (property_id) REFERENCES Property(id)
);

CREATE TABLE Review (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    property_id INT,
    rating INT,
    comment TEXT,
    FOREIGN KEY (user_id) REFERENCES User(id),
    FOREIGN KEY (property_id) REFERENCES Property(id)
);

CREATE TABLE Payment (
    id INT PRIMARY KEY AUTO_INCREMENT,
    booking_id INT,
    amount DECIMAL(10,2),
    status ENUM('paid', 'pending'),
    method VARCHAR(50),
    FOREIGN KEY (booking_id) REFERENCES Booking(id)
);

# 🧱 Design Database Schema

This folder contains the SQL schema for the Airbnb Clone project.

## Files
- `schema.sql`: Defines tables, keys, and constraints.

## Notes
- All foreign keys are properly linked.
- Indexes can be added for performance optimization.

-- Sample Data for Airbnb Clone

INSERT INTO User (name, email, password, role)
VALUES ('Alice', 'alice@example.com', 'hashed_pw', 'guest'),
       ('Bob', 'bob@example.com', 'hashed_pw', 'host');

INSERT INTO Property (title, location, price_per_night, host_id)
VALUES ('Cozy Cottage', 'Cape Town', 1200.00, 2),
       ('Urban Loft', 'Johannesburg', 950.00, 2);

INSERT INTO Booking (user_id, property_id, start_date, end_date)
VALUES (1, 1, '2025-11-01', '2025-11-05'),
       (1, 2, '2025-12-10', '2025-12-15');

INSERT INTO Review (user_id, property_id, rating, comment)
VALUES (1, 1, 5, 'Amazing stay!'),
       (1, 2, 4, 'Great location.');

INSERT INTO Payment (booking_id, amount, status, method)
VALUES (1, 4800.00, 'paid', 'card'),
       (2, 4750.00, 'paid', 'paypal');

   -- Airbnb Clone Schema Definition

CREATE TABLE User (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    role ENUM('guest', 'host')
);

CREATE TABLE Property (
    id INT PRIMARY KEY AUTO_INCREMENT,
    title VARCHAR(255),
    location VARCHAR(255),
    price_per_night DECIMAL(10,2),
    host_id INT,
    FOREIGN KEY (host_id) REFERENCES User(id)
);

CREATE TABLE Booking (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    property_id INT,
    start_date DATE,
    end_date DATE,
    FOREIGN KEY (user_id) REFERENCES User(id),
    FOREIGN KEY (property_id) REFERENCES Property(id)
);

CREATE TABLE Review (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    property_id INT,
    rating INT,
    comment TEXT,
    FOREIGN KEY (user_id) REFERENCES User(id),
    FOREIGN KEY (property_id) REFERENCES Property(id)
);

CREATE TABLE Payment (
    id INT PRIMARY KEY AUTO_INCREMENT,
    booking_id INT,
    amount DECIMAL(10,2),
    status ENUM('paid', 'pending'),
    method VARCHAR(50),
    FOREIGN KEY (booking_id) REFERENCES Booking(id)
);

--## Sample Data for Airbnb Clone

INSERT INTO User (name, email, password, role)
VALUES ('Alice', 'alice@example.com', 'hashed_pw', 'guest'),
       ('Bob', 'bob@example.com', 'hashed_pw', 'host');

INSERT INTO Property (title, location, price_per_night, host_id)
VALUES ('Cozy Cottage', 'Cape Town', 1200.00, 2),
       ('Urban Loft', 'Johannesburg', 950.00, 2);

INSERT INTO Booking (user_id, property_id, start_date, end_date)
VALUES (1, 1, '2025-11-01', '2025-11-05'),
       (1, 2, '2025-12-10', '2025-12-15');

INSERT INTO Review (user_id, property_id, rating, comment)
VALUES (1, 1, 5, 'Amazing stay!'),
       (1, 2, 4, 'Great location.');

INSERT INTO Payment (booking_id, amount, status, method)
VALUES (1, 4800.00, 'paid', 'card'),
       (2, 4750.00, 'paid', 'paypal');


# 💾 Database Seeding

This folder contains SQL scripts to populate the Airbnb Clone database with sample data.

## Files
- `seed.sql`: Inserts realistic data for testing and demonstration.

## Notes
- Data reflects real-world usage scenarios.
- Useful for testing queries and relationships.
