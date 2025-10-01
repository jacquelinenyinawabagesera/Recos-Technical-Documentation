# <span style="font-weight: 1; font-size: 2.1em; color: #232f3e;">Integrations</span>

Detailed documentation for integrating with Odoo, Google Calendar/Meet, AssemblyAI, and Gemini.  
This guide covers setup, configuration, API usage, troubleshooting, and best practices.

---

## <span style="font-weight: 300; color: #8645e8;">Odoo Integration</span>

**Overview:**  
Integrate your platform with Odoo to fetch jobs, candidates, and manage company data.

**Steps:**
1. **Odoo Setup**
   - Obtain your Odoo instance URL (e.g., `https://your-odoo-instance.com`)
   - Generate an API key from your Odoo account (Settings → API Keys).
   - Make sure your Odoo user has the necessary access rights for jobs, candidates, and companies.

2. **Connecting via API**
   - Store credentials securely in `.env`:
     ```
     ODOO_URL=https://your-odoo-instance.com
     ODOO_EMAIL=your@email.com
     ODOO_API_KEY=yourapikey
     ```
   - Use API endpoints to connect and fetch data.
     ```python
     import requests

     url = f"{ODOO_URL}/api/jobs"
     headers = {"Authorization": f"Bearer {ODOO_API_KEY}"}
     response = requests.get(url, headers=headers)
     print(response.json())
     ```

3. **Sample API Calls**
   - `GET /odoo/jobs` – Fetch all jobs from Odoo.
   - `GET /odoo/candidates` – Fetch all candidates.

4. **Common Issues & Solutions**
   - **401 Unauthorized:** Double-check API key, user permissions.
   - **Connection Error:** Ensure Odoo server is running and accessible.
   - **Timeouts:** Optimize endpoints, use pagination if available.

5. **Best Practices**
   - Never log or expose API keys in error messages.
   - Use environment variables for all sensitive data.
   - Handle rate limiting and retries in your integration code.

---

## <span style="font-weight: 300; color: #8645e8;">Google Calendar & Google Meet Integration</span>

**Overview:**  
Integrate Google Calendar to schedule interviews and auto-generate Google Meet links.

