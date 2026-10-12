# DATABASE NORMALISATION OF THE HOTEL RESERVATION SYSTEM

## 1. First Normal Form (1NF)

### 1.1 Introduction

First Normal Form (1NF) is the initial stage of database normalisation. A table satisfies 1NF when every attribute contains a single, atomic value, each record can be uniquely identified, and there are no repeating groups.

In the Hotel Reservation System, the initial database design combines guest information, room details, booking information, and payment details in a single table. This approach can result in data redundancy and difficulties when managing multiple payments for a booking.

### 1.2 Initial Unnormalised Table

Consider the following table representing hotel reservations.

| Guest_ID | Guest_Name | Room_No | Room_Type | Booking_ID | Check_In | Check_Out | Payment_Details |
|---|---|---|---|---|---|---|---|
| G001 | John Okafor | R101 | Standard | B001 | 10/10/2026 | 12/10/2026 | P001: ₦50,000; P002: ₦50,000 |
| G002 | Mary James | R102 | Deluxe | B002 | 11/10/2026 | 13/10/2026 | P003: ₦160,000 |

The Payment_Details column contains multiple payment records in a single cell. This violates the atomic-value requirement of 1NF.

### 1.3 Transformation into First Normal Form

To convert the table into 1NF, multiple payment details are separated into individual rows so that each field contains a single value.

**Table 1: Hotel Reservation Table in 1NF**

| Guest_ID | Guest_Name | Room_No | Room_Type | Booking_ID | Check_In | Check_Out | Payment_ID | Payment_Amount |
|---|---|---|---|---|---|---|---|---:|
| G001 | John Okafor | R101 | Standard | B001 | 10/10/2026 | 12/10/2026 | P001 | ₦50,000 |
| G001 | John Okafor | R101 | Standard | B001 | 10/10/2026 | 12/10/2026 | P002 | ₦50,000 |
| G002 | Mary James | R102 | Deluxe | B002 | 11/10/2026 | 13/10/2026 | P003 | ₦160,000 |

The records above are illustrative examples.

Each payment now occupies a separate row, and every field contains an individual value. For this illustration, Booking_ID and Payment_ID can form a composite primary key if payment identifiers are unique within their respective bookings.

### 1.4 Problems Remaining After 1NF

Although the table satisfies 1NF, several problems remain.

1. **Data redundancy:** Guest names and room details are repeated for multiple payments associated with the same booking.
2. **Update anomalies:** Changing a guest's name or room information may require updating several records.
3. **Insertion anomalies:** Guest information cannot easily be stored independently of a booking if the table requires booking details.
4. **Deletion anomalies:** Deleting the only booking record for a guest could also remove the only stored copy of that guest's information.

The table also contains partial dependencies. For example, Payment_Amount depends on Payment_ID, while Guest_Name depends on the guest identifier rather than on the entire composite key.

### 1.5 Conclusion

The database satisfies 1NF because repeating groups have been removed and each field contains a single value. However, data redundancy and partial dependencies remain. Further decomposition is required to achieve Second Normal Form.

---

## 2. Second Normal Form (2NF)

### 2.1 Introduction

Second Normal Form (2NF) builds on 1NF. A table satisfies 2NF when it is already in 1NF and every non-key attribute depends on the whole of every candidate key, rather than only part of a composite key.

The 1NF design contains guest, room, booking, and payment information in the same table. These attributes describe different entities and should be separated to eliminate partial dependencies.

### 2.2 Identifying Functional Dependencies

The following functional dependencies apply to the example, assuming the stated identifiers uniquely identify their respective records:

- Guest_ID → Guest_Name
- Room_No → Room_Type
- Booking_ID → Guest_ID, Room_No, Check_In, Check_Out
- Payment_ID → Booking_ID, Payment_Amount

These dependencies indicate that guest details depend on the guest identifier, room details depend on the room identifier, booking details depend on the booking identifier, and payment details depend on the payment identifier.

### 2.3 Transformation into Second Normal Form

