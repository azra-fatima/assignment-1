# Initialize coach layout
ROWS = 11
SEATS_PER_ROW = 7
TOTAL_SEATS = 80
LAST_ROW_SEATS = 3

# Simulate the seats table in the database
# False means the seat is booked, True means it's available
seats = [True] * TOTAL_SEATS

# Already booked seats for illustration
# e.g., seats[5] means seat 6 is booked (zero-indexed)
booked_seats = [5, 10, 25, 33, 47, 63, 74]

# Mark booked seats in the seats list
for seat in booked_seats:
    seats[seat] = False

# Function to book seats
def book_seats(requested_seats):
    if requested_seats > SEATS_PER_ROW:
        print("Max seats that can be booked at a time is 7.")
        return None

    available_seats = []
    row_number = 0
    # Search for seats row by row
    for row in range(ROWS + 1):  # including the last partial row
        row_start = row * SEATS_PER_ROW
        row_end = row_start + (SEATS_PER_ROW if row < ROWS else LAST_ROW_SEATS)

        current_row_seats = seats[row_start:row_end]
        
        if current_row_seats.count(True) >= requested_seats:
            # Book seats in this row
            for i in range(row_start, row_end):
                if seats[i]:
                    available_seats.append(i + 1)
                    seats[i] = False
                    if len(available_seats) == requested_seats:
                        return available_seats
    
    # If no single row could fulfill the request, find nearby available seats
    nearby_seats = []
    for i, seat in enumerate(seats):
        if seat:
            nearby_seats.append(i + 1)
            seats[i] = False
            if len(nearby_seats) == requested_seats:
                return nearby_seats

    return nearby_seats

# Test booking function
requested_seats = 3
booked = book_seats(requested_seats)

if booked:
    print(f"Seats booked: {booked}")
else:
    print("Booking failed. Not enough seats available.")
