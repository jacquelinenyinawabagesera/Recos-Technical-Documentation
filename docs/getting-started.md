# <span style="font-weight: 1; font-size: 2.1em; color: #232f3e">Getting Started</span>

Welcome to the platform! This section guides you through account creation, authentication, password reset flows, and connecting your Odoo account. Each step includes technical details and backend logic.

---

### <span style="font-weight: 300; color: #8645e8;">1. Sign Up</span>

To create a new account, fill out the following fields:

- <span style="color: #232f3e"><b>First Name</b></span>
- <span style="color: #232f3e"><b>Last Name</b></span>
- <span style="color: #232f3e"><b>Email</b></span>
- <span style="color: #232f3e"><b>Password</b></span>

**How it works:**
- The backend creates a new user record after validating the fields.
- Passwords are securely hashed.

<div class="api-block">
<pre class="api-dark">
POST   /api/signup
</pre>
</div>

**Sample Request (Frontend → Backend):**
<div class="api-block">
<pre class="api-dark">
{
  "first_name": "Jacky",
  "last_name": "Uwase",
  "email": "jacky@gmail.com",
  "password": "Kumany1@"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "message": "Account created successfully."
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
from rest_framework.views import APIView
from rest_framework.response import Response
from rest_framework import status
from django.contrib.auth.models import User
from django.contrib.auth.hashers import make_password

class SignUpView(APIView):
    def post(self, request):
        first_name = request.data.get('first_name')
        last_name = request.data.get('last_name')
        email = request.data.get('email')
        password = request.data.get('password')
        if not (first_name and last_name and email and password):
            return Response({'error': 'All fields are required.'}, status=status.HTTP_400_BAD_REQUEST)
        if User.objects.filter(email=email).exists():
            return Response({'error': 'Email already exists.'}, status=status.HTTP_400_BAD_REQUEST)
        user = User.objects.create(
            first_name=first_name,
            last_name=last_name,
            email=email,
            username=email,
            password=make_password(password)
        )
        return Response({'message': 'Account created successfully.'}, status=status.HTTP_201_CREATED)
</pre>
</div>

![Signup Screenshot](images/signup.png)

---

### <span style="font-weight: 300; color: #8645e8;">2. Login</span>

If you already have an account, sign in using:

- **Email**
- **Password**

<div class="api-block">
<pre class="api-dark">
POST   /api/login
</pre>
</div>

**Sample Request:**
<div class="api-block">
<pre class="api-dark">
{
  "email": "jacky@gmail.com",
  "password": "Kumany1@"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "token": "jwt-token"
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
from rest_framework_simplejwt.tokens import RefreshToken

class LoginView(APIView):
    def post(self, request):
        email = request.data.get('email')
        password = request.data.get('password')
        user = authenticate(username=email, password=password)
        if user is not None:
            refresh = RefreshToken.for_user(user)
            return Response({'token': str(refresh.access_token)})
        return Response({'error': 'Invalid credentials.'}, status=status.HTTP_401_UNAUTHORIZED)
</pre>
</div>

![Login Screenshot](images/login.png)

---

### <span style="font-weight: 300; color: #8645e8;">3. Forgot Password Flow</span>

If you forget your password, follow these steps:

#### <span style="color: #232f3e;"><b>a. Initiate Password Reset</b></span>

- Click **"Forgot Password?"** on the login page.
- Enter your registered email address.

<div class="api-block">
<pre class="api-dark">
POST   /forgot-password/          # Start password reset
</pre>
</div>

**Sample Request:**
<div class="api-block">
<pre class="api-dark">
{
  "email": "jacky@gmail.com"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "message": "Password reset code sent."
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
class ForgotPasswordView(APIView):
    permission_classes = [permissions.AllowAny]

    def post(self, request):
        email = request.data.get('email', '').lower().strip()
        if not email:
            return Response({'error': 'Email is required'}, status=status.HTTP_400_BAD_REQUEST)

        try:
            user = Recruiter.objects.get(email=email)
        except Recruiter.DoesNotExist:
            return Response({'message': 'Password reset code sent if email exists in our system'}, status=status.HTTP_200_OK)

        verification_code = ''.join(random.choices('0123456789', k=4))
        cache.set(f'reset_code_{email}', verification_code, timeout=3*60)

        subject = 'Password Reset Verification Code'
        message = (
            f"Hello {user.first_name},\n\n"
            f"We received a request to reset your password. Use the verification code below:\n\n"
            f"{verification_code}\n\n"
            f"This code will expire in 3 minutes.\n\n"
            f"If you didn't request this, please ignore this email.\n\n"
            f"Thank you,\nThe Team"
        )

        send_mail(subject, message, settings.DEFAULT_FROM_EMAIL, [user.email], fail_silently=False)
        return Response({'message': 'Password reset code sent.'}, status=status.HTTP_200_OK)
</pre>
</div>

![Forgot Password Screenshot](images/forgot-password.png)

---

#### <span style="color: #232f3e;"><b>b. Enter Verification Code</b></span>

After receiving an email, enter the 4-digit code on the verification page.

<div class="api-block">
<pre class="api-dark">
POST   /verify-reset-code/        # Verify reset code sent to email
</pre>
</div>

**Sample Request:**
<div class="api-block">
<pre class="api-dark">
{
  "email": "jacky@gmail.com",
  "code": "1234"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "detail": "Code verified, you may now reset your password."
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
class VerifyCodeView(APIView):
    permission_classes = [permissions.AllowAny]

    def post(self, request):
        email = request.data.get('email', '').lower().strip()
        code = str(request.data.get('code', '')).strip()
        if not email or not code:
            return Response({'detail': 'Email and code are required.'}, status=status.HTTP_400_BAD_REQUEST)

        cached_code = cache.get(f'reset_code_{email}')
        if cached_code is None:
            return Response({"detail": "Code has expired, please request a new one."}, status=status.HTTP_400_BAD_REQUEST)
        if cached_code != code:
            return Response({"detail": "Invalid code."}, status=status.HTTP_400_BAD_REQUEST)

        request.session['reset_email'] = email
        cache.delete(f'reset_code_{email}')
        return Response({"detail": "Code verified, you may now reset your password."})
</pre>
</div>

![Verification Code Screenshot](images/verify-code.png)

---

#### <span style="color: #232f3e;"><b>c. Reset Password</b></span>

After code verification, set a new password (two fields: New Password, Confirm New Password).

<div class="api-block">
<pre class="api-dark">
POST   /reset-password/           # Complete password reset
</pre>
</div>

**Sample Request:**
<div class="api-block">
<pre class="api-dark">
{
  "email": "jacky@gmail.com",
  "password": "NewStrong1@"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "detail": "Password reset successful."
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
class ResetPasswordView(APIView):
    permission_classes = [AllowAny] 

    def post(self, request):
        serializer = ResetPasswordSerializer(data=request.data)
        serializer.is_valid(raise_exception=True)
        email = serializer.validated_data['email']
        password = serializer.validated_data['password']
        try:
            user = User.objects.get(email=email)
        except User.DoesNotExist:
            return Response({"detail": "User with this email does not exist."}, status=status.HTTP_400_BAD_REQUEST)
        user.set_password(password)
        user.save()
        code_storage.pop(email, None)
        return Response({"detail": "Password reset successful."})
</pre>
</div>

![Reset Password Screenshot](images/reset-password.png)

---

### <span style="font-weight: 300; color: #8645e8;">4. Connecting to Odoo</span>

After logging in, connect your Odoo account using:

- **Odoo Instance URL**
- **Email**
- **API Key / Password**

<div class="api-block">
<pre class="api-dark">
POST   /api/connect-odoo
</pre>
</div>

**Sample Request:**
<div class="api-block">
<pre class="api-dark">
{
  "odoo_url": "https://your-odoo-instance.com",
  "email": "odoo_user@example.com",
  "api_key": "yourOdooApiKey"
}
</pre>
</div>

**Sample Response:**
<div class="api-block">
<pre class="api-dark">
{
  "companies": [
    {
      "id": 1,
      "name": "Acme Corp"
    },
    {
      "id": 2,
      "name": "Innovate LLC"
    }
  ]
}
</pre>
</div>

**Backend Logic:**
<div class="api-block">
<pre class="api-dark">
import requests

class ConnectOdooView(APIView):
    def post(self, request):
        odoo_url = request.data.get('odoo_url')
        email = request.data.get('email')
        api_key = request.data.get('api_key')
        if not (odoo_url and email and api_key):
            return Response({'error': 'All fields are required.'}, status=status.HTTP_400_BAD_REQUEST)
        # Example Odoo connection logic (pseudo)
        response = requests.post(f"{odoo_url}/api/auth", data={"email": email, "api_key": api_key})
        if response.status_code != 200:
            return Response({'error': 'Odoo authentication failed.'}, status=status.HTTP_401_UNAUTHORIZED)
        # Fetch companies
        companies = requests.get(f"{odoo_url}/api/companies", headers={"Authorization": f"Bearer {api_key}"})
        return Response({'companies': companies.json()}, status=status.HTTP_200_OK)
</pre>
</div>

 ![Odoo Connection Screenshot](images/connect-to-odoo.png)

---

### <span style="font-weight: 300; color: #8645e8;">5. Company Selection</span>

After connecting, select a company from your Odoo account to start working.

- The dashboard lists companies fetched from Odoo.
- Choose one to work on.
- If you add a new company in Odoo, click **"Add Company"** to refresh and connect again.

![Company Selection Screenshot](images/connect-to-company.png)

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