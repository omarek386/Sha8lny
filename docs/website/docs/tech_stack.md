# 🛠️ Tech Stack & UI Libraries

This document details the frontend framework, styling libraries, visualization tools, and third-party UI dependencies powering the **Sha8alny B2B Portal**.

---

## 1. Core Framework & Build Infrastructure

The application runs on a modern, typed Single-Page Application (SPA) architecture:

*   **React 18**: Utilizing state hooks, context providers, and component lifecycles for interactive user interfaces.
*   **Vite**: The build engine and local development server, providing hot module replacement (HMR), tree-shaking, and minified production outputs.
*   **TypeScript**: Implements strict compile-time types for database schemas, React components, and routing parameters.

---

## 2. Design System & CSS Libraries

The visual identity is based on a dark-mode-ready, flat, and spacious UI layout.

*   **Tailwind CSS**: A utility-first styling library used for layout styling, padding, grids, and flexboxes.
*   **shadcn/ui**: Built on top of **Radix UI Primitives**, shadcn/ui provides pre-styled, accessible components:
    *   *Data Input*: `Button`, `Input`, `Label`, `Select`, `Checkbox`, `RadioGroup`, `Slider`, `Switch`.
    *   *Layout & Structure*: `Card`, `Accordion`, `Tabs`, `Separator`, `ScrollArea`, `ResizablePanels`.
    *   *Overlays & Modals*: `Dialog` (for creation modals and details view), `AlertDialog` (for deletions and alerts), `Popover`, `Tooltip`.
    *   *Navigation*: `NavigationMenu`, `Menubar`, `DropdownMenu`.
*   **Lucide React**: Vector icon library providing system iconography (e.g. `Home`, `Users`, `MessageSquare`, `Settings`, `FolderKanban`, etc.).
*   **Tailwind CSS Animate**: Adds smooth keyframe transitions (skeletons, spin states, dialog fade-ins).

---

## 3. Data Visualization & Charting

To represent dashboard analytics for both Admin overview metrics and Company dashboard reviews, the app relies on **Recharts**:

*   **`<ResponsiveContainer>`**: Ensures charts scale dynamically on mobile screens and desktop sidebars.
*   **`<PieChart>` & `<Pie>`**: Rendered on company dashboards to show ongoing vs completed project distributions. Custom `Cell` overrides map Tailwind color tokens to the segments.
*   **`<BarChart>` & `<Bar>`**: Used in both Admin and Company portals to illustrate student skill concentrations, mapping top skills.

---

## 4. Forms & Schema Validation

To guarantee data integrity and simplify forms state (e.g., posting opportunities, registration, updating settings):

*   **React Hook Form (`react-hook-form`)**: Handles form control state, reducing re-renders by binding directly to inputs.
*   **Zod (`zod`)**: A schema declaration and validation library. The B2B portal declares schemas for:
    *   *User registration/login* (email patterns, password lengths).
    *   *Opportunities* (deadline dates, reward amount limits).
    *   *Profile completion* (validating website URLs, descriptions, and categories).
*   **Hook Form Resolvers (`@hookform/resolvers`)**: Binds Zod schemas to React Hook Form, automatically intercepting form submissions and rendering error messages under invalid inputs.

---

## 5. Auxiliary Dependencies

*   **Sonner**: Renders elegant, custom toast notifications in response to actions (e.g., successful project creations, login failures, profile update alerts).
*   **Embla Carousel**: Powering sliders and project carousels.
*   **Date-Fns**: Provides helper utilities for formatting timestamps and deadlines.
*   **React Day Picker**: Powering date inputs for opportunity deadlines.
