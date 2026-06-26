# Gatherly - Product Requirements Document (PRD)

> **Tagline:** Discover. Create. Gather.

---

# Overview

Gatherly is a modern location-based event discovery and booking platform that enables users to explore nearby events, reserve tickets, and create their own events. The platform leverages geolocation and interactive maps to provide a seamless event discovery experience.

The MVP focuses on community events, workshops, hackathons, college fests, meetups, and sports events while providing a scalable foundation for future features like seat selection, real payment gateways, queue management, and AI-powered recommendations.

---

# Objectives

## Primary Goals

* Discover events based on the user's location.
* Allow users to book tickets with a smooth booking flow.
* Enable organizers to create and manage events.
* Simulate a complete booking and payment experience.
* Build a scalable architecture for future expansion.

---

# Target Users

## Event Attendees

Users who want to:

* Discover nearby events
* Book tickets
* Track upcoming bookings
* Receive event recommendations

## Event Organizers

Users who want to:

* Host events
* Manage registrations
* Track attendees
* Share event locations

---

# Core Features (MVP)

## Authentication

* Email Sign Up
* Login
* Logout
* Protected Routes
* User Session Management

---

## Location Detection

After login:

* Request location permission
* Detect user's current location
* Display nearby events
* Show event distance

---

## Browse Events

Users can:

* View nearby events
* Search events
* Filter by category
* Filter by date
* Sort by distance

Each event displays:

* Banner Image
* Title
* Description
* Venue
* Distance
* Date & Time
* Available Capacity

---

## Event Details

Displays:

* Banner
* Full Description
* Organizer
* Venue
* Interactive Map
* Event Date
* Capacity
* Book Ticket Button

---

## Book Event

Booking Flow

```
Browse Event
      ↓
Event Details
      ↓
Book Ticket
      ↓
Mock Payment
      ↓
Booking Confirmation
```

Rules:

* Login required
* One booking per user
* Cannot exceed event capacity

---

## Create Event

Organizers can:

* Add title
* Description
* Category
* Date
* Time
* Venue
* Capacity
* Upload banner image
* Select event location on OpenStreetMap

---

## User Dashboard

Users can:

* View upcoming bookings
* View booking history
* Cancel bookings
* Update profile

---

## Organizer Dashboard

Organizers can:

* Create events
* Edit events
* Delete events
* View attendee list
* Track registrations

---

# Mock Payment

The MVP simulates a payment flow.

```
Book Event
      ↓
Payment Screen
      ↓
Payment Successful
      ↓
Booking Confirmed
```

Store:

* Transaction ID
* Payment Status
* Booking Timestamp

---

# Tech Stack

## Frontend

* React
* React Router
* Tailwind CSS
* React Leaflet
* Axios

## Backend

* Supabase Authentication
* Supabase PostgreSQL
* Supabase Storage
* Row Level Security (RLS)

## Maps

* OpenStreetMap
* Leaflet

---

# Database Design

## Events

| Field           | Type      |
| --------------- | --------- |
| id              | UUID      |
| title           | Text      |
| description     | Text      |
| category        | Text      |
| banner_url      | Text      |
| venue_name      | Text      |
| latitude        | Decimal   |
| longitude       | Decimal   |
| event_date      | Timestamp |
| capacity        | Integer   |
| available_slots | Integer   |
| organizer_id    | UUID      |
| created_at      | Timestamp |

---

## Bookings

| Field          | Type      |
| -------------- | --------- |
| id             | UUID      |
| user_id        | UUID      |
| event_id       | UUID      |
| booking_status | Text      |
| payment_status | Text      |
| transaction_id | Text      |
| created_at     | Timestamp |

---

# Business Rules

## Capacity Validation

Bookings cannot exceed event capacity.

---

## Duplicate Booking Prevention

A user can only book an event once.

Unique Constraint:

```
(user_id, event_id)
```

---

## Organizer Permissions

Only the event creator can:

* Edit
* Delete
* View attendees

---

## Authentication

Login required for:

* Booking events
* Creating events
* Viewing dashboard

---

# Project Structure

```
src/
│
├── components/
├── pages/
├── layouts/
├── hooks/
├── context/
├── services/
├── lib/
├── utils/
├── assets/
└── types/
```

---

# Future Roadmap

## Phase 2

### Real Payment Gateway

* Razorpay
* Stripe
* Webhook verification

---

## Phase 3

### Venue Management

* Multiple venues
* Screens
* Seating layouts
* Seat categories

---

## Phase 4

### Seat Selection

Interactive seating layout similar to movie ticket booking.

```
A1 A2 A3 A4

B1 B2 B3 B4

C1 C2 C3 C4
```

---

## Phase 5

### Seat Locking

When payment begins:

```
Seat Selected
      ↓
Seat Locked
      ↓
5 Minute Timer
      ↓
Payment Success → Confirm Booking

OR

Timer Expired → Unlock Seat
```

This prevents multiple users from purchasing the same seat simultaneously.

---

## Phase 6

### Queue Management

Support high-demand events.

```
User
    ↓
Waiting Room
    ↓
Queue Number
    ↓
Booking Window
    ↓
Checkout
```

Useful for concerts, sports events, and movie releases.

---

## Phase 7

### AI Features

* AI-generated event descriptions
* Event recommendations
* Duplicate event detection
* Smart search
* Popularity prediction

---

# Non-Functional Requirements

* Responsive Design
* Mobile Friendly
* Fast Loading
* Secure Authentication
* Scalable Database
* Clean Architecture
* Proper Error Handling
* Accessible UI

---

# Success Criteria

The MVP is successful when:

* Users can discover nearby events.
* Users can create events.
* Users can book events successfully.
* Duplicate bookings are prevented.
* Event capacity is enforced.
* Organizers can manage their events.
* The application is ready for future upgrades without major architectural changes.

---

# Long-Term Vision

```
Gatherly
        │
        ▼
Location-Based Event Discovery
        │
        ▼
Online Ticket Booking
        │
        ▼
Venue Management
        │
        ▼
Seat Reservation System
        │
        ▼
Real Payment Integration
        │
        ▼
High Traffic Queue Management
        │
        ▼
AI-Powered Event Platform
```

---

**Version:** 1.0 (MVP)

**Status:** Planning

**Project Goal:** Build a production-inspired event booking platform that demonstrates full-stack engineering, scalable system design, and serves as the foundation for future features comparable to platforms like BookMyShow.
