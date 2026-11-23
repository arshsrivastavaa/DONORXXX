# DonarXXX – Blood Bank & Donor Management System

> **A modern, production-ready, full-stack Blood Bank and Donor Management System** engineered to digitize, centralize, and automate blood inventory tracking, voluntary donor appointment scheduling, hospital emergency blood requisitions, cold-chain shelf-life monitoring, and role-based operational governance.

---

## 🌟 Executive Overview

**DonarXXX** bridges the critical time gap between hospital emergency blood demand and blood bank inventory reserves. Built with Next.js 14 (App Router), TypeScript, Prisma ORM, NextAuth.js, and Tailwind CSS, the platform enforces strict Role-Based Access Control (RBAC), prevents inventory double-allocation through transactional operations, and provides real-time automated alerts for low stock levels and expiring blood components.

---

## 🚀 Key Features by Module

### 1. 🛡️ Authentication & Role-Based Access Control (RBAC)
- Multi-tenant architecture supporting 4 distinct user roles:
  - **DONOR**: Self-service profile, voluntary appointment booking, donation history, and blood compatibility guide.
  - **BLOOD_BANK_STAFF**: Cold-chain inventory management, unit addition, hospital requisition review, and transactional unit allocation.
  - **HOSPITAL**: Emergency and routine requisition submission, patient tracking, and live order dispatch tracking.
  - **ADMIN**: System-wide governance, user provisioning/suspension, dynamic safety thresholds, and immutable audit logs.
- Password hashing with `bcryptjs`.
- Session management via JWT with server-side role validation on every API endpoint.
- One-click quick demo login selector on the sign-in screen for rapid evaluation.

### 2. 🩸 Full Blood Group Support & Compatibility Engine
- Comprehensive support for all **8 clinical blood groups**:
  - `A+`, `A-`, `B+`, `B-`, `AB+`, `AB-`, `O+`, `O-`
- Interactive Universal Blood Compatibility Engine mapping donor-recipient compatibility for Red Blood Cells (RBC) and components.

### 3. 📦 Cold-Chain Blood Inventory Module
- Tracks 5 distinct blood component types with realistic clinical shelf lives:
  - **Whole Blood** (35 days)
  - **Packed RBC** (42 days)
  - **Platelets** (5 days)
  - **Plasma** (365 days)
  - **Cryoprecipitate** (365 days)
- Comprehensive unit statuses:
  - `AVAILABLE`, `RESERVED`, `ISSUED`, `QUARANTINED`, `EXPIRED`, `DISCARDED`
- Real-time stock summary cards for each of the 8 blood groups comparing available reserves against safety thresholds.
- Automatic near-expiry detection (units expiring within configured window, e.g., 7 days).
- Automated expiration transition for expired units.

### 4. 🏥 Hospital Blood Requisition & Allocation Module
- Hospitals submit orders with urgency levels:
  - `NORMAL` (Scheduled procedures)
  - `URGENT` (Within 24 hours)
  - `EMERGENCY` (Immediate trauma / acute hemorrhage)
- Requisition status lifecycle:
  - `SUBMITTED` ➔ `UNDER_REVIEW` ➔ `PARTIALLY_FULFILLED` ➔ `FULFILLED` / `REJECTED` / `CANCELLED`
- **Transactional Unit Allocation**:
  - Atomic database transactions ensure selected blood units are locked, marked as `ISSUED`, and linked via `RequestFulfilment`.
  - Guarantees zero double-allocation of units.
  - In-app notification automatically dispatched to the requesting hospital upon allocation.

### 5. 📅 Donor Scheduling & Geo-Registry
- Donors select preferred dates and 1-hour appointment slots.
- Staff confirm visits, record completions, or flag missed slots.
- Geo-search engine allows blood banks to locate registered donors by blood group and city/state for emergency drives.
- **Privacy Masking**: Hospital partners can view regional donor availability counts without accessing private contact details (HIPAA-aligned).

### 6. 📊 Reports & Visual Analytics
- Interactive charts powered by **Recharts**:
  - Blood component breakdown per blood group (Stacked Bar Chart).
  - Storage lifecycle proportions (Pie Chart).
  - Historical donation intake trends (Line Chart).
- One-click **Export to CSV** for offline inventory auditing.

