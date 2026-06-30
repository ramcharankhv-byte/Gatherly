# Gatherly - Product Requirements Document (PRD)

> **Tagline:** Discover. Create. Gather.

---

# Overview

Gatherly is a modern city-based event discovery and booking platform where users can discover events happening in their selected city, reserve their spot, and organizers can create and manage events.

Unlike location-based discovery, users manually choose their city after logging in, allowing a simpler and more scalable MVP while keeping the user experience clean.

The project is designed as a production-inspired full-stack application and also serves as a learning project for React by implementing features in increasing complexity.

---

# Objectives

## Primary Goals

- Discover events within a selected city.
- Allow users to reserve seats for events.
- Enable organizers to create and manage events.
- Prevent overbooking.
- Build a scalable architecture that can later support payments, AI recommendations, notifications, and seat selection.

---

# User Roles

## Event Attendee

Users who want to:

- Browse events
- View event details
- Book events
- View bookings
- Update profile
- Change city

---

## Event Organizer

Everything an attendee can do, plus:

- Create events
- Edit events
- Delete events
- View attendees
- Close registrations

---

# User Workflow

```
Landing Page
      │
      ▼
Login with Google
      │
      ▼
First Login?
      │
 ┌────┴─────┐
 │          │
Yes         No
 │          │
 ▼          ▼
Choose City Home Feed
      │
      ▼
Save Profile
      │
      ▼
Home Feed
      │
      ▼
Browse Events
      │
      ▼
Event Details
      │
      ▼
Book Event
      │
      ▼
Booking Successful
```

---

# Organizer Workflow

```
Login
   │
   ▼
Organizer Dashboard
   │
   ▼
Create Event
   │
   ▼
Publish Event
   │
   ▼
Users Book Event
   │
   ▼
Manage Attendees
```

---

# Authentication

Authentication will be handled using **Supabase Google OAuth**.

Flow

```
Landing Page
      │
      ▼
Continue with Google
      │
      ▼
Supabase Authentication
      │
      ▼
User Session Created
```

No email/password authentication will be implemented in the MVP.

---

# First Login

After authentication:

Check whether the user already has a profile.

If not:

```
Google Login
      │
      ▼
Profile Exists?
      │
 ┌────┴─────┐
 │          │
No         Yes
 │          │
 ▼          ▼
Choose City Home
      │
      ▼
Create Profile
      │
      ▼
Home
```

Profile contains:

- Name
- Email
- Avatar
- Selected City

---

# City Selection

Instead of GPS permissions, users manually select a city.

Supported cities (MVP):

- Hyderabad
- Bengaluru
- Chennai
- Mumbai
- Delhi
- Pune
- Kolkata
- Visakhapatnam

Users can later change their city from the Profile page.

---

# Home Feed

After login:

Display

- Greeting
- Current City
- Upcoming Events
- Popular Events
- Recent Events
- Categories

Example

```
Hello RC 👋

Current City

Visakhapatnam

Upcoming Events

Popular Events

Recent Events
```

---

# Browse Events

Only events belonging to the selected city are shown.

Example query

```sql
SELECT *
FROM events
WHERE city = 'Visakhapatnam';
```

Each event card displays

- Banner
- Event Title
- Category
- Organizer
- Date
- Time
- Address
- Seats Left

---

# Event Details

Displays

- Banner
- Title
- Description
- Organizer
- Category
- Date
- Time
- Address
- Google Maps Link
- Capacity
- Seats Left
- Book Button

---

# Booking Workflow

```
User Clicks Book
        │
        ▼
Already Booked?
        │
   Yes ─┴──► Show Error
        │
       No
        │
        ▼
Seats Available?
        │
   No ──┴──► Event Full
        │
       Yes
        │
        ▼
Create Booking
        │
        ▼
Booking Confirmed
```

---

# Booking Rules

Users cannot

- Book the same event twice.
- Book an event that has reached capacity.

Users can

- Cancel bookings.

---

# Event Capacity

Example

Capacity = 50

Bookings = 48

Seats Left = 2

If

```
Seats Left = 0
```

Disable booking button.

Display

```
Event Full
```

---

# Organizer Dashboard

Dashboard cards

- Total Events
- Upcoming Events
- Total Bookings
- Total Attendees

---

# Create Event

Organizers provide

- Title
- Description
- Category
- City
- Date
- Start Time
- End Time
- Address
- Google Maps URL
- Banner Image
- Capacity

Then publish.

---

# My Events

Each event allows

- Edit
- Delete
- View Attendees

---

# Attendee List

Display

- Avatar
- Name
- Email
- Booking Date

---

