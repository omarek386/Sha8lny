# 📂 Folder Structure Blueprint

This document details the folder structure and organization of the **Sha8alny B2B Portal** frontend codebase, illustrating a clean separation of views, components, utilities, and integrations.

---

## 1. Directory Tree Overview

```text
company-website-made/
├── docs/                      # Technical system documentation (Showcase files)
├── public/                    # Static public assets (logos, favicon)
├── supabase/                  # Supabase migrations, seed files, and configuration
├── src/                       # Main application source code
│   ├── components/            # UI components, layout sheets, and route guards
│   │   ├── admin/             # Admin-specific component layout widgets and insights charts
│   │   ├── layout/            # General layout blocks (Sidebar, Header, Dashboard wrapper)
│   │   ├── ui/                # atomic components (buttons, cards, inputs)
│   │   ├── RequireAdminAuth.tsx            # Admin route protection guard
│   │   └── RequireProfileCompletion.tsx    # Company profile completion guard
│   ├── contexts/              # Global React context providers
│   │   └── ProfileContext.tsx # Authentication session state and profile context
│   ├── hooks/                 # Custom React hooks (Shadcn hooks)
│   ├── integrations/          # Database client connectors
│   │   └── supabase/          # Supabase client initializer and TS schemas
│   ├── lib/                   # Internal utilities
│   │   └── utils.ts           # Classnames merger helper (cn)
│   ├── pages/                 # Full Page routing views
│   │   ├── admin/             # Administrator dashboard pages
│   │   │   ├── AdminAddAnnouncementPage.tsx # Publish or edit announcements
│   │   │   ├── AdminAnnouncementPage.tsx    # List and manage announcements
│   │   │   ├── AdminInboxPage.tsx           # Admin system inbox
│   │   │   ├── AdminIndex.tsx               # Admin main overview dashboard
│   │   │   ├── AdminOpportunitiesPage.tsx   # Global opportunities table
│   │   │   ├── AdminProfilePage.tsx         # Admin profile updates
│   │   │   ├── AdminSettingsPage.tsx        # System configuration page
│   │   │   ├── AdminTablePage.tsx           # Candidate audit table
│   │   │   ├── AdminTrainingSubmissionsPage.tsx # Approve student certifications
│   │   │   └── AdminViewApplicantPage.tsx   # Detailed applicant profile review
│   │   ├── Applicants.tsx     # Company applicants tracking page
│   │   ├── Auth.tsx           # Gateway login & sign-up page
│   │   ├── Dashboard.tsx      # B2B dashboard statistics & charts
│   │   ├── Home.tsx           # Company dashboard home view
│   │   ├── Messages.tsx       # B2B direct messaging client
│   │   ├── NotFound.tsx       # 404 fallback page
│   │   ├── Payment.tsx        # Financial transaction ledgers page
│   │   ├── Profile.tsx        # Company profile setup page
│   │   ├── Projects.tsx       # Post/manage opportunities page
│   │   ├── Settings.tsx       # User account parameters page
│   │   └── Students.tsx       # Company candidate search database
│   ├── types/                 # TypeScript type interface definitions
│   ├── App.css                # Global styling properties override
│   ├── App.tsx                # App entrypoint and React Router configurations
│   ├── index.css              # Baseline Tailwind CSS definitions
│   └── main.tsx               # Client execution bootstrap file
├── eslint.config.js           # Linter properties
├── index.html                 # Main template index file
├── package.json               # Node script list and dependency manager
├── postcss.config.js          # Tailwind CSS support configuration
├── tailwind.config.ts         # Design system tokens configuration
├── tsconfig.json              # TypeScript compilation setup
└── vite.config.ts             # Vite server builder options
```

---

## 2. Directory Purpose Breakdowns

### `/src/components`
Houses the interface components of the B2B portal.
*   **`admin/`**: High-level visual panels for administrators (charts, notifications grids, and statistics metrics).
*   **`layout/`**: Core shells defining sidebar menus and page margins for both Company dashboard layouts and general layouts.
*   **`ui/`**: Low-level, atomic user-interaction components (buttons, input boxes, badges) styled with Tailwind CSS utility classes.
*   **Guards**: Components wrapping page elements inside `App.tsx` routes, programmatically redirecting users depending on authentication and role parameters.

### `/src/contexts`
Stores global React context providers. The most critical is `ProfileContext.tsx`, which exposes active session information, performs user role checks, and exposes profile save hooks to all rendering widgets.

### `/src/integrations`
Manages connections to remote services. Includes `supabase/` which holds:
*   `client.ts`: The singleton database connection configuration.
*   `types.ts`: TypeScript mappings generated directly from public tables, ensuring type-safety when executing queries.

### `/src/pages`
Contains screen-level page layouts which are mapped to specific routes inside the SPA routing system. The `admin/` subdirectory separates pages visible exclusively to platform administrators.
