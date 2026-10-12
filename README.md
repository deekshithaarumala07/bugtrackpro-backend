# BugTrackPro Backend

Java REST API for BugTrackPro Enterprise. It provides authentication, role-aware access control, bug and project workflows, sprint management, comments, attachments, notifications, analytics, and audit history.

## Requirements

- Java 21
- Maven 3.9+ (or Docker)
- MySQL 8

## Run locally

Create a MySQL database and user, copy `.env.example` to `.env`, and replace the placeholder values. Then run:

```bash
bash run-local.sh
```

The API listens on `http://localhost:8080`; its health endpoint is `/api/health`. Configure the frontend origin in `CORS_ORIGINS`.

## Important configuration

- `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`: MySQL connection
- `JWT_SECRET`: unique secret of at least 32 bytes; required in production
- `APP_ENV=production`: enables production configuration checks
- `CORS_ORIGINS`: comma-separated exact browser origins allowed to call the API
- `APP_UPLOAD_DIR`: directory for private uploaded attachments; use persistent storage when deployed
- `BOOTSTRAP_ADMIN_*`: one-time initial admin values; remove them after the first successful bootstrap

Never commit `.env` or real credentials. Public registration creates a Tester account.

## Build and deploy

```bash
mvn clean package
```

The Dockerfile builds and runs the API from this repository root. For Railway, configure the required database, JWT, CORS, and upload storage variables, then attach persistent storage at the upload directory if attachments need to survive redeploys. The backend currently uses Hibernate schema auto-update; use a versioned migration strategy before relying on it for production data.
