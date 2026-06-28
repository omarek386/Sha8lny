# 💼 Core Modules & Dashboards

This document provides a comprehensive breakdown of the core modules and functional features built into the B2B portal of the **Sha8alny** platform, mapping details from both the **Company Workspace** and **Admin Control Center**.

---

## 1. Company Portal Features

The Company Portal is designed for corporate managers to publish training/project opportunities, search for student candidates, manage applications, and coordinate payments.

```
                  ┌─────────────────────────────────┐
                  │         Company Portal          │
                  └───────────────┬─────────────────┘
      ┌───────────────────────────┼───────────────────────────┐
      ▼                           ▼                           ▼
┌───────────┐               ┌───────────┐               ┌───────────┐
│ Analytics │               │ Projects  │               │ Candidates│
│ Dashboard │               │ & Opps    │               │ & Message │
└───────────┘               └───────────┘               └───────────┘
```

### A. Dashboard Analytics & Statistics (`Dashboard.tsx`)
*   **Key Indicators**: Displays four high-level metrics cards:
    1.  *Total Students*: Platform-wide student counts.
    2.  *Completed Projects*: Number of successful projects matching the company.
    3.  *Opportunities*: Active opportunities posted by this company profile.
    4.  *Total Projects*: Active assignments currently under contract.
*   **Recent Applications Feed**: Lists the five most recent student applications, each featuring a dynamic `StatusBadge` (e.g., `pending`, `accepted`, `ongoing`).
*   **Recharts Data Visualizations**:
    *   *Projects Progress (Pie Chart)*: Shows a percentage breakdown of ongoing, draft, complete, and failed projects.
    *   *Top Insights (Bar Chart)*: Illustrates skill-demand distributions by aggregation of student skills.

### B. Opportunity & Project Management (`Projects.tsx`)
*   **Creation Wizard (`AddProjectDialog.tsx`)**: Allows companies to post opportunities with fields for:
    *   Title, Description, and Category Selection.
    *   Type (e.g., "Internship Remote", "Job Hybrid").
    *   Remuneration (toggle between Unpaid/Paid and inputting the reward amount).
    *   Milestone/Module Breakdown: Allows decomposing opportunities into distinct milestones with specific titles, descriptions, and duration metrics.
*   **Active Directory Grid**: Lists opportunities with cards showcasing deadlines, prices, and requirements tags.

### C. Candidate Search & Profiles (`Students.tsx`)
*   **Interactive Grid**: High-fidelity student catalog listing students' names, graduation years, majors, and registration dates.
*   **Searching & Filtering**: Live search by name and major.
*   **Student Details Modal (`StudentDetailsDialog.tsx`)**: Renders candidate information, including university credentials, CV links, GitHub profile connections, and skills tags. Shows their past opportunity histories.

### D. Applicant Tracking (`Applicants.tsx`)
*   **Evaluation Funnel**: Displays applications with proposals, applicant stats, and CV links.
*   **Applicant Details Modal (`ApplicationDetailsDialog.tsx`)**: Enables hiring managers to view the student's submission letter and execute state transitions (accepting, rejecting, or advancing applications to `ongoing`).

### E. Direct Chat Communication (`Messages.tsx`)
*   **Messaging Shell**: Dual-pane chat layout showing chat channels on the left and active messages on the right.
*   **Messaging Feed**: Displays timestamps, message state indicators, and sender details. Allows typing message content.

### F. Ledgers & Payments (`Payment.tsx`)
*   **Transactional Grid**: Comprehensive financial ledger tracking past payments, billing amounts, currencies, transaction types (e.g., upfront, milestone), and statuses (e.g., success, pending, failed).

---

## 2. Admin Portal Features

The Admin Control Center is reserved for platform administrators to regulate platform status, inspect and approve student credentials, coordinate announcements, and edit core parameters.

```
                  ┌─────────────────────────────────┐
                  │          Admin Portal           │
                  └───────────────┬─────────────────┘
      ┌───────────────────────────┼───────────────────────────┐
      ▼                           ▼                           ▼
┌───────────┐               ┌───────────┐               ┌───────────┐
│ Global    │               │ Training  │               │ System    │
│ Analytics │               │ Audits    │               │ Config    │
└───────────┘               └───────────┘               └───────────┘
```

### A. Global Control Dashboard (`AdminIndex.tsx`)
*   **Global Health Stats**: Analytical card displays tracking global student numbers, total completed contracts, active opportunities, and all assignments.
*   **System Integrity Monitors**: Includes a `SupabaseStatus` widget verifying direct API and database connection states.

### B. Student Management (`AdminTablePage.tsx`)
*   **Full Registry Audits**: Comprehensive table of all student entries, showing details, majors, and universities, allowing administrators to audit student accounts.

### C. Training Verification Pipeline (`AdminTrainingSubmissionsPage.tsx`)
*   **Verifications Queue**: List of student certificates and reports waiting for verification.
*   **Evaluation Actions**:
    *   Administrators view submitted training certificates, company reviews, survey forms, and presentations.
    *   Provides two actions: **Approve** (commits the opportunity duration to the student's completed training hours) or **Reject** (prompts for feedback notes explaining why the submission was refused).

### D. Announcement Pipeline (`AdminAnnouncementPage.tsx`, `AdminAddAnnouncementPage.tsx`)
*   **Announcement Editor**: Rich form tool to publish platform news alerts, supporting titles, text bodies, reference links, and image cover URLs.
*   **Feed Manager**: Grid of announcements showing publish dates and editing options.

### E. Global Platform Settings (`AdminSettingsPage.tsx`)
*   **Maintenance Operations**: Controls the system state:
    *   *Maintenance Mode Toggle*: Locks down frontend operations.
    *   *Custom Message Editor*: Configures warning text displayed to users during down-times.
    *   *Version Controls*: Declares minimum supported client application versions.
