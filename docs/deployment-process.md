# <span style="font-weight: 1; font-size: 2.1em; color: #232f3e;">Deployment Process</span>

---

## <span style="font-weight: 300; color: #8645e8;">Frontend Deployment</span>

- <span style="color: #232f3e;"><b>Platform:</b></span> Vercel
- <span style="color: #232f3e;"><b>Branch:</b></span> Auto-deployment from `main`
- <span style="color: #232f3e;"><b>Environment Variables:</b></span> Managed securely via `.env` in Vercel dashboard
- <span style="color: #232f3e;"><b>Build & Preview:</b></span> Each push triggers preview builds for PRs and deploys on merge to `main`

---

## <span style="font-weight: 300; color: #8645e8;">Backend Deployment</span>

- <span style="color: #232f3e;"><b>Platform:</b></span> Django REST API deployed on Heroku
- <span style="color: #232f3e;"><b>Static/Media Files:</b></span> Managed via cloud storage (e.g., AWS S3), if required
- <span style="color: #232f3e;"><b>Environment Variables:</b></span> Configured in Heroku dashboard for security
- <span style="color: #232f3e;"><b>Scaling:</b></span> Automatic scaling via Heroku dynos for increased demand

---

## <span style="font-weight: 300; color: #8645e8;">CI/CD Pipeline</span>

- <span style="color: #232f3e;"><b>Tool:</b></span> GitHub Actions
- <span style="color: #232f3e;"><b>Pre-Deployment:</b></span> All codebases run tests, build, and lint checks before deploy
- <span style="color: #232f3e;"><b>Automation:</b></span> Automatic deployment on merge to `main`
- <span style="color: #232f3e;"><b>Status:</b></span> Build and test status visible in PRs and repository dashboard

---

## <span style="font-weight: 300; color: #8645e8;">Best Practices</span>

- <span style="color: #232f3e;"><b>Secrets Management:</b></span> Never commit secrets; always use environment variable configuration
- <span style="color: #232f3e;"><b>Rollback:</b></span> Use Vercel and Heroku dashboards to rollback failed deployments
- <span style="color: #232f3e;"><b>Monitoring:</b></span> Monitor deployments and logs for errors using integrated dashboards

---