### 7. 📜 Immutable Audit Ledger & System Configuration
- Comprehensive audit trail recording actor identity, role, action, target entity, entity ID, and timestamp.
- Configurable safety thresholds:
  - Global and group-specific low-stock alert thresholds.
  - Near-expiry warning window (default: 7 days).

---

## 🔑 Default Login Credentials

The database comes pre-seeded with accounts for all roles. You can click any role badge directly on the `/login` screen to auto-fill these credentials:

| Role | Email | Password | Access Scope |
| :--- | :--- | :--- | :--- |
| **System Admin** | `admin@donarxxx.com` | `admin123` | Full access, user governance, audit trail, config |
| **Blood Bank Staff** | `staff1@donarxxx.com` | `staff123` | Inventory CRUD, unit allocation, appointments |
| **Blood Bank Staff #2** | `staff2@donarxxx.com` | `staff123` | Inventory CRUD, unit allocation, appointments |
| **Hospital (City Care)** | `cityhospital@donarxxx.com` | `hospital123` | Blood requisitions, order status, stock glance |
| **Hospital (Metro Trauma)** | `metro@donarxxx.com` | `hospital123` | Emergency requisitions, order status |
| **Hospital (St. Jude)** | `stjude@donarxxx.com` | `hospital123` | Pediatric blood requisitions |
| **Donor (O+)** | `donor1@donarxxx.com` | `donor123` | Appointment booking, donation history, profile |
| **Donor (O-)** | `donor2@donarxxx.com` | `donor123` | Universal donor profile, appointment booking |
| **Donor (A+)** | `donor3@donarxxx.com` | `donor123` | Appointment booking, profile |
| **Donor (B+)** | `donor5@donarxxx.com` | `donor123` | Appointment booking, profile |
| **Donor (AB+)** | `donor7@donarxxx.com` | `donor123` | Universal recipient profile |

*(Donors `donor1@donarxxx.com` through `donor16@donarxxx.com` are available, all using password `donor123`)*

---

## 🛠️ Technology Stack