To achieve 2NF, the combined table is decomposed into four related tables.

**Table 2.1: Guest**

| Guest_ID (PK) | Guest_Name |
|---|---|
| G001 | John Okafor |
| G002 | Mary James |

This table stores guest information independently of reservations.

**Table 2.2: Room**

| Room_No (PK) | Room_Type |
|---|---|
| R101 | Standard |
| R102 | Deluxe |

This table stores room information independently of bookings.

**Table 2.3: Booking**

| Booking_ID (PK) | Guest_ID (FK) | Room_No (FK) | Check_In | Check_Out |
|---|---|---|---|---|
| B001 | G001 | R101 | 10/10/2026 | 12/10/2026 |
| B002 | G002 | R102 | 11/10/2026 | 13/10/2026 |

This table records reservations and uses foreign keys to reference the associated guest and room.

**Table 2.4: Payment**

| Payment_ID (PK) | Booking_ID (FK) | Payment_Amount |
|---|---|---:|
| P001 | B001 | ₦50,000 |
| P002 | B001 | ₦50,000 |
| P003 | B002 | ₦160,000 |

This table records individual payments and links each payment to its associated booking.

PK means primary key, while FK means foreign key.

### 2.4 Why the Design Satisfies 2NF

The decomposition separates attributes according to the entities they describe. Guest information is stored in Guest, room information in Room, reservation information in Booking, and payment information in Payment.

Each table has a single-column primary key, and its non-key attributes depend on that key. Therefore, the design avoids partial dependencies on a subset of a composite primary key.

The use of foreign keys maintains the relationships between the tables without requiring guest and room details to be repeated in every payment record.

### 2.5 Problems That May Remain

Although the design satisfies 2NF, the database must still be examined for transitive dependencies.

For example, if Guest_Name and Room_Type were stored in the Booking table, the following dependencies could occur:

- Booking_ID → Guest_ID → Guest_Name
- Booking_ID → Room_No → Room_Type

Guest names and room types are determined by their respective identifiers rather than directly by the booking identifier.

Storing these attributes repeatedly in the booking table would introduce unnecessary duplication and possible update anomalies.

### 2.6 Conclusion

The design satisfies 2NF by eliminating partial dependencies and separating guest, room, booking, and payment information. However, the database must also avoid transitive dependencies to achieve Third Normal Form.

---

## 3. Third Normal Form (3NF)

### 3.1 Introduction

Third Normal Form (3NF) builds on 2NF. A table satisfies 3NF when it is in 2NF and its non-key attributes do not have inappropriate transitive dependencies on candidate keys.

In practical terms, each table should store attributes that describe the entity identified by its key, rather than attributes that depend on another non-key attribute.

### 3.2 Identifying Transitive Dependencies

Consider the following poorly designed booking table.

**Table 3.1: Booking Table Before 3NF**

| Booking_ID (PK) | Guest_ID | Guest_Name | Room_No | Room_Type | Check_In | Check_Out |
|---|---|---|---|---|---|---|
| B001 | G001 | John Okafor | R101 | Standard | 10/10/2026 | 12/10/2026 |
| B002 | G002 | Mary James | R102 | Deluxe | 11/10/2026 | 13/10/2026 |

Assuming guest IDs uniquely identify guests and room numbers uniquely identify rooms, the following functional dependencies exist:

- Booking_ID → Guest_ID, Room_No, Check_In, Check_Out
- Guest_ID → Guest_Name
- Room_No → Room_Type

Consequently, the following transitive dependencies exist:

- Booking_ID → Guest_ID → Guest_Name
- Booking_ID → Room_No → Room_Type

This means guest names and room types are determined through other attributes rather than being independent booking attributes.

Keeping them in the booking table would create redundancy and increase the risk of inconsistent information.

### 3.3 Transformation into Third Normal Form

To achieve 3NF, guest and room attributes are stored in their respective tables. The booking table retains only booking-specific attributes and foreign keys.

The resulting logical design contains five related tables.

