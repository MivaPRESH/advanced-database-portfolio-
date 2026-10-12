# Case Study: Hotel Reservation Management System

## Organisation and Nature of the System

This project models a simulated hotel that provides accommodation services to guests. The hotel manages room reservations, guest information, payments, and check-in/check-out activities. The proposed database system will centralise these records to improve reservation management, maintain accurate room availability, and reduce booking conflicts.

## Database Users

The main users of the system are receptionists, hotel administrators, finance personnel, and hotel managers.

- Receptionists manage guest registration, room reservations, and check-in/check-out activities.
- Hotel administrators maintain room details, room types, and room rates.
- Finance personnel record and verify guest payments.
- Hotel managers review reservation records, room occupancy, and payment information.

## Major Data Stored

The database will store guest details, room information, bookings, payment records, and check-in/check-out records.

- **Guests:** Guest ID, full name, phone number, email address, and address.
- **Rooms:** Room ID, room number, room type, rate per night, and room status.
- **Bookings:** Booking ID, guest ID, room ID, booking date, check-in date, check-out date, and booking status.
- **Payments:** Payment ID, booking ID, payment date, amount paid, payment method, and payment status.
- **Check-in/Check-out Records:** Record ID, booking ID, actual check-in time, actual check-out time, and staff member responsible.

## Key Operations

The database will support the following operations:

- Register new guests and update their details.
- Add rooms and update room information and rates.
- Search for available rooms for specified dates.
- Create, update, and cancel room reservations.
- Prevent overlapping confirmed bookings for the same room.
- Record guest payments and retrieve payment history.
- Record guest check-in and check-out activities.
- Retrieve a guest's booking history.
- Generate reports on room occupancy and reservations.

## Business Rules

- Each guest must have a unique guest ID.
- Each room must have a unique room ID and room number.
- Each booking must reference an existing guest and room.
- A guest may make multiple bookings over time.
- A room may have multiple bookings over time, provided confirmed reservations do not overlap.
- Each payment must be associated with an existing booking.
- The check-out date must be later than the check-in date.
- Payment amounts and room rates must not be negative.
- A booking may have no check-in/check-out record before arrival, but it must reference an existing booking if such a record exists.

## Expected Outcome

The proposed system will provide a structured and reliable way to manage hotel reservations, guest information, payments, and room occupancy. The database will use relational database principles, including primary keys, foreign keys, integrity constraints, and normalisation, to maintain data accuracy and consistency.