- **Framework**: [Next.js 14](https://nextjs.org/) (App Router, Server Components & Route Handlers)
- **Language**: [TypeScript](https://www.typescriptlang.org/) (Strict typing across models, APIs, and components)
- **Styling**: [Tailwind CSS](https://tailwindcss.com/) (Medical theme with crimson red & slate accents)
- **Database & ORM**: [Prisma ORM](https://www.prisma.io/)
  - Zero-config local demo: **SQLite** (`dev.db`)
  - Production deployment ready: **PostgreSQL** (`prisma/schema.postgresql.prisma`)
- **Authentication**: [NextAuth.js](https://next-auth.js.org/) (Credentials Provider + JWT + RBAC)
- **Input Validation**: [Zod](https://zod.dev/)
- **Data Visualization**: [Recharts](https://recharts.org/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **Date Manipulation**: [date-fns](https://date-fns.org/)

---

## 📁 Project Structure

```
DonarXXX/
├── app/
│   ├── (auth)/
│   │   ├── login/page.tsx               # Login screen with 1-click demo switcher
│   │   └── register/page.tsx            # Donor & Hospital registration
│   ├── admin/
│   │   ├── audit/page.tsx               # Immutable audit log ledger
│   │   ├── config/page.tsx              # System thresholds & alert windows
│   │   └── users/page.tsx               # User management & governance
│   ├── api/
│   │   ├── admin/                       # Admin APIs (audit, config, users)
│   │   ├── appointments/                # Appointment booking & status APIs
│   │   ├── auth/                        # NextAuth handler & registration API
│   │   ├── donors/                      # Donor directory & profile APIs
│   │   ├── inventory/                   # Blood unit CRUD & summary APIs
│   │   ├── notifications/               # In-app notifications & read receipts
│   │   ├── reports/                     # Analytical metrics & chart APIs
│   │   └── requests/                    # Hospital requisition & allocation APIs
│   ├── appointments/page.tsx            # Staff appointment schedule roster
│   ├── dashboard/page.tsx               # Multi-role dashboard dispatcher
│   ├── donor/
│   │   ├── appointments/page.tsx        # Donor visit booking & history
│   │   └── profile/page.tsx             # Donor profile & medical notes
│   ├── donors/page.tsx                  # Geo-donor search & compatibility guide
│   ├── inventory/page.tsx               # Cold-chain blood inventory manager
│   ├── my-requests/page.tsx             # Hospital requisition portal
│   ├── reports/page.tsx                 # Visual analytics & CSV export
│   ├── requests/page.tsx                # Staff requisition review & allocation
│   ├── globals.css                      # Tailwind base & medical styling
│   ├── layout.tsx                       # Root HTML & AuthProvider wrapper
│   └── page.tsx                         # Public landing page & matrix widget
├── components/
│   ├── dashboard/                       # Role-specific dashboard components
│   │   ├── AdminDashboard.tsx
│   │   ├── DonorDashboard.tsx
│   │   ├── HospitalDashboard.tsx
│   │   └── StaffDashboard.tsx
│   ├── layout/                          # App shell (Navbar, Sidebar, Shell)
│   ├── providers/                       # AuthProvider (SessionProvider)
│   └── ui/                              # Atoms (Badge, Button, Card, Modal)
├── lib/
│   ├── audit.ts                         # Centralized audit logging helper
│   ├── auth.ts                          # NextAuth options & RBAC guard functions
│   ├── constants.ts                     # Blood groups, components, compatibility
│   ├── inventory-service.ts             # Transactional allocation & stock logic
│   ├── prisma.ts                        # Prisma Client singleton
│   ├── utils.ts                         # Formatting & identifier generators
│   └── validations.ts                   # Zod schemas for all models & mutations
├── prisma/
│   ├── dev.db                           # Local SQLite database
│   ├── schema.prisma                    # Active Prisma schema (SQLite)
│   ├── schema.postgresql.prisma         # Production PostgreSQL schema (Enums)
│   └── seed.js                          # Database seed script (50+ units, 16 donors)
├── types/                               # TypeScript definitions & NextAuth augmentations
├── .env                                 # Local environment variables
├── .env.example                         # Environment configuration template
├── package.json
├── tailwind.config.ts
└── tsconfig.json
```

---

## ⚡ Quick Start (Local Setup)

### 1. Prerequisites
- **Node.js** (v18.17.0 or higher)
- **npm** or **pnpm**

### 2. Clone / Open Directory
```bash
cd DonarXXX
```

### 3. Install Dependencies
```bash
npm install
```

### 4. Configure Environment
Copy `.env.example` to `.env` (already created with default values for local SQLite demo):
```bash
cp .env.example .env
```

### 5. Setup Database & Seed Initial Data
```bash
npx prisma db push
node prisma/seed.js
```
*This instantly generates the database and populates 1 Admin, 2 Staff, 3 Hospitals, 16 Donors, 50+ blood units across all 8 blood groups, 12 hospital requests, notifications, and audit logs.*

### 6. Start the Development Server
```bash
npm run dev
```

Visit **`http://localhost:3000`** in your browser.

---

## 🌐 Production Deployment (PostgreSQL + Vercel / Railway / Neon)

To deploy to production with PostgreSQL:

1. In `prisma/schema.prisma`, update the datasource or replace it with `prisma/schema.postgresql.prisma`:
   ```prisma
   datasource db {
     provider = "postgresql"
     url      = env("DATABASE_URL")
   }
   ```
2. Set your environment variables in Vercel / Railway:
   - `DATABASE_URL`: Your PostgreSQL connection string (Neon, Supabase, Railway, etc.)
   - `NEXTAUTH_URL`: `https://your-domain.com`
   - `NEXTAUTH_SECRET`: A secure 32+ character random string
3. Run the migrations and seed script:
   ```bash
   npx prisma db push
   node prisma/seed.js
   ```

---

## 🔒 Security Best Practices Implemented

- **Server-Side Authorization**: API routes verify session token and role before performing any mutation (`requireRole(["BLOOD_BANK_STAFF", "ADMIN"])`).
- **IDOR Prevention**: Hospital users cannot read or update other hospitals' requisitions. Donors cannot view other donors' appointments.
- **Double-Allocation Prevention**: Atomic transactions (`prisma.$transaction`) ensure units cannot be claimed by concurrent requisitions.
- **HIPAA Data Masking**: Hospital partners see aggregate counts and donor blood groups without identifying names or phone numbers.
- **Audit Logging**: Sensitive events (logins, stock movements, allocations, user edits) generate immutable audit records with actor timestamps.

---

## 📄 License
This project is licensed under the MIT License.

