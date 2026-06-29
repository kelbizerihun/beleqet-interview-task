# Beleqet Complete API Reference

This document provides a comprehensive list of **all API endpoints** available in the backend, including HTTP methods, required authentication, and sample JSON payloads for each request. 

The base URL for all endpoints is `http://localhost:4000/api/v1`.  
Endpoints marked with 🔒 require an `Authorization: Bearer <accessToken>` header.

---

## 1. Auth Module (`/auth`)

### Register a new user
- **POST** `/auth/register`
- **Auth:** Public
- **Payload:**
```json
{
  "email": "user@domain.com",
  "firstName": "John",
  "lastName": "Doe",
  "password": "SecurePass123!",
  "role": "JOB_SEEKER" // Options: ADMIN, EMPLOYER, JOB_SEEKER, FREELANCER
}
```

### Login
- **POST** `/auth/login`
- **Auth:** Public
- **Payload:**
```json
{
  "email": "user@domain.com",
  "password": "SecurePass123!"
}
```

### Refresh Token
- **POST** `/auth/refresh`
- **Auth:** Public
- **Payload:**
```json
{
  "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Logout
- **POST** `/auth/logout`
- **Auth:** 🔒 Required
- **Payload:** *(Empty body)*

### Get Current User (Me)
- **GET** `/auth/me`
- **Auth:** 🔒 Required
- **Description:** Returns the currently authenticated user's session data.

### Verify Email
- **POST** `/auth/verify-email`
- **Auth:** Public
- **Payload:**
```json
{
  "token": "email-verification-token"
}
```

### Forgot Password
- **POST** `/auth/forgot-password`
- **Auth:** Public
- **Payload:**
```json
{
  "email": "user@domain.com"
}
```

### Reset Password
- **POST** `/auth/reset-password`
- **Auth:** Public
- **Payload:**
```json
{
  "token": "reset-token-received-via-email",
  "newPassword": "NewSecurePass123!"
}
```

---

## 2. Users Module (`/users`)

### Get Own Profile
- **GET** `/users/profile`
- **Auth:** 🔒 Required

### Update Profile
- **PATCH** `/users/profile`
- **Auth:** 🔒 Required
- **Payload (All fields optional):**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+251911234567",
  "headline": "Senior Developer",
  "bio": "Experienced in React and NestJS.",
  "location": "Addis Ababa",
  "skills": ["React", "TypeScript", "NestJS"],
  "linkedinUrl": "https://linkedin.com/in/john",
  "githubUrl": "https://github.com/john"
}
```

### Get Own Company
- **GET** `/users/company`
- **Auth:** 🔒 Required (EMPLOYER)

### Create/Update Company Profile
- **POST** `/users/company`
- **Auth:** 🔒 Required (EMPLOYER)
- **Payload:**
```json
{
  "name": "Tech Solutions PLC",
  "description": "Leading tech company.",
  "website": "https://techsolutions.com.et",
  "industry": "Software Development",
  "size": "50-100",
  "location": "Addis Ababa",
  "benefits": ["Remote work", "Bonus"],
  "foundedYear": 2018
}
```

### Get Notifications
- **GET** `/users/notifications`
- **Auth:** 🔒 Required

### Mark Notification as Read
- **PATCH** `/users/notifications/:id/read`
- **Auth:** 🔒 Required

---

## 3. Uploads Module (`/uploads`)

### Get Presigned S3 Upload URL
- **POST** `/uploads/presigned-url`
- **Auth:** 🔒 Required
- **Payload:**
```json
{
  "filename": "resume.pdf",
  "contentType": "application/pdf",
  "folder": "resumes"
}
```

---

## 4. Jobs Module (`/jobs`)

### List/Search Jobs
- **GET** `/jobs?q=developer&category=123&type=FULL_TIME&page=1&limit=20`
- **Auth:** Public

### Get Job Categories
- **GET** `/jobs/categories`
- **Auth:** Public

### Get Job by ID
- **GET** `/jobs/:id`
- **Auth:** Public

### Get Employer's Own Jobs
- **GET** `/jobs/my`
- **Auth:** 🔒 Required (EMPLOYER)

### Create a Job
- **POST** `/jobs`
- **Auth:** 🔒 Required (EMPLOYER)
- **Payload:**
```json
{
  "title": "Senior Frontend Developer",
  "description": "We are looking for an expert in React.",
  "requirements": "3+ years experience.",
  "location": "Addis Ababa",
  "type": "FULL_TIME", // FULL_TIME, PART_TIME, REMOTE, HYBRID, CONTRACT
  "categoryId": "category-uuid-here",
  "salaryMin": 30000,
  "salaryMax": 50000,
  "currency": "ETB",
  "tags": ["React", "Frontend"],
  "vacancies": 2,
  "experienceLevel": "Senior"
}
```

### Update a Job
- **PATCH** `/jobs/:id`
- **Auth:** 🔒 Required (EMPLOYER)
- **Payload (Partial of Create):**
```json
{
  "status": "PUBLISHED" // DRAFT, PUBLISHED, CLOSED
}
```