# User Dashboard

Displays

- Upcoming Bookings
- Past Bookings
- Cancelled Bookings

---

# Profile

Users can update

- Display Name
- Avatar
- City

---

# Tech Stack

## Frontend

- React
- React Router
- Tailwind CSS
- Axios

---

## Backend

- Supabase Authentication
- Supabase PostgreSQL
- Supabase Storage
- Row Level Security (RLS)

---

# Database Design

## Profiles

| Field      | Type      |
| ---------- | --------- |
| id         | UUID      |
| email      | Text      |
| name       | Text      |
| avatar_url | Text      |
| city       | Text      |
| created_at | Timestamp |

---

## Events

| Field        | Type      |
| ------------ | --------- |
| id           | UUID      |
| organizer_id | UUID      |
| title        | Text      |
| description  | Text      |
| category     | Text      |
| city         | Text      |
| address      | Text      |
| map_url      | Text      |
| banner_url   | Text      |
| event_date   | Date      |
| start_time   | Time      |
| end_time     | Time      |
| capacity     | Integer   |
| created_at   | Timestamp |

---

## Bookings

| Field      | Type      |
| ---------- | --------- |
| id         | UUID      |
| user_id    | UUID      |
| event_id   | UUID      |
| status     | Text      |
| created_at | Timestamp |

---

# Business Rules

## Authentication

Required for

- Booking
- Creating Events
- Dashboard
- Profile

---

## Booking

A user can only book an event once.

Unique Constraint

```
(user_id, event_id)
```

---

## Capacity

Bookings cannot exceed event capacity.

Seats Left

```
capacity - confirmed bookings
```

No separate `available_slots` field is stored.

---

## Organizer Permissions

Only the creator of an event can

- Edit
- Delete
- View attendees

---

## Event Restrictions

Past events become read-only.

---

# React Learning Roadmap

The project itself is designed to teach React progressively.

| Step | Feature        | React Concepts        |
| ---- | -------------- | --------------------- |
| 1    | Project Setup  | Vite, Components, JSX |
| 2    | Navbar         | Reusable Components   |
| 3    | Login Page     | Event Handling        |
| 4    | Google OAuth   | Async Functions       |
| 5    | Choose City    | useState              |
| 6    | Home Feed      | Lists, Props          |
| 7    | Event Cards    | Component Composition |
| 8    | Event Details  | React Router          |
| 9    | Booking        | API Integration       |
| 10   | Dashboard      | useEffect             |
| 11   | Create Event   | Forms                 |
| 12   | Profile        | Context API           |
| 13   | Loading States | Conditional Rendering |
| 14   | Final Polish   | Custom Hooks          |

---

# Project Structure

```
src/
│
├── assets/
├── components/
├── contexts/
├── hooks/
├── layouts/
├── lib/
├── pages/
│
│   ├── Landing
│   ├── Login
│   ├── ChooseCity
│   ├── Home
│   ├── EventDetails
│   ├── Dashboard
│   ├── CreateEvent
│   ├── Profile
│
├── services/
├── utils/
├── routes/
└── App.jsx
```

---

# Future Roadmap

## Phase 2

- Notifications
- Event Search
- Category Filters
- Event Likes

---

## Phase 3

- Razorpay
- Stripe
- Payment Verification

---

## Phase 4

- QR Ticket Generation
- QR Verification
- Email Confirmation

---

## Phase 5

- AI Event Recommendations
- AI Event Description Generator
- Smart Search

---

## Phase 6

- Seat Selection
- Queue Management
- Live Ticket Availability

---

# Non-Functional Requirements

- Responsive Design
- Mobile Friendly
- Fast Loading
- Secure Authentication
- Clean Architecture
- Scalable Database
- Proper Error Handling
- Reusable Components

---

# Success Criteria

The MVP is successful when

- Users can sign in with Google.
- Users can choose their city.
- Users can browse city-specific events.
- Organizers can create and manage events.
- Users can reserve events.
- Duplicate bookings are prevented.
- Event capacity is enforced.
- The application demonstrates production-quality frontend architecture using React.

---

# Long-Term Vision

```
Gatherly
      │
      ▼
City-Based Event Discovery
      │
      ▼
Event Booking Platform
      │
      ▼
Organizer Management
      │
      ▼
Online Payments
      │
      ▼
QR Ticketing
      │
      ▼
Seat Reservation
      │
      ▼
AI-Powered Event Platform
```

---

**Version:** 2.0 (MVP)

**Status:** Planning

**Primary Goal:** Build a production-quality full-stack event booking platform while learning React through real-world feature development and demonstrating frontend engineering skills for internships.
