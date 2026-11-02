# Shubham Karoti Maitreyi Gurukula — Child Journey Tracking System

> **A compassionate, longitudinal, mobile-first holistic developmental tracking system for Gurukula education.**
> *"शुभं करोति कल्याणमारोग्यं धनसंपदा | शत्रुबुद्धि-विनाशाय दीपज्योतिर्नमोऽस्तु ते ||"*

---

## 1. Project Vision

Traditional Gurukula education educates the whole child—heart, intellect, character, hands, and soul. An education must never be reduced to mere numeric marks and attendance percentages.

For any child in the Gurukula, authorized staff and Acharyas can instantly answer:
- **Who is this child?** (Her origins, family heritage, Samskaras, and vernacular identity)
- **Where is she in her journey?** (Her grade, varga, and developmental stage)
- **How is she developing?** (Vedic recitation, classical music, mathematics, nature science, and practical crafts)
- **What are her unique strengths?** (Innate virtues, leadership, empathy, and artistic gifts)
- **What needs gentle attention?** (Areas where supportive guidance can help without stigmatizing labels)
- **What support has been provided?** (Trackable corrective action plans with assigned mentors)
- **How has she progressed over time?** (Preserving historical evidence across years without ever overwriting prior grade data)

---

## 2. Technology Stack

- **Frontend**: React 18, TypeScript, Vite, Tailwind CSS, Lucide Icons, Recharts
- **Backend**: Node.js, Express, TypeScript, Zod, Multer, PDFKit, bcryptjs, jsonwebtoken, cookie-parser
- **Database**: MongoDB 8.x with Mongoose ODM (normalized, indexed collections)
- **Authentication**: Secure server-side sessions via `HttpOnly` cookies + Bearer token fallback for API clients
- **Mobile First**: Android-friendly Progressive Web App (PWA) with installability, thumb-friendly bottom navigation, floating quick observation action, and offline app-shell caching
- **Architecture**: Monorepo with distinct `client/` and `server/` codebases, zero third-party paid dependency requirements

---

## 3. User Roles & Permission Matrix

| Module / Privilege | System Admin (`admin`) | Admissions Coord (`admissions`) | Academic Coord (`coordinator`) | Teacher / Mentor (`teacher`) | Management / Trustee (`management`) |
|---|---|---|---|---|---|
| **Default Login** | `admin@gurukula.org` | `admissions@gurukula.org` | `coordinator@gurukula.org` | `teacher@gurukula.org` | `trustee@gurukula.org` |
| **Password** | `GurukulaAdmin2026!` | `GurukulaAdmissions2026!` | `GurukulaCoord2026!` | `GurukulaTeacher2026!` | `GurukulaTrustee2026!` |
| **Admissions Pipeline** | Full Access | Full CRUD & Convert | Review & Assess | View Assigned Candidates | Aggregate Stats Only |
| **Candidate Conversion** | Authorized | Authorized | Authorized | No | No |
| **Children Directory** | Full Access | View Converted | Full Access | Assigned Classes/Children | Directory (No sensitive logs) |
| **Academic Progress** | Full Access | Read Only | Full Access | Qualitative Rubrics Entry | Aggregate Academic Trends |
| **Skills & Talents** | Full Access | Read Only | Full Access | Full Entry & Update | Aggregate Skill Domains |
| **Responsibilities & Seva** | Full Access | Read Only | Full Access | Assign & Assess | Aggregate Participation |
| **Achievements** | Full Access | Read Only | Full Access | Record Honors | Institutional Trends |
| **Quick Observations** | Full Access | Read Only | Full Access | High-Priority 10s Mobile Flow | Aggregate Counts |
| **Follow-ups & Actions** | Full Access | Admissions Only | Full Workload Management | Manage Assigned Actions | Workload Metrics Only |
| **Health & Well-being** | Audit Only | Restricted | Authorized View & Entry | Authorized with Permission | **Restricted / Hidden** |
| **Document Vault** | Full CRUD | Candidate Docs | Full Access | Class Uploads | Read Only |
| **Signature Timeline** | Full Access | Full Access | Full Access | Full Access | Public Milestone Cards |
| **Institutional Reports** | Full Access | Admissions CSV | Full Reports | Class Journey Summaries | Institutional PDF & CSV |
| **System Admin & Audit** | **Full Access** | No Access | No Access | No Access | Read Only Audit |

---

## 4. Key Architectural Implementations

