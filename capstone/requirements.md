# BookNow — Sprint 1 Requirements

**Project:** BookNow Hotel Booking
**Sprint:** Sprint 1
**Sprint Goal:** Deliver a working login, room browsing, booking, and confirmation flow
**Sprint Duration:** 2 weeks
**Status:** Ready for QA

> Capstone spec. Use it to verify the BookNow app the way you used TechShop's
> requirements — check that every requirement is covered and flag every deviation
> as a finding. The number and location of bugs are **not** disclosed.

---

## In Scope

### Login Page

- Users log in with email and password
- Email must be a valid format (contains @ and a domain)
- Password field must mask input characters
- Empty fields must be rejected with an error message
- Valid credentials: `guest@booknow.com` / `stay2026`
- Successful login redirects to the Available Rooms page
- Failed login shows an error message and stays on the login page
- The top navigation bar is hidden until the user is authenticated
- Protected pages (rooms, booking, confirmation) are unreachable without logging in

### Available Rooms (Search Results)

- Grid of available rooms
- Each card: room name, description, image, price per night, discounted price,
  discount badge, availability badge, and a "Select Room" button
- Discounted price = price × (1 − discount% / 100), shown to two decimals
- Selecting a room opens the booking form for that room
- Browser tab title: "BookNow — Hotel Room Booking"

### Booking Form

- Reached by selecting a room
- Guest Details: First Name, Last Name, Email, Phone
- Stay Details: Check-in Date, Check-out Date, Number of Guests
- All fields required — empty submission rejected with per-field errors
- Email: valid format
- Phone: digits (spaces, +, -, parentheses allowed), at least 7 characters
- Check-out date must be **after** the check-in date
- Number of Guests: minimum 1
- Booking summary shows: room name, price per night, number of nights, estimated total
- Number of nights = check-out minus check-in (in days)
- Estimated total = discounted price per night × number of nights

### Confirmation Page

- Shown after a successful booking
- Displays a unique booking reference in the format `BKN-` followed by 6 digits
- Summary: guest name, email, room, check-in, check-out, nights, total charged
- "Browse More Rooms" returns to the Available Rooms page

---

## Out of Scope — Sprint 1

- Real payment processing
- Email confirmation notifications
- User registration and password reset
- Real-time availability / inventory
- Admin dashboard
- Mobile responsive design
- Cross-browser testing (Chrome only)
- Room search and filtering

---

## Technical Context

- Static HTML/CSS/JavaScript — no backend server
- Testing in Chrome (latest), served locally
- No staging environment
- Credentials hardcoded: `guest@booknow.com` / `stay2026`
- Booking state in JavaScript variables — not persisted

---

## QA Resource

- Tester: you (independent capstone)
- Browser: Chrome latest
- Environment: local, served with `npx serve`