### Delete a Job
- **DELETE** `/jobs/:id`
- **Auth:** 🔒 Required (EMPLOYER)

---

## 5. Applications Module (`/applications`)

### Submit an Application
- **POST** `/applications`
- **Auth:** 🔒 Required (JOB_SEEKER)
- **Payload:**
```json
{
  "jobId": "job-uuid-here",
  "coverLetter": "I am writing to express my interest in this position. I have over 5 years of experience building scalable backend APIs using NestJS and PostgreSQL...",
  "resumeUrl": "https://s3.amazonaws.com/bucket/resume.pdf",
  "portfolioUrl": "https://portfolio.dev",
  "expectedSalary": 40000,
  "screeningAnswers": {
    "Why do you want this job?": "I love the tech stack."
  }
}
```

### Get My Applications (Job Seeker)
- **GET** `/applications/my`
- **Auth:** 🔒 Required (JOB_SEEKER)

### Get Applications for a Specific Job (Employer)
- **GET** `/applications/job/:jobId`
- **Auth:** 🔒 Required (EMPLOYER)

### Get Application Details
- **GET** `/applications/:id`
- **Auth:** 🔒 Required

### Update Application Status
- **PATCH** `/applications/:id/status`
- **Auth:** 🔒 Required (EMPLOYER)
- **Payload:**
```json
{
  "status": "SHORTLISTED" // SUBMITTED, SCREENING, SHORTLISTED, INTERVIEW_SCHEDULED, OFFERED, REJECTED
}
```

---

## 6. Freelance Module (`/freelance`)

### List/Search Freelance Gigs
- **GET** `/freelance/jobs`
- **Auth:** Public

### Get Gig by ID
- **GET** `/freelance/jobs/:id`
- **Auth:** Public

### Get My Bids (Freelancer)
- **GET** `/freelance/my-bids`
- **Auth:** 🔒 Required (FREELANCER)

### Get Contract Details
- **GET** `/freelance/contracts/:id`
- **Auth:** 🔒 Required

### Create a Freelance Gig
- **POST** `/freelance/jobs`
- **Auth:** 🔒 Required (EMPLOYER / CLIENT)
- **Payload:**
```json
{
  "title": "Build an E-commerce Mobile App",
  "description": "Need a Flutter dev.",
  "categoryId": "category-uuid-here",
  "budgetMin": 10000,
  "budgetMax": 20000,
  "deadlineDays": 30,
  "skills": ["Flutter", "Firebase"],
  "pricingType": "FIXED"
}
```

### Submit a Bid on a Gig
- **POST** `/freelance/jobs/:id/bids`
- **Auth:** 🔒 Required (FREELANCER)
- **Payload:**
```json
{
  "amount": 15000,
  "timelineDays": 25,
  "coverLetter": "I have built similar apps before. Check my portfolio..."
}
```

### Accept a Bid (Client)
- **PATCH** `/freelance/bids/:id/accept`
- **Auth:** 🔒 Required (CLIENT)
- **Payload:** *(Empty body)*

### Approve a Milestone (Client)
- **PATCH** `/freelance/milestones/:id/approve`
- **Auth:** 🔒 Required (CLIENT)
- **Payload:** *(Empty body)*

---

## 7. Escrow Module (`/escrow`)

### Initiate Escrow Funding
- **POST** `/escrow/initiate/:gigId`
- **Auth:** 🔒 Required (CLIENT)
- **Payload:** *(Empty body)*

### Escrow Webhook Callback (Internal)
- **POST** `/escrow/callback`
- **Auth:** Public (Chapa server callback)
- **Payload:** *(Chapa defined payload)*

### Release Milestone Payment
- **POST** `/escrow/milestones/:id/release`
- **Auth:** 🔒 Required (CLIENT)
- **Payload:** *(Empty body)*

---

## 8. Wallet Module (`/wallet`)

### Get Wallet Balance & Transactions
- **GET** `/wallet`
- **Auth:** 🔒 Required (FREELANCER)

### Request Withdrawal
- **POST** `/wallet/withdraw`
- **Auth:** 🔒 Required (FREELANCER)
- **Payload:**
```json
{
  "amount": 5000,
  "method": "CHAPA", // CHAPA, TELEBIRR, CBE_BIRR
  "accountRef": "0911234567"
}
```

---

## 9. Admin Module (`/admin`)

### List All Users
- **GET** `/admin/users`
- **Auth:** 🔒 Required (ADMIN)

### Suspend a User
- **PATCH** `/admin/users/:id/suspend`
- **Auth:** 🔒 Required (ADMIN)
- **Payload:** *(Empty body)*

### List All Disputes
- **GET** `/admin/escrow/disputes`
- **Auth:** 🔒 Required (ADMIN)

### Resolve a Dispute
- **PATCH** `/admin/disputes/:id/resolve`
- **Auth:** 🔒 Required (ADMIN)
- **Payload:**
```json
{
  "resolution": "Refunded the client because the work was not delivered as per contract."
}
```
