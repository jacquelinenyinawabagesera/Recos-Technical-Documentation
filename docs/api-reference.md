# <span style="font-weight: 350; font-size: 2.1em; color: #232f3e">API Reference</span>

Welcome to the <strong>Recos API Reference</strong>. You will find everything you need to integrate with, test, and explore the API endpoints that power the Recos platform.

---

## <span style="font-weight: 300; font-size: 1.3em; color: #8645e8">Live API Docs</span>

- **Postman Online Docs:** [View Postman Documentation](https://documenter.getpostman.com/view/45699975/2sB3HqHJX4)
- **Swagger UI:**

---

## <span style="font-weight: 300; font-size: 1.3em; color: #8645e8">API Documentation Screenshots</span>

| <span style="font-weight:600">Swagger UI Example</span> | <span style="font-weight:600">Postman API Docs Example</span> |
|--------------------|-------------------------|
| ![Swagger Screenshot 1](images/swagger1.png) | ![Postman Screenshot](images/swagger.png) |
| ![Swagger Screenshot 2](images/swagger2.png) | ![Postman Screenshot](images/doc1.png) |
| ![Swagger Screenshot 3](images/swagger3.png) | ![Postman Screenshot](images/doc2.png) |

---

## <span style="font-weight: 300; font-size: 1.2em; color: #232f3e">Main Endpoints</span>

Below are the core API endpoints available in Recos. All endpoints follow REST conventions and require authentication.

### <span style="font-weight: 300; color: #8645e8;">Authentication</span>

<div class="api-block">
<pre class="api-dark">
POST   /register/           # Recruiter registration
POST   /login/              # Login
POST   /logout/             # Logout
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Password Management</span>

<div class="api-block">
<pre class="api-dark">
POST   /forgot-password/          # Start password reset
POST   /reset-password/           # Complete password reset
POST   /verify-reset-code/        # Verify reset code sent to email
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Recruiter & Profile</span>

<div class="api-block">
<pre class="api-dark">
GET    /users/                    # List all recruiters
PUT    /update-profile/           # Update your profile
DELETE /delete-account/           # Delete your account
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Odoo Integration</span>

<div class="api-block">
<pre class="api-dark">
POST   /verify-odoo/              # Verify Odoo account
POST   /odoo-credentials/         # Add Odoo credentials
GET    /odoo-credentials/list/    # List Odoo credentials
GET    /companies/                # List companies
GET    /companies/&lt;company_id&gt;/jobs/    # Get jobs by company
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Jobs & Candidates</span>

<div class="api-block">
<pre class="api-dark">
GET    /jobs/                     # List all jobs
GET    /jobs/&lt;job_id&gt;/candidates/ # Get candidates for a specific job
GET    /candidates/               # List all candidates
GET    /candidates/&lt;candidate_id&gt;/attachments/   # Candidate attachments
GET    /candidates/&lt;candidate_id&gt;/attachments/download/&lt;attachment_id&gt;/   # Download attachment
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Sync Operations (Odoo Data)</span>

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

### <span style="font-weight: 300; color: #8645e8;">Interview Management</span>

<div class="api-block">
<pre class="api-dark">
POST   /interviews/create/                                   # Create interview
POST   /interviews/&lt;interview_id&gt;/create-calendar-event/     # Add interview to Google Calendar
GET    /interviews/&lt;interview_id&gt;/analytics/                 # Get interview analytics (AI/Gemini)
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">AI Reports & Conversations</span>

<div class="api-block">
<pre class="api-dark">
GET    /interview_conversations/     # List all interview conversations
GET    /ai-reports/                  # List all AI reports
</pre>
</div>

### <span style="font-weight: 300; color: #8645e8;">Google Calendar & OAuth</span>

<div class="api-block">
<pre class="api-dark">
GET    /auth/google/initiate/    # Start Google OAuth
GET    /auth/google/callback/    # Google OAuth callback
GET    /api/auth/google/callback/# API-level OAuth callback
</pre>
</div>

---

## <span style="font-weight: 300; font-size: 1.2em; color: #232f3e">Example API Usage</span>

### <span style="font-weight: 300; color: #8645e8;">Odoo Integration</span>

- <strong>Verify Odoo Account:</strong>
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

- <strong>Fetch Jobs from Odoo:</strong>
<div class="api-block">
<pre class="api-dark">
GET /odoo/jobs
</pre>
</div>

- <strong>Fetch Candidates from Odoo:</strong>
<div class="api-block">
<pre class="api-dark">
GET /odoo/candidates
</pre>
</div>

---

### <span style="font-weight: 300; color: #8645e8;">Google Calendar / Meet Integration</span>

- <strong>Schedule Interview Event:</strong>
<div class="api-block">
<pre class="api-dark">
POST /interviews/{interview_id}/create-calendar-event/
</pre>
</div>
- <strong>Create Google Calendar Event:</strong>
<div class="api-block">
<pre class="api-dark">
POST /calendar/events
</pre>
</div>
- <strong>Get All Google Meet Links:</strong>
<div class="api-block">
<pre class="api-dark">
GET /calendar/meets
</pre>
</div>

---

### <span style="font-weight: 300; color: #8645e8;">AI, AssemblyAI, and Gemini</span>

- <strong>Start Interview with AI Assistant (transcription):</strong>
<div class="api-block">
<pre class="api-dark">
POST /interviews/create/
</pre>
</div>
- <strong>Get Post-Interview Analytics:</strong>
<div class="api-block">
<pre class="api-dark">
GET /interviews/{interview_id}/analytics/
</pre>
</div>

---

### <span style="font-weight: 300; color: #8645e8;">Attachments</span>

- <strong>Get Candidate Attachments:</strong>
<div class="api-block">
<pre class="api-dark">
GET /candidates/{candidate_id}/attachments/
</pre>
</div>

- <strong>Download Attachment:</strong>
<div class="api-block">
<pre class="api-dark">
GET /candidates/{candidate_id}/attachments/download/{attachment_id}/
</pre>
</div>

---

## <span style="font-weight: 300; font-size: 1.2em; color: #232f3e">API Testing & Documentation</span>

- **All APIs are tested in Postman.**
- See [Postman Docs](https://documenter.getpostman.com/view/45699975/2sB3HqHJX4).
- See screenshots above for Swagger UI and Postman documentation examples.

---

## <span style="font-weight: 300; font-size: 1.2em; color: #232f3e">Security & Authentication</span>

- Endpoints require authentication via JWT or session.
- Store API keys and secrets in environment variables (`.env`), **never** commit secrets to your repository.

---

## <span style="font-weight: 300; font-size: 1.2em; color: #232f3e">More Information</span>

- For a full list of all endpoints and models, see the [Developer Docs](developer-docs.md).
- For architecture and integration details, see [Developer Docs](developer-docs.md) and [Integrations](integrations.md).

---

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