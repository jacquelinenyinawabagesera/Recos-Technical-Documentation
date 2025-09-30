# Q/A Process

## Testing

- **Unit Tests:** All backend logic covered with Django REST unit tests.
- **Frontend:** UI tested with manual and automated checks.
- **API:** All endpoints tested with Postman.

## Code Review

- All PRs reviewed on GitHub.
- Pre-merge checks for lint, tests, and build.

---

## Test Coverage

- Automated reports generated for backend code.
- Manual verification for integration flows.

---




# API Reference

Welcome to the **Recos API Reference**. Here you will find everything you need to integrate with, test, and explore the API endpoints that power the Recos platform.

---

## Live API Docs

- **Postman Online Docs:** [View Postman Documentation](https://documenter.getpostman.com/view/45699975/2sB3HqHJX4)
- **Swagger UI:** _Local Only (see screenshots below)_

---

## API Documentation Screenshots

## 📸 API Docs Visuals

| Swagger UI Example | Postman API Docs Example |
|--------------------|-------------------------|
| ![Swagger Screenshot 1](images/swagger1.png) | ![Postman Screenshot](images/swagger.png) |
| ![Swagger Screenshot 2](images/swagger2.png) |  |
| ![Swagger Screenshot 3](images/swagger3.png) |  |

---

---

## Main Endpoints

Below are the core API endpoints available in Recos. All endpoints follow REST conventions and require authentication unless noted.

### Authentication

<div class="api-block">
<pre class="api-dark">
POST   /register/           # Recruiter registration
POST   /login/              # Login
POST   /logout/             # Logout
</pre>
</div>

### Password Management

<div class="api-block">
<pre class="api-dark">
POST   /forgot-password/          # Start password reset
POST   /reset-password/           # Complete password reset
POST   /verify-reset-code/        # Verify reset code sent to email
</pre>
</div>

### Recruiter & Profile

<div class="api-block">
<pre class="api-dark">
GET    /users/                    # List all recruiters
PUT    /update-profile/           # Update your profile
DELETE /delete-account/           # Delete your account
</pre>
</div>

### Odoo Integration

<div class="api-block">
<pre class="api-dark">
POST   /verify-odoo/              # Verify Odoo account
POST   /odoo-credentials/         # Add Odoo credentials
GET    /odoo-credentials/list/    # List Odoo credentials
GET    /companies/                # List companies
GET    /companies/&lt;company_id&gt;/jobs/    # Get jobs by company
</pre>
</div>

### Jobs & Candidates

<div class="api-block">
<pre class="api-dark">
GET    /jobs/                     # List all jobs
GET    /jobs/&lt;job_id&gt;/candidates/ # Get candidates for a specific job
GET    /candidates/               # List all candidates
GET    /candidates/&lt;candidate_id&gt;/attachments/   # Candidate attachments
GET    /candidates/&lt;candidate_id&gt;/attachments/download/&lt;attachment_id&gt;/   # Download attachment
</pre>
</div>

### Sync Operations (Odoo Data)

<div class="api-block">
<pre class="api-dark">
POST   /sync/jobs/company/&lt;company_id&gt;/          # Sync jobs for a company
POST   /sync/jobs/handle-duplicates/             # Sync jobs & handle duplicates
POST   /sync/jobs/user/                          # Sync jobs for logged-in user
POST   /sync/candidates/job/&lt;job_id&gt;/            # Sync candidates for a job
POST   /sync/candidates/company/&lt;company_id&gt;/    # Sync candidates by company
POST   /sync/candidates/all/                     # Sync all candidates
POST   /sync/candidates/&lt;candidate_id&gt;/attachments/   # Sync candidate attachments
</pre>
</div>

### Interview Management

<div class="api-block">
<pre class="api-dark">
POST   /interviews/create/                                   # Create interview
POST   /interviews/&lt;interview_id&gt;/create-calendar-event/     # Add interview to Google Calendar
GET    /interviews/&lt;interview_id&gt;/analytics/                 # Get interview analytics (AI/Gemini)
</pre>
</div>

### AI Reports & Conversations

<div class="api-block">
<pre class="api-dark">
GET    /interview_conversations/     # List all interview conversations
GET    /ai-reports/                  # List all AI reports
</pre>
</div>

### Google Calendar & OAuth

<div class="api-block">
<pre class="api-dark">
GET    /auth/google/initiate/    # Start Google OAuth
GET    /auth/google/callback/    # Google OAuth callback
GET    /api/auth/google/callback/# API-level OAuth callback
</pre>
</div>

---

## Example API Usage

### Odoo Integration

- **Verify Odoo Account:**
<div class="api-block">
<pre class="api-dark">
POST /verify-odoo/
{
  "instance_url": "https://your-odoo.com",
  "email": "you@example.com",
  "api_key": "..."
}
</pre>
</div>

- **Fetch Jobs from Odoo:**
<div class="api-block">
<pre class="api-dark">
GET /odoo/jobs
</pre>
</div>

- **Fetch Candidates from Odoo:**
<div class="api-block">
<pre class="api-dark">
GET /odoo/candidates
</pre>
</div>

---

### Google Calendar / Meet Integration

- **Schedule Interview Event:**
<div class="api-block">
<pre class="api-dark">
POST /interviews/{interview_id}/create-calendar-event/
</pre>
</div>
- **Create Google Calendar Event:**
<div class="api-block">
<pre class="api-dark">
POST /calendar/events
</pre>
</div>
- **Get All Google Meet Links:**
<div class="api-block">
<pre class="api-dark">
GET /calendar/meets
</pre>
</div>

---

### AI, AssemblyAI, and Gemini

- **Start Interview with AI Assistant (transcription):**
<div class="api-block">
<pre class="api-dark">
POST /interviews/create/
</pre>
</div>
- **Get Post-Interview Analytics:**
<div class="api-block">
<pre class="api-dark">
GET /interviews/{interview_id}/analytics/
</pre>
</div>

---

### Attachments

- **Get Candidate Attachments:**
<div class="api-block">
<pre class="api-dark">
GET /candidates/{candidate_id}/attachments/
</pre>
</div>

- **Download Attachment:**
<div class="api-block">
<pre class="api-dark">
GET /candidates/{candidate_id}/attachments/download/{attachment_id}/
</pre>
</div>

---

## API Testing & Documentation

- **All APIs are tested in Postman.**
- See [Postman Docs](https://documenter.getpostman.com/view/45699975/2sB3HqHJX4) for live examples and sample payloads.
- See screenshots above for Swagger UI and Postman documentation examples.

---

## Security & Authentication

- Most endpoints require authentication via JWT or session.
- Store API keys and secrets in environment variables (`.env`), **never** commit secrets to your repository.

---

## More Information

- For a full list of all endpoints and models, see the [Developer Docs](developer-docs.md).
- For architecture and integration details, see [Developer Docs](developer-docs.md) and [Integrations](integrations.md).

---

> Have questions or need help? See our [Q/A Process](qa-process.md) or contact our team.

---

<style>
.api-block {
  background: #232f3e;
  border-radius: 10px;
  padding: 18px 18px 10px 18px;
  margin: 18px 0 24px 0;
  overflow-x: auto;
  font-size: 1.09em;
  font-family: 'Fira Mono', 'Consolas', 'monospace';
}
.api-dark {
  background: #232f3e !important;
  color: #fff !important;
  border-radius: 6px;
  padding: 12px 14px 8px 14px;
  margin: 0;
  font-size: 1em;
  font-family: 'Fira Mono', 'Consolas', 'monospace';
}
</style>