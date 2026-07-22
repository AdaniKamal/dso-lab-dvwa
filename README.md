# 🔐 DevSecOps Learning Journal

> Building a complete DevSecOps pipeline from scratch using open-source security tools and GitHub Actions.

![GitHub](https://img.shields.io/badge/GitHub-Actions-blue)
![Docker](https://img.shields.io/badge/Docker-Container-blue)
![DevSecOps](https://img.shields.io/badge/DevSecOps-Learning-success)
![License](https://img.shields.io/badge/License-MIT-green)

## 📖 About

This repository documents my hands-on journey of learning **DevSecOps** by building an automated security pipeline from the ground up.

Instead of only learning the theory, every step was implemented, tested, documented, and automated using GitHub Actions.

The project uses **DVWA (Damn Vulnerable Web Application)** as the target application and demonstrates how multiple security tools work together across the Software Development Lifecycle (SDLC).

## 🎯 Project Goals

- Learn modern DevSecOps practices
- Build an automated CI security pipeline
- Understand where each security tool fits within SDLC
- Gain practical experience with GitHub Actions
- Produce a reusable reference for future projects

# 🛠️ Technologies

| Category | Tools |
|----------|-------|
| Version Control | Git, GitHub |
| CI/CD | GitHub Actions |
| Containers | Docker, Docker Compose |
| Vulnerable Target | DVWA |
| Code Quality | Super-Linter |
| Secret Scanning | Gitleaks |
| SAST | Semgrep |
| SCA | Trivy Filesystem |
| Container Security | Trivy Image Scan |
| DAST | OWASP ZAP |

# 🚀 DevSecOps Pipeline

```text
Developer
     │
     ▼
Git Push
     │
     ▼
GitHub Actions
     │
     ├──────────────► Super-Linter
     │
     ├──────────────► Gitleaks
     │
     ├──────────────► Semgrep
     │
     ├──────────────► Trivy Filesystem
     │
     ├──────────────► Build Docker Image
     │
     ├──────────────► Trivy Image Scan
     │
     ├──────────────► Deploy DVWA
     │
     └──────────────► OWASP ZAP
```

# 📚 Learning Progress

| Step | Topic | Status |
|------|-------|:------:|
| 1 | Docker Desktop & Docker Compose | ✅ |
| 2 | Deploy DVWA | ✅ |
| 3 | Git & GitHub | ✅ |
| 4 | GitHub Actions | ✅ |
| 5 | Super-Linter | ✅ |
| 6 | Gitleaks | ✅ |
| 7 | Semgrep (SAST) | ✅ |
| 8 | Trivy Filesystem (SCA) | ✅ |
| 9 | Docker Compose Deployment | ✅ |
| 10 | Trivy Image Scan | ✅ |
| 11 | OWASP ZAP (DAST) | ✅ |

# 🔍 Security Coverage

| Security Layer | Tool |
|---------------|------|
| Code Quality | Super-Linter |
| Secret Detection | Gitleaks |
| Static Analysis (SAST) | Semgrep |
| Dependency Analysis (SCA) | Trivy Filesystem |
| Container Security | Trivy Image |
| Dynamic Security Testing (DAST) | OWASP ZAP |

# 🎓 Key Skills Demonstrated

- DevSecOps
- Secure CI/CD
- GitHub Actions
- Docker
- Docker Compose
- SAST
- DAST
- SCA
- Secret Scanning
- Secure Software Supply Chain
- Infrastructure as Code
- Vulnerability Assessment
- Application Security

# 📖 Full Documentation

The complete implementation, explanations, screenshots, troubleshooting notes, and reflections are available in:

📄 **DevSecOps-Pipeline-Lab.md**

## ⭐ Future Improvements

- [ ] DefectDojo integration
- [ ] SBOM generation
- [ ] Dependency Track integration
- [ ] CodeQL scanning
- [ ] Kubernetes deployment
- [ ] Checkov IaC scanning
- [ ] Cosign image signing
- [ ] Falco runtime security
- [ ] Automated Slack/Teams notifications