**Table 3.2: Guest**

| Attribute | Description |
|---|---|
| guest_id (PK) | Unique guest identifier |
| full_name | Guest's full name |
| phone_number | Guest's contact number |
| email | Guest's email address |
| address | Guest's residential address |
| date_registered | Date the guest registered |

Each guest is recorded once.

**Table 3.3: Room**

| Attribute | Description |
|---|---|
| room_id (PK) | Unique room identifier |
| room_number | Hotel room number |
| room_type | Category of room |
| rate_per_night | Standard nightly rate |
| room_status | Current room status |

Each room's information is stored independently of its reservations.

**Table 3.4: Booking**

| Attribute | Description |
|---|---|
| booking_id (PK) | Unique booking identifier |
| guest_id (FK) | References the guest |
| room_id (FK) | References the room |
| booking_date | Date the booking was made |
| check_in_date | Scheduled arrival date |
| check_out_date | Scheduled departure date |
| number_of_guests | Number of guests covered by the booking |
| booking_status | Current booking status |

The table contains reservation-specific information and references the guest and room.

**Table 3.5: Payment**

| Attribute | Description |
|---|---|
| payment_id (PK) | Unique payment identifier |
| booking_id (FK) | References the booking |
| payment_date | Date of payment |
| amount | Payment amount |
| payment_method | Method used to make payment |
| transaction_reference | Payment transaction reference |
| payment_status | Current payment status |

Each payment is recorded independently and linked to its booking.

**Table 3.6: Check-in/Check-out**

| Attribute | Description |
|---|---|
| record_id (PK) | Unique check-in/check-out record identifier |
| booking_id (FK, UNIQUE) | References the associated booking |
| actual_check_in | Actual arrival date and time |
| actual_check_out | Actual departure date and time |
| staff_name | Name of the staff member recorded for the activity |

The unique constraint on booking_id allows at most one check-in/check-out record per booking.

### 3.4 Why the Final Design Satisfies 3NF

The final design separates the database into five tables, each representing a distinct entity or activity.

- The Guest table stores guest-specific information.
- The Room table stores room-specific information.
- The Booking table stores reservation information and references the guest and room.
- The Payment table stores individual payment records associated with bookings.
- The Check-in/Check-out table stores actual arrival and departure information.

Guest names and contact details are no longer repeated in booking records. Similarly, room descriptions and standard rates are stored in the Room table rather than duplicated across reservations.

Primary keys identify records, while foreign keys establish relationships between the tables. This organisation reduces unnecessary duplication and helps prevent update, insertion, and deletion anomalies.

This conclusion assumes that the attributes follow the functional dependencies described above. If a room type determines a standard rate independently of the individual room, the design could be further refined by introducing a separate Room_Type table.

### 3.5 Benefits of the 3NF Design

The final design provides the following benefits:

1. **Reduced data redundancy:** Guest and room information is stored in dedicated tables.
2. **Improved data consistency:** Changes to guest details can be made in one location.
3. **Improved payment management:** Each payment is stored separately and linked to its booking.
4. **Stronger data integrity:** Primary and foreign keys maintain valid relationships.
5. **Reduced anomalies:** The design minimises problems associated with inserting, updating, or deleting records.
6. **Improved maintainability:** Each table has a clearly defined responsibility.
7. **Scalability:** New guests, rooms, bookings, and payments can be added without duplicating unrelated information.

### 3.6 Conclusion

The Hotel Reservation System has been normalised progressively from an initial unnormalised design into 1NF, 2NF, and 3NF.

First Normal Form eliminates repeating groups and ensures that each field contains a single value. Second Normal Form eliminates partial dependencies by separating data according to the entities it describes. Third Normal Form removes inappropriate transitive dependencies by ensuring that guest, room, booking, payment, and check-in/check-out information are stored in their appropriate tables.

The resulting relational design provides a sound foundation for managing hotel reservations, room information, payments, and guest activities while maintaining data consistency and integrity.