### A. Admissions Management & Candidate Conversion Engine
- An applicant is **never** automatically an enrolled child.
- Supports the exact 8 specified admission statuses:
  1. `Application Received`
  2. `Selected for Interview`
  3. `Invited`
  4. `Selected as Student`
  5. `Not Interested`
  6. `Not Eligible`
  7. `Admission Not Taken`
  8. `To be Followed Up Later`
- **Conversion to Enrolled Student**: Requires an authorized explicit action after joining.
  - Automatically generates unique permanent student identifier (e.g. `SKMG-2026-001`).
  - Creates linked `Child` profile with immutable `applicantId` reference.
  - Automatically provisions `Guardian` profile from candidate contact information.
  - Creates initial `EnrollmentHistory` entry for the assigned starting grade.
  - Prevents duplicate conversions and preserves all interview and assessment history.

### B. Complete Child Profile (12 Tabs)
1. **Overview**: Current grade, permanent student ID, summary badges, key strengths.
2. **Personal Details**: Demographic information, vernacular names, date of birth, blood group.
3. **Family & Guardians**: Parent contacts, occupation, talents, skills, and Seva contribution willingness.
4. **Academic Journey**: Year-wise progression and descriptive qualitative rubrics (`Pravesha`, `Abhyasa`, `Praveena`, `Visharada`).
5. **Skills & Talents**: 10 traditional domains (Vedic Chanting, Carnatic Music, Classical Dance, Tarka/Debate, Krishi/Organic Farming, Hastakala, Yoga, Kalaripayattu, Paaka-kala). Evaluated by stages: *Emerging, Developing, Consistent, Strong*.
6. **Responsibilities**: Campus Seva roles (Kitchen Incharge, Morning Bell, Prayer Anchor, Goshala Lead) with learning outcomes.
7. **Achievements**: State, District, and National honors, certifications, and citations.
8. **Health & Well-being**: Strictly protected module. Non-medicalized, supportive observations on sleep, seasonal routines, and traditional Ayurvedic nourishment.
9. **Mentor Observations**: Reverse chronological stream of quick mobile notes.
10. **Corrective Actions / Follow-ups**: Action plans, due dates, review notes, and outcome resolutions.
11. **Documents**: Server-managed protected document vault for birth certificates, photos, and award citations.
12. **Full Journey Timeline**: Signature longitudinal past-to-present developmental continuum.

### C. Teacher Mobile Quick Observations (10-Second Flow)
- Tap the center floating action button (+) on phone.
- Select child from searchable dropdown.
- Select observation category (Academics, Skills, Participation, Responsibility, Well-being, Strength, Concern, Other).
- Tap quick tags (Vedic Chanting, Compassion, Morning Prayer, Focus, etc.).
- Dictate or type short note with optional voice recognition (Speech-to-Text).
- Auto-saves draft in localStorage so teachers never lose an interrupted thought.
- Enforces authenticated `authorId` server-side with zero impersonation.
- Optional toggle: automatically spawns a linked `FollowUp` support action item.

### D. Multi-Year Grade History Preservation
- When a child is promoted to a higher grade or next academic year:
  - Prior grade records in `EnrollmentHistory` are marked `isReadOnlyHistory: true`.
  - Prior academic term evaluations, achievements, and observations remain permanent evidence.
  - Prior data is never overwritten.

---

## 5. Quick Start & Installation

### Prerequisites
- Node.js v18+ (tested on Node v25.9.0)
- MongoDB Community or Enterprise running locally on port 27017 or remote URI

### 1. Clone & Setup
```bash
# Navigate to project directory
cd "c:\gurukula app BBH"

# Install backend dependencies
npm install --prefix server

# Install frontend dependencies
npm install --prefix client
```

### 2. Configure Environment
Create `.env` inside `server/` (or copy from `.env.example`):
```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/gurukula_db
JWT_SECRET=gurukula-sacred-journey-secure-jwt-secret-2026-production-grade
CORS_ORIGIN=http://localhost:5173
```

### 3. Seed Database
Initialize with realistic, pre-configured Gurukula demo data across all roles:
```bash
npm run seed --prefix server
```

### 4. Run Development Servers
In terminal 1 (Backend API):
```bash
npm run dev --prefix server
```
*Backend runs on `http://localhost:5000/api/v1`*

In terminal 2 (Frontend Client):
```bash
npm run dev --prefix client
```
*Frontend runs on `http://localhost:5173`*

---

## 6. Running Tests

The test suite covers authentication, RBAC authorization, applicant-to-student conversion, grade history preservation, teacher quick observations, follow-ups, and the signature journey timeline aggregator:

```bash
npm test --prefix server
```

---

## 7. API Reference (`/api/v1`)

