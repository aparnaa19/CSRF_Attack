# CSRF Attack Lab (SEED Labs)

This project demonstrates Cross-Site Request Forgery (CSRF) vulnerabilities using the Elgg social networking platform as the target environment.

## 🔍 Objective
- Understand how CSRF attacks are crafted and triggered
- Launch GET and POST-based CSRF attacks
- Craft attacker-controlled HTML pages to exploit logged-in users
- Analyze HTTP requests and CSRF tokens
- Evaluate and implement CSRF countermeasures

## 🧰 Tools Used
- Elgg Web App
- Firefox with Http Header Live Extension
- Docker (SEED Ubuntu VM)
- HTML (attacker-controlled pages)
- SameSite Cookies

## 📌 Key Attack Scenarios
- **GET-based CSRF:** Added Samy as a friend to Alice by tricking Alice into clicking a crafted link
- **POST-based CSRF:** Modified Alice’s profile description without her knowledge using an attacker site
- **Blog Injection:** Hosted attacker HTML page on a fake blog to carry out hidden actions
- **Session Hijack Simulation:** Used cookies and GUID manipulation for identification
- **Cookie Experiment:** Demonstrated how SameSite cookie policies (Lax, Strict) prevent CSRF

## 🛡️ Mitigation
- Enabled built-in CSRF token checks in Elgg (`csrf.php`)
- Verified that missing tokens caused CSRF failures
- Demonstrated SameSite cookie enforcement and its impact on cross-site requests

## 🖼️ Screenshots
Add optional screenshots to demonstrate attack flow and prevention if needed.

## 📁 Files
- `csrf-attack-report.pdf` – Full documentation of attack steps, observations, and mitigation.

## 📄 Author
**Aparnaa Mahalaxmi Arulljothi**  
Student ID: A20560995

---

## 🔗 References
- [SEED Labs - CSRF Attack](https://seedsecuritylabs.org/Labs_20.04/Web/Web_CSRF/)
- [OWASP CSRF Overview](https://owasp.org/www-community/attacks/csrf)
