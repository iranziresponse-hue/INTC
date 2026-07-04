# Candys Palace Integrated Guest House Management System

## 1. Purpose
This repository now documents the target design for an **Integrated Guest House Management System** for Candys Palace. The system unifies:
- Room booking (website + Booking.com)
- Accommodation/front-desk operations
- Bar/restaurant POS sales integration
- Teller and receptionist workflows
- Owner-level consolidated analytics and monthly reporting

## 2. Scope
### In scope
- Public website booking engine for rooms
- Booking.com channel manager integration (extensible to additional OTAs)
- Integration with the existing POS machine
- Teller portal (bar/restaurant operations)
- Receptionist portal (accommodation operations)
- Boss/Admin dashboard with monthly analytics reports
- Centralized RBAC user/access management

### Out of scope
- Replacement of physical POS hardware
- Payroll/HR
- Inventory/procurement management (future phase)

## 3. Stakeholders and Roles
- **Guest**: browse rooms, check availability, book/pay, receive confirmation
- **Teller**: manage bar/restaurant orders and payments, shift reconciliation
- **Receptionist**: manage bookings, check-in/check-out, room assignment/status
- **Boss/Admin**: view consolidated reporting, manage users, rates, menu, integrations
- **System Administrator (optional)**: technical configuration, backup, integration oversight

## 4. Functional Requirements

### 4.1 Guest-Facing Booking Module
- Display room types, descriptions, photos, amenities, rates
- Real-time availability by check-in/check-out dates
- Booking creation with guest details
- Mobile money and/or card payment support
- Automatic confirmation via email/SMS
- Reference-based guest self-service (view/modify/cancel within policy)
- Optional booking add-ons (e.g., breakfast, airport pickup)

### 4.2 OTA Integration (Booking.com)
- Two-way inventory/rate synchronization via channel manager
- Double-booking prevention through real-time channel updates
- Unified reservation inbox across website/OTA/walk-in/phone
- Channel-based rate parity controls
- Extensible connector model for additional OTAs

### 4.3 POS Integration (Bar & Restaurant)
- Integration preference order:
  1. API/export integration
  2. DB-level integration
  3. Manual/CSV import fallback
- Capture department, item, quantity, price, teller, timestamp
- Support split payment methods
- Daily sales reconciliation by teller/shift

### 4.4 Teller Portal
- Secure login (optional quick PIN)
- Dashboard: today’s sales, open tabs, shift summary
- Order lifecycle: create/update/discount/close
- Sales history by date and permission scope
- Shift start/end tracking
- End-of-shift expected vs actual cash reconciliation

### 4.5 Receptionist Portal
- Secure login
- Room/calendar grid with status states
- Walk-in booking creation
- Check-in/check-out workflows with payment and invoice support
- Unified booking list across all channels
- Housekeeping/maintenance room flags
- Daily arrivals/departures reporting

### 4.6 Boss Dashboard & Reporting
- Real-time consolidated metrics across accommodation + restaurant + bar
- Automated monthly report with:
  - Revenue by department and total
  - Occupancy, ADR, RevPAR with month-over-month trends
  - Booking source breakdown
  - Top-selling items
  - Teller performance comparison
  - Cancellation/no-show rates
  - Average length of stay
  - Month-on-month and year-on-year trend charts
- Drill-down from monthly to weekly/daily detail
- Export as PDF/Excel and optional auto-email
- User management and pricing/menu management interfaces

## 5. Non-Functional Requirements
- **Security**: RBAC, password hashing (bcrypt/argon2), TLS, encrypted sensitive data, audit trails
- **Availability**: target ≥99% uptime, offline queue/sync behavior for teller/receptionist workflows
- **Performance**: room search < 2s, POS sync within 5 minutes
- **Scalability**: supports more rooms, OTAs, and branches
- **Usability**: optimized counter workflows on tablet/desktop
- **Backup/Recovery**: daily automated backups + disaster recovery plan
- **Localization**: UGX support and optional multi-currency display

## 6. High-Level Architecture
1. Public website + OTA traffic enters booking engine/channel manager
2. Core backend/API centralizes business logic
3. Shared relational database stores users, rooms, bookings, sales, reports
4. Receptionist and teller portals consume role-scoped APIs
5. POS adapter normalizes sales into central transactions
6. Reporting engine computes KPIs and generates monthly files
7. Notification services send email/SMS confirmations and alerts

## 7. Key Data Entities
- **Guest**: identity/contact details
- **RoomType** and **Room**: catalog + operational status
- **Booking**: dates, source, status, payment state
- **MenuItem**: bar/restaurant item catalog
- **SalesTransaction**: POS-linked sale metadata and payment breakdown
- **User**: role-based identity and access control
- **ShiftLog**: teller shift accounting
- **Report**: generated analytical outputs

## 8. Integration Strategy
- Prefer certified channel manager for Booking.com connectivity
- Implement POS sync adapter with staged integration fallback
- Integrate regional payment providers for mobile money and card processing

## 9. Recommended Stack (Reference)
- Frontend: React/Next.js
- Backend: Node.js (Express/NestJS) or Python (Django/FastAPI)
- Database: PostgreSQL/MySQL
- Channel manager: Cloudbeds/SiteMinder/Hotelogix
- Payments: Flutterwave/Pesapal
- Notifications: SMS gateway + SMTP/SendGrid
- Reporting: scheduled server-side jobs generating PDF/Excel

## 10. Phased Rollout
1. Core backend, database, receptionist portal, basic website
2. Online booking + payment gateway
3. Booking.com/channel manager sync
4. POS adapter + teller portal
5. Boss dashboard + automated reporting
6. UAT, staff training, go-live, monitoring

## 11. Constraints and Assumptions
- Existing POS provides API/export/DB access, else manual fallback
- Booking.com property account is available for integration
- Internet availability supports real-time sync; offline queues mitigate outages
- Staff training is part of rollout
