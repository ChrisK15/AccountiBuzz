# AccountiBuzz - Requirements Engineering - 1


---

## 1. Business Objective Model
```mermaid
flowchart TD
    subgraph Problems["Business Problems"]
        BP1["BP1: Unverified self-reporting —members can fake or skip without consequence"]
        BP2["BP2: No daily engagement loop;users only open app to submit"]
        BP3["BP3: Punitive streak designcauses immediate abandonment"]
        BP4["BP4: Fitness-only tools excludeknowledge workers and wellness users"]
        BP5["BP5: No verification = no trust;free-riding goes undetected"]
    end

    subgraph Objectives["Business Objectives"]
        BO1["BO1: Verified accountability platform≥80% of groups submit within 48h of creation"]
        BO2["BO2: Daily active engagement≥70% of members open app on submission days"]
        BO3["BO3: 30-day retention≥60% retention at 30 days"]
        BO4["BO4: Diverse use cases at launchAll 3 proof types functional"]
        BO5["BO5: Trust as core brand value≥90% admin reviews completed within 24h"]
    end

    BP1 --> BO1
    BP2 --> BO2
    BP3 --> BO3
    BP4 --> BO4
    BP5 --> BO5

    BO1 & BO2 & BO3 & BO4 & BO5 --> PC(["AccountiBuzzMobile Accountability Platform"])

    PC --> F1["Group Mode Toggle(Competitive / Collaborative)"]
    PC --> F2["Video ProofSubmission"]
    PC --> F3["Streak & GraceSystem"]
    PC --> F4["Admin Verification& Review"]
    PC --> F5["Push NotificationEngine"]
```

---

## 2. Context Diagram
**Key:** Rectangles = External Entities | Oval = System | Arrows = Data Flows

```mermaid
flowchart LR
    subgraph Humans["Human Actors"]
        GA["Group Admin"]
        GM["Group Member"]
        DEV["Mobile Device(Camera / Gallery)"]
    end

    ABS(["AccountiBuzzSystem"])

    subgraph FireSvc["Firebase Services"]
        AUTH["FirebaseAuthentication"]
        FDB["FirebaseFirestore"]
        FS["FirebaseStorage"]
        FCM["Firebase CloudMessaging"]
    end

    GA -->|"group config, review decisions"| ABS
    ABS -->|"review queue, confirmations, alerts"| GA

    GM -->|"video / photo proof, reports"| ABS
    ABS -->|"notifications, streak status"| GM

    DEV -->|"video stream / gallery file"| ABS

    ABS <-->|"auth tokens"| AUTH
    ABS <-->|"data read / write"| FDB
    ABS <-->|"video upload / signed URLs"| FS
    ABS <-->|"notification payloads"| FCM
```

---

## 3. Ecosystem Map
```mermaid
flowchart LR
    APP["AccountiBuzz Mobile App (React Native)"]

    subgraph Firebase ["Firebase Platform"]
        AUTH["Firebase Authentication"]
        FDB["Firebase Firestore"]
        FS["Firebase Storage"]
        FCF["Firebase Cloud Functions"]
        FCM_SVC["Firebase Cloud Messaging Service"]
    end

    subgraph Push ["Push Delivery Layer"]
        APNS["Apple Push Notification Service (APNs)"]
        GCM["Google FCM (Android)"]
    end

    subgraph Devices ["End-User Devices"]
        IOS["iOS Device (Camera, Gallery, Notifications)"]
        AND["Android Device (Camera, Gallery, Notifications)"]
    end

    subgraph Stores ["App Distribution"]
        AAS["Apple App Store"]
        GPS["Google Play Store"]
    end

    TS["T&S Moderation Platform"]

    APP -->|"auth requests"| AUTH
    AUTH -->|"session tokens"| APP

    APP -->|"read / write data"| FDB
    FDB -->|"real-time listeners"| APP

    APP -->|"upload video / photo"| FS
    FS -->|"signed URLs"| APP

    FDB -->|"event triggers (deadline, inactivity, grace day)"| FCF
    FCF -->|"streak updates, auto-approvals, grace day writes"| FDB
    FCF -->|"notification payloads"| FCM_SVC

    FCM_SVC -->|"iOS notifications"| APNS
    FCM_SVC -->|"Android notifications"| GCM
    APNS -->|"push delivery"| IOS
    GCM -->|"push delivery"| AND

    AAS -->|"app install / update (iOS)"| IOS
    GPS -->|"app install / update (Android)"| AND

    APP -->|"escalated content reports"| TS
    TS -->|"moderation decisions"| FDB
```

