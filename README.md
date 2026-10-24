# Hotel Booking Web Application

A responsive hotel discovery and booking interface built with React. Visitors can explore hotel rooms, view room details, and see a sample bookings page. A hotel owner area provides dashboard, room listing, and add-room screens.

> **Project status:** This repository currently contains a frontend prototype. Room, booking, and dashboard content comes from local demo data in `src/assets/assets.js`. The screens do not currently call a hotel API or persist bookings and room changes to a database. Clerk is wired for sign-in UI; the demo data is not connected to the signed-in account.

## Features

- Landing page with destination search UI, featured destinations, offers, testimonials, and newsletter sections.
- Room catalogue at `/rooms` with room cards and filter/sort controls in the interface.
- Room detail pages at `/rooms/:id` with image selection, amenities, and a date/guest form.
- Sample booking history at `/my-bookings`.
- Owner area at `/owner` with a dashboard, add-room form, and room list.
- Clerk-powered sign-in controls and a user menu in the navigation.
- Responsive styling with Tailwind CSS.

## Tech stack

- **React 19** for UI components.
- **Vite 6** for the development server and production build.
- **React Router 7** for client-side routes.
- **Clerk React** for authentication UI.
- **Tailwind CSS 4** with the Vite plugin for styling.
- **ESLint 9** for linting.

This repository does not include a server, database, payment provider, or hotel API integration.

## Project structure

```text
.
├── public/                     # Static public assets
├── src/
│   ├── assets/
│   │   ├── assets.js           # Icons, room/hotel/booking demo data, shared assets
│   │   └── *.svg, *.png        # Images and icons used by the UI
│   ├── components/             # Shared guest-facing UI components
│   │   └── hotelOwner/         # Owner navigation and sidebar
│   ├── pages/
│   │   ├── Home.jsx            # Landing page
│   │   ├── AllRooms.jsx        # Room catalogue
│   │   ├── RoomDetails.jsx     # Details for one room
│   │   ├── MyBookings.jsx      # Sample booking history
│   │   └── hotelOwner/         # Owner layout, dashboard, add/list room pages
│   ├── App.jsx                 # Route definitions and shared page shell
│   ├── main.jsx                # React entry point, Clerk and router providers
│   └── index.css               # Tailwind import, fonts, theme colors
├── index.html                  # Vite HTML entry point
├── vite.config.js              # Vite, React, and Tailwind plugin configuration
├── eslint.config.js            # ESLint configuration
└── package.json                # Scripts and dependencies
```

## Application flow

1. **Start at the home page (`/`).** The landing page composes the hero search form and promotional sections. The form is currently presentation-only.
2. **Browse rooms (`/rooms`).** The catalogue renders the room objects from `roomsDummyData` in `src/assets/assets.js`. Selecting a room opens its detail route.
3. **View a room (`/rooms/:id`).** The page looks up the matching demo room by ID, shows its images, amenities, and nightly price, and presents a check-in/check-out/guest form. The form does not create a reservation or check availability against a service yet.
4. **Open bookings (`/my-bookings`).** The page renders `userBookingsDummyData`; its payment status and “Pay Now” button are examples and are not backed by payment processing.
5. **Open the owner area (`/owner`).** The nested owner routes share an owner layout and navigation. The dashboard and room list use demo data, while the add-room form is a UI example and does not save a room.

`src/App.jsx` defines these routes. `src/main.jsx` mounts the app inside `BrowserRouter` and `ClerkProvider`. The global navigation is hidden on paths containing `owner`; the owner layout supplies its own navigation.

## Getting started

### Prerequisites

- Node.js (an active LTS release is recommended)
- npm
- A Clerk application and its **publishable key** for sign-in UI

### Install and run

```bash
npm install
```

Create a `.env` file in the project root and provide the Clerk publishable key:

```env
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
```

Then start the Vite development server:

```bash
npm run dev
```

Open the local URL printed by Vite (typically `http://localhost:5173`). The app checks for `VITE_CLERK_PUBLISHABLE_KEY` at startup and displays an error if it is missing.

### Available scripts

```bash
npm run dev      # Start the development server
npm run build    # Create a production build in dist/
npm run preview  # Serve the production build locally
npm run lint     # Run ESLint
```

## Data and extending the prototype

The sample hotels, rooms, bookings, dashboard totals, locations, and amenity mappings are kept in `src/assets/assets.js`. To make the booking flow persistent, connect the page forms and owner actions to a backend/API, replace the demo data with API responses, and implement the required availability, booking, and payment logic. Clerk currently supplies client-side sign-in components; authorization for owner operations must also be enforced by a trusted backend before those operations are treated as secure.

## Notes

- `How to Run Project.pdf` is included in the repository as a separate run guide.
- Do not commit real secrets. The Vite `VITE_` prefix exposes a variable to browser code, so use it only for public client-side values such as the Clerk publishable key; never put a Clerk secret key or other server secret in this variable.
