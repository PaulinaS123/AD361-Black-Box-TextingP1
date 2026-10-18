# North Vacation Booking: Black-Box Testing, Part 1
### Student: Victoria Salomon
### AD361 Application Testing
----
## 1. Introduction

This assignment applies black-box testing and equivalence partitioning to the North Vacation Booking application. The goal is to identify valid and invalid input groups and design test cases based on the application requirements without examining its source code.

## 2. Equivalence Partition Tables

### 2.1 Number of Nights ( Range )

Requirement: A reservation must be between 1 and 31 nights, inclusive.

| Partition ID | Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|---|
| N1 | Number of nights | Invalid low | Fewer than 1 night | ValueError |
| N2 | Number of nights | Valid | 1–31 nights | Reservation accepted, assuming all other requirements are met |
| N3 | Number of nights | Invalid high | More than 31 nights | ValueError |

### 2.2 Number of Guests

Requirement: A reservation must include between 1 and 12 guests, inclusive. Each guest over five costs an additional $25 per night.

| Partition ID | Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|---|
| G1 | Number of guests | Invalid low | Fewer than 1 guest | ValueError |
| G2 | Number of guests | Valid, standard rate | 1–5 guests | No additional guest fee |
| G3 | Number of guests | Valid, additional fee | 6–12 guests | Additional $25 per guest, per night |
| G4 | Number of guests | Invalid high | More than 12 guests | ValueError |

### 2.3 Check-in and Checkout Dates

Requirement: Check-in must be before checkout, and the stay must be between 1 and 31 nights, inclusive.

| Partition ID | Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|---|
| D1 | Check-in and checkout dates | Valid | Check-in is before checkout, with a stay of 1–31 nights | Reservation accepted, assuming all other requirements are met |
| D2 | Check-in and checkout dates | Invalid, same date | Check-in date equals checkout date | ValueError |
| D3 | Check-in and checkout dates | Invalid, reversed dates | Check-in date is after checkout date | ValueError |
| D4 | Check-in and checkout dates | Invalid, too many nights | Checkout date is more than 31 nights after check-in | ValueError |

### 2.4 Availability

Requirement: Every night in the requested date range must be available.

| Partition ID | Classification | Partition | Range or Condition | Expected Result |
|---|---|---|---|---|
| A1 | Availability | Valid, fully available | Every night in the requested date range is available | Reservation accepted, assuming all other requirements are met |
| A2 | Availability | Invalid, unavailable night | At least one night in the requested date range is unavailable | UnavailableError |

## 3. Test Cases

Assumption: Unless stated otherwise, all other reservation information is valid, and all requested nights are available.

### 3.1 Number of Nights

**TC01: Invalid Low Number of Nights**

- **Test-case ID:** TC01
- **Classification:** Number of nights
- **Partition:** N1 - Invalid low
- **Input:** A reservation for 0 nights.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the reservation must be at least 1 night.

**TC02: Valid Number of Nights**

- **Test-case ID:** TC02
- **Classification:** Number of nights
- **Partition:** N2 - Valid
- **Input:** A reservation for 10 nights with 3 guests, with all nights available.
- **Expected Result:** The reservation is accepted. The total cost is $2,050, including the $2,000 nightly cost and the $50 cleaning fee.

**TC03: Invalid High Number of Nights**

- **Test-case ID:** TC03
- **Classification:** Number of nights
- **Partition:** N3 - Invalid high
- **Input:** A reservation for 32 nights.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the reservation cannot exceed 31 nights.

### 3.2 Number of Guests

**TC04: Invalid Low Number of Guests**

- **Test-case ID:** TC04
- **Classification:** Number of guests
- **Partition:** G1 - Invalid low
- **Input:** A reservation for 0 guests.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because at least 1 guest is required.

**TC05: Valid Standard Number of Guests**

- **Test-case ID:** TC05
- **Classification:** Number of guests
- **Partition:** G2 - Valid, standard rate
- **Input:** A reservation for 3 guests staying 2 nights.
- **Expected Result:** The reservation is accepted, and no additional guest fee is added. The total cost is $450, including the $400 nightly cost and the $50 cleaning fee.

**TC06: Valid Number of Guests With Additional Fee**

- **Test-case ID:** TC06
- **Classification:** Number of guests
- **Partition:** G3 - Valid, additional fee
- **Input:** A reservation for 6 guests staying 2 nights.
- **Expected Result:** The reservation is accepted. One additional guest incurs a fee of $25 per night, resulting in a total cost of $500, including the $400 nightly cost and the $50 cleaning fee.

**TC07: Invalid High Number of Guests**

- **Test-case ID:** TC07
- **Classification:** Number of guests
- **Partition:** G4 - Invalid high
- **Input:** A reservation for 13 guests.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the maximum is 12 guests.

### 3.3 Check-in and Checkout Dates

**TC08: Valid Check-in and Checkout Dates**

- **Test-case ID:** TC08
- **Classification:** Check-in and checkout dates
- **Partition:** D1 - Valid
- **Input:** Check-in on October 10 and checkout on October 15, with all five reserved nights available.
- **Expected Result:** The reservation is accepted for 5 nights. October 15 is not counted as a reserved night.

**TC09: Invalid Same-Day Check-in and Checkout**

- **Test-case ID:** TC09
- **Classification:** Check-in and checkout dates
- **Partition:** D2 - Invalid, same date
- **Input:** Check-in and checkout on October 10.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the check-in date must be before the checkout date.

**TC10: Invalid Reversed Check-in and Checkout Dates**

- **Test-case ID:** TC10
- **Classification:** Check-in and checkout dates
- **Partition:** D3 - Invalid, reversed dates
- **Input:** Check-in on October 15 and checkout on October 10.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the check-in date is after the checkout date.

**TC11: Invalid Reservation Exceeding 31 Nights**

- **Test-case ID:** TC11
- **Classification:** Check-in and checkout dates
- **Partition:** D4 - Invalid, too many nights
- **Input:** Check-in on October 1 and checkout on November 2.
- **Expected Result:** The reservation is rejected, and a ValueError is raised because the stay exceeds 31 nights.

### 3.4 Availability

**TC12: Reservation With All Nights Available**

- **Test-case ID:** TC12
- **Classification:** Availability
- **Partition:** A1 - Valid, fully available
- **Input:** A reservation from October 10 to October 13, with October 10, 11, and 12 all available.
- **Expected Result:** The reservation is accepted because every requested night is available.

**TC13: Reservation With an Unavailable Night**

- **Test-case ID:** TC13
- **Classification:** Availability
- **Partition:** A2 - Invalid, unavailable night
- **Input:** A reservation from October 10 to October 13, with October 11 unavailable and the other requested nights available.
- **Expected Result:** The reservation is rejected, and an UnavailableError is raised because at least one requested night is unavailable.