---

## 4. Feature Tree
### Part A — Core Features

```mermaid
flowchart LR
    ABS(["AccountiBuzz"])

    ABS --> AUTH["Authentication& Profile"]
    ABS --> GRP["GroupManagement"]
    ABS --> PROOF["ProofSubmission"]
    ABS --> VERIFY["Admin Verification& Review"]

    AUTH --> A1["Email Registration& Sign-In"]
    AUTH --> A2["Profile Setup(Display Name)"]
    AUTH --> A3["Timezone Detection& Override"]

    GRP --> G1["Group Creation(Mode Selection)"]
    GRP --> G2["Invite LinkGeneration"]
    GRP --> G3["MemberJoin Flow"]
    GRP --> G4["Daily ChallengeSetup"]
    GRP --> G5["Co-AdminDesignation"]
    GRP --> G6["SabbaticalStatus"]
    GRP --> G7["Group ModeChange"]

    PROOF --> P1["In-App CameraRecording"]
    PROOF --> P2["Gallery / ScreenRecording Upload"]
    PROOF --> P3["Photo ProofUpload"]
    PROOF --> P4["Submission Timestampat Initiation"]
    PROOF --> P5["Offline Queue& Auto-Retry"]

    VERIFY --> V1["PendingReview Queue"]
    VERIFY --> V2["Approve Action→ Streak Credit"]
    VERIFY --> V3["Needs Resubmission+ Feedback"]
    VERIFY --> V4["Integrity Flag(Separate Log)"]
    VERIFY --> V5["Auto-ApprovalFallback (8h)"]
    VERIFY --> V6["Admin InactivityEscalation"]
```

### Part B — Engagement & Trust Features

```mermaid
flowchart LR
    ABS(["AccountiBuzz"])

    ABS --> STREAK["Streak & GraceSystem"]
    ABS --> BOARD["Leaderboard /Completion Board"]
    ABS --> NOTIF["Notifications"]
    
    STREAK --> S1["Streak CounterDisplay"]
    STREAK --> S2["Automatic Grace Day(1/week, no action)"]
    STREAK --> S3["Earned Streak Freeze(1 per 30-day cycle)"]
    STREAK --> S4["Deferred StreakFinalization"]
    STREAK --> S5["Technical GraceWindow (10 min)"]

    BOARD --> B1["Real-Time RankedLeaderboard(Competitive)"]
    BOARD --> B2["Completion Board(Collaborative)"]
    BOARD --> B3["Milestone Broadcasts(7 / 30 / 60-day)"]

    NOTIF --> N1["Daily SubmissionReminders (2×)"]
    NOTIF --> N2["Review ResultAlerts"]
    NOTIF --> N3["Accountability Intensity(Gentle / Firm)"]
    NOTIF --> N4["NotificationDeep-Links"]
    NOTIF --> N5["Admin ReviewReminders"]
```

---

## 5. External Events
- Member submits a video, screen recording, or photo proof.
- Member initiates a resubmission after receiving feedback.
- Member joins a group via invite link.
- Admin creates a new group and sets the group mode.
- Admin approves a pending submission.
- Admin requests resubmission with a feedback message.
- Admin flags a submission for a violation.
- Admin promotes a member to co-admin.
- Admin sends a broadcast message to the group.
---