### Authentication & Sessions
- `POST /auth/login` — Log in staff account, issues HttpOnly cookie and JWT token
- `POST /auth/logout` — Clear session
- `GET /auth/me` — Current authenticated user profile
- `PUT /auth/change-password` — Change password

### Admissions
- `GET /admissions` — Search, filter by status/grade, status counts
- `POST /admissions` — Create candidate application (generates `APP-YYYY-XXX`)
- `GET /admissions/:id` — Application details & interview history
- `PUT /admissions/:id` — Update status, staff notes, and follow-up date
- `POST /admissions/:id/interview` — Record interview evaluation
- `POST /admissions/:id/convert` — **Explicit conversion of candidate to enrolled student**

### Children & Profiles
- `GET /children` — Directory of active or alumni students with filters
- `GET /children/:id` — Master profile with related counts and permissions
- `POST /children` — Direct enrollment
- `PUT /children/:id` — Update details
- `POST /children/:id/promote` — Promote child with historical preservation
- `POST /children/:id/transition-alumni` — Transition graduate to alumni
- `GET /children/:id/timeline` — **Signature multi-year developmental timeline**

### Observations & Follow-ups
- `GET /observations` — Recent observation stream
- `POST /observations` — Quick observation entry (server-enforced authorId)
- `GET /followups` — Support actions with workload metrics (`dueToday`, `overdue`, `upcoming`, `completed`)
- `POST /followups` — Create support action
- `PUT /followups/:id` — Update status, review notes, or complete action

### Academic, Skills, Responsibilities & Health
- `GET /academic/years` & `POST /academic/years`
- `GET /academic/grades` & `POST /academic/grades`
- `GET /academic/subjects` & `POST /academic/subjects`
- `GET /academic/progress/:childId` & `POST /academic/progress`
- `GET /skills/:childId` & `POST /skills` & `PUT /skills/:id`
- `GET /responsibilities/:childId` & `POST /responsibilities`
- `GET /achievements/:childId` & `POST /achievements`
- `GET /health/:childId` & `POST /health` *(Permission Restricted)*
- `GET /family/:childId` & `POST /family/seva`

### Documents & Reports
- `POST /documents/upload` — Multer upload to protected storage
- `GET /documents/download/:id` — Authenticated document streaming
- `GET /reports/child-journey/:childId/pdf` — **Printable PDF Child Journey Report**
- `GET /reports/followups/csv` — CSV data export of support actions
- `GET /reports/admissions/csv` — CSV data export of candidate pipeline

---

## 7. Development & Deployment

### Local Development
1. Install dependencies across server and client:
   ```bash
   npm run install:all
   ```
2. Start the backend server (runs at `http://localhost:5000`):
   ```bash
   npm run dev:server
   ```
3. Start the frontend client (runs at `http://localhost:5173`):
   ```bash
   npm run dev:client
   ```
4. Run automated test suite:
   ```bash
   npm test
   ```

### 🚀 Deploying to Vercel
The system is configured for single-click deployment to **Vercel** with full serverless Express API support and static Vite frontend distribution:
- Root serverless function: `api/index.ts`
- Routing & Headers: `vercel.json`
- Mongoose connection pooling & serverless warm caching
- Auto-seeding on fresh database deployments
- Refer to [VERCEL_DEPLOYMENT.md](file:///c:/gurukula%20app%20BBH/VERCEL_DEPLOYMENT.md) for step-by-step setup instructions and environment variables.

---

## 8. Mobile PWA & Capacitor Deployment

- The web application includes a full W3C Web App Manifest (`manifest.json`) and service worker (`sw.js`).
- Can be installed directly on Android or iOS via "Add to Home Screen".
- To wrap as a native Android APK using Capacitor:
  ```bash
  cd client
  npm install @capacitor/core @capacitor/cli @capacitor/android
  npx cap init "Maitreyi Gurukula" "org.gurukula.journey" --web-dir dist
  npm run build
  npx cap add android
  npx cap copy
  npx cap open android
  ```

---

## 9. Security & Data Protection Architecture

- **Role-Based & Record-Level Authorization**: Server-enforced middleware on every route.
- **Privacy by Default**: Health and emotional well-being notes are barred from general exports and protected by separate permissions.
- **No Data Loss on Promotions**: Enrollment histories are marked immutable read-only records upon advancement.
- **Audit Logging**: Sensitive access, admissions conversion, and password updates are permanently recorded in `AuditLog`.
- **Zero Third-Party Vendor Locks**: System runs completely self-hosted without requiring paid cloud APIs.
