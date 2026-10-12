1. Entities

The Hotel Reservation System consists of five main entities:

* Guest: Stores guest information, including GuestID, FullName, PhoneNumber, Email, Address, and DateRegistered. GuestID serves as the primary key.
* Room: Stores room information, including RoomID, RoomNumber, RoomType, RatePerNight, and RoomStatus. RoomID serves as the primary key, while RoomNumber must be unique.
* Booking: Stores reservation information, including BookingID, GuestID, RoomID, BookingDate, CheckInDate, CheckOutDate, NumberOfGuests, and BookingStatus. BookingID is the primary key, while GuestID and RoomID are foreign keys referencing the Guest and Room entities, respectively.
* Payment: Stores payment information, including PaymentID, BookingID, PaymentDate, Amount, PaymentMethod, TransactionReference, and PaymentStatus. PaymentID serves as the primary key, while BookingID is a foreign key referencing the Booking entity.
* CheckInCheckOut: Stores guests’ actual arrival and departure information, including RecordID, BookingID, ActualCheckIn, ActualCheckOut, and StaffName. RecordID serves as the primary key, while BookingID is a foreign key referencing the Booking entity.

2. Business Rules

* Every guest must have a unique GuestID, and every room must have a unique RoomID and RoomNumber.
* A guest may make multiple bookings over time, but each booking must belong to one registered guest.
* Each booking must reference one existing room.
* A room may have multiple bookings over time, provided confirmed bookings do not overlap.
* Every booking must have a unique BookingID and a valid booking status, such as Pending, Confirmed, Cancelled, or Completed.
* The check-out date must be later than the check-in date.
* The number of guests for a booking must be at least one and must not exceed the room’s permitted occupancy.
* A room must not be assigned to a new booking for dates when it is unavailable or under maintenance.
* Each payment must have a unique PaymentID and must be associated with an existing booking.
* A booking may have multiple payments, allowing deposits and subsequent payments.
* Payment amounts must be greater than zero, and only successful payments count toward the amount paid.
* Every check-in/check-out record must reference an existing booking.
* Each booking may have at most one check-in/check-out record in this design.
* A guest must have a valid booking before checking in.
* The actual check-out time must not be earlier than the actual check-in time.
* A booking should only be marked Completed after the guest has checked out.
* Primary keys, foreign keys, unique constraints, and other validation rules must be used to maintain data integrity and consistency.
