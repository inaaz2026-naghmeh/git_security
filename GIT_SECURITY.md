# Git Security: Protecting Sensitive Information

Committing secrets to a public or shared repository is one of the most common causes of data breaches. Attackers use automated tools to scan GitHub for secrets within seconds of a commit.

## ❌ What MUST NEVER be Pushed to GitHub

Do not commit any file containing the following types of information:

### 1. API Keys & Access Tokens
- **Cloud Providers:** AWS Access/Secret Keys, Google Cloud JSON keys, Azure Service Principals.
- **SaaS Platforms:** Slack Webhooks, Discord Tokens, SendGrid API keys, Stripe Secret Keys.
- **VCS Tokens:** GitHub Personal Access Tokens (PATs).

### 2. Private Keys & Certificates
- **SSH Keys:** `id_rsa`, `id_ed25519`.
- **SSL/TLS Certificates:** `.pem`, `.crt`, `.p12` files.
- **Encryption Keys:** PGP/GPG private keys.

### 3. Database Credentials
- **Connection Strings:** `DATABASE_URL=postgres://user:password@host:port/db`.
- **Hardcoded Credentials:** Username and password pairs in configuration files.

### 4. Environment Variables
- **Configuration Files:** `.env`, `secrets.yaml`, `config.json` containing secrets.
- **CI/CD Configuration:** Hardcoded passwords in `.github/workflows` or Jenkinsfiles.

---

## 🛡️ Risks of Leaked Secrets

1. **Automated Exploitation:** Bots scrape public feeds and can spin up crypto-miners on your AWS account or exfiltrate database data in minutes.
2. **Lateral Movement:** A single leaked key can give an attacker access to your entire cloud infrastructure.
3. **Supply Chain Attacks:** Leaking npm or PyPI tokens allows attackers to inject malicious code into your packages, affecting all users.
4. **Permanent Exposure:** Once a secret is in Git history, it remains there even if you delete the file. The only safe fix is to **revoke the secret**.

---

## ✅ Best Practices for Security

1. **Use `.gitignore`:** Always list sensitive files like `.env`, `*.pem`, and `node_modules` in your `.gitignore`.
2. **Environment Variables:** Use local environment variables and tools like `dotenv` for development.
3. **Secret Managers:** Use GitHub Secrets, AWS Secrets Manager, or HashiCorp Vault for production.
4. **Pre-commit Hooks:** Install tools like `gitleaks` or `trufflehog` to detect secrets before you commit.
5. **Secret Scanning:** Enable GitHub's built-in Secret Scanning in your repository settings.

**If you commit a secret by mistake: REVOKE IT IMMEDIATELY. Deleting the commit or the file is NOT enough.**