**Steps:**
1. **Create Google Cloud Project**
   - Go to [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project (e.g., “Recruitment Platform”).

2. **Enable APIs**
   - In the project, enable:
     - Google Calendar API
     - Google People API (if you want to fetch attendee profiles)
     - Google Meet integration comes with Calendar events

3. **Create OAuth 2.0 Client Credentials**
   - Go to “APIs & Services” → “Credentials”.
   - Click **Create Credentials** → **OAuth Client ID**.
   - Choose **Web application** and add your redirect URI (e.g., `https://your-domain.com/api/auth/google/callback/`).
   - Download the `credentials.json` file and keep it **secure**.
   - Store client ID and secret in `.env`.

4. **OAuth Flow Implementation**
   - On the backend (Python/Django), use libraries like `google-auth`, `google-api-python-client`.
   - Redirect users to Google OAuth consent screen.
   - After success, handle the callback, exchange the code for tokens, and store refresh tokens securely.

   ```python
   from google_auth_oauthlib.flow import Flow

   flow = Flow.from_client_secrets_file(
       'credentials.json',
       scopes=['https://www.googleapis.com/auth/calendar'],
       redirect_uri='https://your-domain.com/api/auth/google/callback/'
   )
   authorization_url, state = flow.authorization_url(access_type='offline', prompt='consent')
   ```

5. **Creating Calendar Events & Meet Links**
   - Use the Google Calendar API to create events with `conferenceData` for Meet links:
     ```python
     event = {
         'summary': 'Interview',
         'start': {'dateTime': '2025-10-02T10:00:00-07:00', 'timeZone': 'America/Los_Angeles'},
         'end': {'dateTime': '2025-10-02T11:00:00-07:00', 'timeZone': 'America/Los_Angeles'},
         'attendees': [{'email': 'candidate@example.com'}],
         'conferenceData': {
             'createRequest': {'requestId': 'sample123'}
         }
     }
     created_event = calendar_service.events().insert(
         calendarId='primary',
         body=event,
         conferenceDataVersion=1
     ).execute()
     meet_link = created_event['hangoutLink']
     ```

6. **Sample API Calls**
   - `POST /calendar/events` – Create interview event and Meet link.
   - `GET /calendar/meets` – List all Meet links.

7. **Common Issues & Solutions**
   - **Redirect URI mismatch:** Make sure the URI in Google Console matches your app’s callback.
   - **Missing conferenceData:** Pass `conferenceDataVersion=1` when creating events.
   - **Token expired:** Implement token refresh logic.
   - **Invalid credentials:** Check your `credentials.json` file and `.env` settings.

8. **Security & Best Practices**
   - Never expose `credentials.json` publicly.
   - Store tokens encrypted.
   - Use HTTPS for all OAuth flows.

9. **Error Handling**
   - Log errors and monitor via Sentry or your logging system.
   - Always handle exceptions for network/API calls.

---

## <span style="font-weight: 300; color: #8645e8;">AssemblyAI Integration</span>

**Overview:**  
Use AssemblyAI to transcribe interviews in real-time and provide transcripts for later processing.

**Steps:**
1. **Sign Up and Get API Key**
   - Register at [AssemblyAI](https://www.assemblyai.com/) and create an account.
   - Find your API key in your dashboard.

2. **Store API Key**
   - Store your API key in `.env`:
     ```
     ASSEMBLYAI_API_KEY=your_api_key
     ```

3. **Upload Audio & Start Transcription**
   - Send audio file to AssemblyAI and start transcription:
     ```python
     import requests

     headers = {'authorization': ASSEMBLYAI_API_KEY}
     response = requests.post('https://api.assemblyai.com/v2/upload', files={'file': open('audio.mp3', 'rb')}, headers=headers)
     audio_url = response.json()['upload_url']

     transcript_response = requests.post(
         'https://api.assemblyai.com/v2/transcript',
         json={'audio_url': audio_url},
         headers=headers
     )
     transcript_id = transcript_response.json()['id']
     ```

4. **Get Transcription Results**
   - Poll the API until transcription is complete:
     ```python
     import time
     while True:
         res = requests.get(f'https://api.assemblyai.com/v2/transcript/{transcript_id}', headers=headers)
         status = res.json()['status']
         if status == 'completed': break
         elif status == 'failed': raise Exception('Transcription failed')
         time.sleep(5)
     transcript_text = res.json()['text']
     ```

5. **Common Issues & Solutions**
   - **Large files:** Use chunked upload if audio is >100MB.
   - **API limits:** Monitor usage and handle rate limiting.
   - **Audio format errors:** Use standard formats (mp3, wav).

6. **Security & Best Practices**
   - Never expose your API key.
   - Delete audio files after processing if not needed.

---

## <span style="font-weight: 300; color: #8645e8;">Gemini Integration (AI Reports)</span>

**Overview:**  
Use Gemini (Google AI) to analyze interviews, extract sentiments, and generate candidate ratings and recommendations.

**Steps:**
1. **Gemini API Access**
   - Sign up for Gemini or Google AI platform and enable the Gemini API.
   - Obtain an API key and set usage quotas.

2. **Store API Key**
   - Add your Gemini API key to `.env`:
     ```
     GEMINI_API_KEY=your_api_key
     ```

3. **Send Data for AI Analysis**
   - Format interview transcription and candidate data.
   - Send to Gemini API for report generation:
     ```python
     import requests

     headers = {"Authorization": f"Bearer {GEMINI_API_KEY}"}
     data = {"interview_text": transcript_text, "candidate_profile": {...}}
     response = requests.post("https://gemini.googleapis.com/v1/analyze", json=data, headers=headers)
     report = response.json()
     ```

4. **Handle Response**
   - Parse AI report, extract recommendations, strengths, gaps, and scores.
   - Store and display in your dashboard.

5. **Common Issues & Solutions**
   - **API errors:** Validate data format and authentication.
   - **Quota exceeded:** Monitor usage and request more quota if needed.
   - **Latency:** Gemini AI reports may take several seconds; show loading indicator.

6. **Security & Best Practices**
   - Never expose your Gemini API key.
   - Validate and sanitize all data sent to AI APIs.

---

## <span style="font-weight: 300; color: #8645e8;">General Troubleshooting & Best Practices</span>

- Always use `.env` for sensitive credentials and never commit them.
- Use HTTPS for all API and OAuth flows.
- Monitor logs (Django, Vercel, Heroku, Sentry) for errors and usage spikes.
- Handle errors gracefully and inform users of next steps.
- Keep all API libraries up to date for security and compatibility.
- Regularly test integrations with sandbox/test accounts before deploying to production.

---

## <span style="font-weight: 300; color: #8645e8;">References & Further Reading</span>

- [Odoo API Docs](https://www.odoo.com/documentation/)
- [Google Calendar API Quickstart](https://developers.google.com/calendar/api/quickstart/python)
- [Google OAuth Docs](https://developers.google.com/identity/protocols/oauth2)
- [AssemblyAI API Docs](https://www.assemblyai.com/docs/)
- [Gemini (Google AI) Docs](https://ai.google.dev/gemini)
