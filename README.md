Ultimate Deployment Master Prompt 
📌 Project Overview
Architecture: Modular Monolith (Spring Boot + React).

Target: Single EC2 Instance (t3.small - 2GB RAM).

Goal: Multi-environment setup with Automated Migrations, Error Tracking, and Off-site Backups.

🛠 Tech Stack Requirements
Backend: Java 17+, Spring Boot (with Actuator & Liquibase for DB Migrations).

Frontend: React (with Sentry SDK for error tracking).

Database: PostgreSQL (with automated S3 Backup scripts).

CI/CD: Jenkins (Dockerized, On-demand).

Reverse Proxy: Nginx (SSL/HTTPS).

Monitoring: Portainer (GUI), UptimeRobot (External), and Sentry (Error Dashboard).

📏 Mandatory Rules & Regulations
1. Infrastructure & Isolation
Environments: Production (Port 8080/prod_db), Testing (Port 8081/test_db).

Docker: Full containerization via docker-compose.

2. Zero Downtime & DB Safety (Liquibase)
Automatic Migrations: Use Liquibase. No manual SQL execution on production. Every DB change must be a changeset in the code.

Health Checks: Use Spring Actuator. Old container kills only AFTER the new one is "Healthy".

Grace Period: Minimum 40-60s for JVM boot.

3. Error Tracking & Reliability (Sentry)
Sentry Integration: Configure Sentry SDK for both React (Frontend) and Spring Boot (Backend).

Alerting: Ensure the setup captures stack traces and sends them to the Sentry dashboard automatically.

4. Disaster Recovery (S3 Backup Logic)
Automated Backups: Create a Cron Job/Shell Script that:

Takes a pg_dump of the PostgreSQL database.

Compresses the file.

Uploads it to an AWS S3 Bucket using AWS CLI.

Retention: Implement a 7-day local rotation and 30-day S3 lifecycle policy.

5. Resource Management (2GB RAM Optimization)
Swap: Enable 2GB Swap Memory.

JVM Limits: Backend Prod (512MB), Backend Test (256MB).

On-Demand SonarQube: Start Sonar container only during the scan stage and stop immediately after.

Jenkins: Start only for builds, stop after deployment.

6. CI/CD Pipeline (Jenkinsfile Logic)
Multi-Repo: Independent checkout for Frontend and Backend repos.

Build & Quality: Run Snyk (Security) and SonarQube (Quality) on-demand.

Manual Sign-Off: Block production deployment until manual approval after Staging test.

📝 Required Deliverables (The Code I Need)
docker-compose.yml: Optimized for 2GB RAM, including App, DB, and Portainer.

Jenkinsfile: Including Multi-repo, SonarQube Start/Stop logic, and Approval gate.

Liquibase Configuration: Sample changelog-master.xml and folder structure for Spring Boot.

Sentry Setup: Basic configuration for application.yml and React index.js.

Backup Script: backup-to-s3.sh with pg_dump and aws s3 cp commands.

Setup Scripts: Shell scripts for EC2 Swap Memory, Docker, and AWS CLI configuration.

Nginx Config: Subdomain routing for app. and test. environments.

💡 Founder's Final Note to AI:
"The goal is a production-ready environment for a solo founder. Focus on automation to minimize manual server intervention. Use sasta-jugad (low cost/low RAM) but maintain high-level architecture principles (Principal Architect level)."
