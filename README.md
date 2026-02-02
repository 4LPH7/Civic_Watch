#  Civic Watch
> **The People's Voice Platform**

![Next.js](https://img.shields.io/badge/Next.js-15-black) ![Supabase](https://img.shields.io/badge/Supabase-PostgreSQL-green) ![TypeScript](https://img.shields.io/badge/Language-TypeScript-blue) ![License](https://img.shields.io/badge/License-MIT-yellow)

**Civic Watch** is a Progressive Web Application (PWA) designed to empower citizens to report civic issues and allow officials to track and resolve them in real time.

Built for speed and security, it leverages the latest **Next.js 15** features, **Supabase** for backend-as-a-service, and **MapLibre GL JS** for high-performance open mapping on **OpenStreetMap** tiles.

---

## 🚀 Key Features

* **⚡ Modern Next.js 15 Stack:** Built on the App Router with **React 19**. Utilizes stable Server Actions for mutation handling, eliminating the need for separate API routes.
* **🗺️ High-Performance Open Mapping:** Powered by **MapLibre GL JS** (via `react-map-gl/maplibre`). Renders thousands of vector markers efficiently using WebGL, ensuring 60fps performance even with heavy datasets.
* **🔐 RBAC & Row-Level Security:** Strictly typed authentication via Supabase. Data access is governed by **Row-Level Security (RLS)** policies right at the database engine level—Citizens own their data; Admins manage status.
* **📍 Smart Reporting Engine:** Auto-detects user geolocation. Reports are categorized and risk-assessed. Forms are validated strictly using **Zod** + **React Hook Form**.
* **📡 Real-time Notifications:** Automated email triggers via **Resend** for high-risk reports or high-upvote thresholds.
* **🛡️ Admin Dashboard:** Dedicated secure route for officials to triage, filter, and resolve issues.

---

## 🛠️ Tech Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Framework** | Next.js 15 (App Router) | Server Components, Server Actions, React 19 |
| **Language** | TypeScript | Strict type safety across full stack |
| **Database** | Supabase (PostgreSQL) | Auth, Database, Storage, Realtime |
| **Maps** | MapLibre GL JS + OpenStreetMap | Open-source vector rendering, interactive markers |
| **Styling** | Tailwind CSS | Utility-first styling with `shadcn/ui` components |
| **Forms** | React Hook Form + Zod | Client-side validation & schema enforcement |
| **Email** | Resend SDK | Transactional emails for high-risk alerts |

---

## 🏗️ Project Structure

```bash
/
├── app/                  # Next.js 15 App Router
│   ├── (auth)/           # Authentication routes (Login/Signup)
│   ├── (dashboard)/      # Protected Admin Dashboard
│   ├── actions/          # Server Actions (Mutations: submit, resolve, notify)
│   ├── map/              # Main Citizen View (MapLibre + OSM integration)
│   ├── api/              # Route Handlers (Webhooks)
│   └── layout.tsx        # Root Layout
├── components/
│   ├── forms/            # Zod-validated forms (ReportIssueForm)
│   ├── map/              # MapLibre wrappers & Markers
│   └── ui/               # Reusable UI components (Shadcn)
├── lib/
│   └── supabase/         # Typed Supabase Clients (Server/Client)
├── types/                # Database definitions
├── database.sql          # SQL Schema & RLS Policies
└── public/               # Static assets


---
```
## ⚡ Getting Started

### 1. Clone & Install

```bash
git clone [https://github.com/4LPH7/Civic-Watch.git](https://github.com/4LPH7/Civic-Watch.git)
cd Civic-Watch
npm install

```

### 2. Environment Setup

Create a `.env.local` file in the root directory:

```bash
# Supabase Keys
NEXT_PUBLIC_SUPABASE_URL=your_project_url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
SUPABASE_SERVICE_ROLE_KEY=your_service_role_key

# Map tiles (OpenStreetMap / self-hosted)
NEXT_PUBLIC_MAP_TILES_URL=https://tile.openstreetmap.org/{z}/{x}/{y}.png

# Resend (Email)
RESEND_API_KEY=your_resend_api_key

```

### 3. Provision Database

Run the SQL commands found in `database.sql` inside your Supabase SQL Editor to set up tables and security policies.

**Critical Security Note:** ensure RLS is enabled.

```sql
-- Example Policy
CREATE POLICY "Public can view active reports" 
ON reports FOR SELECT 
USING (status != 'resolved');

```

### 4. Run Development Server

```bash
npm run dev

```

Visit `http://localhost:3000/map` to start reporting.

---

## 🧠 Technical Highlights & Decisions

* **Caching Strategy (Next.js 15):** We utilize `export dynamic = 'force-static'` where possible, but default to uncached fetching for the map data to ensure citizens see real-time updates.
* **Security First:** Logic is moved to the server wherever possible. Direct database interaction from the client is restricted to `SELECT` (read-only) operations permitted by RLS. All writes go through Server Actions to allow for extra server-side validation and rate limiting.
* **WebGL vs Leaflet:** We chose MapLibre/WebGL over Leaflet because the app aims to visualize potentially thousands of civic data points. Canvas/DOM-based rendering (Leaflet) creates bottlenecks at scale.

---

## 🔮 Roadmap

* [ ] **AI Analysis:** Integration with OpenAI/Gemini API to auto-triage report descriptions for "Urgency" and "Sentiment."
* [ ] **Media Storage:** Full implementation of Supabase Storage bucket for evidence photos.
* [ ] **Voting System:** Reddit-style upvoting to bubble up critical community issues.
* [ ] **PWA Offline Mode:** Service workers to allow reporting even with spotty internet connection.

---

## 🤝 Contributing

This is a civic-tech initiative. Issues and Pull Requests are welcome.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/UsefulFeature`)
3. Commit your Changes (`git commit -m 'Add some UsefulFeature'`)
4. Push to the Branch (`git push origin feature/UsefulFeature`)
5. Open a Pull Request
