# Security Policy

## Supported Versions

This project is actively maintained on the `master` branch. Security fixes are applied to the latest version only.

| Version | Supported |
| ------- | --------- |
| latest (`master`) | :white_check_mark: |
| older commits | :x: |

## Reporting a Vulnerability

If you discover a security vulnerability, please report it privately:

- Use GitHub's [private vulnerability reporting](https://docs.github.com/en/code-security/security-advisories/guidance-on-reporting-and-writing-information-about-vulnerabilities/privately-reporting-a-security-vulnerability) ("Report a vulnerability" under the **Security** tab), **or**
- Email the maintainer at **nipon@6amtech.com**.

Please include steps to reproduce, affected versions, and potential impact. You can expect an initial response within **5 business days**. Please do not open a public issue for security reports.

## Security Considerations

- This is a **development template**. The MySQL credentials in `docker-compose.yml` are defaults (`dev`/`dev`, root password `rootadmin`) — change all of them before any shared or production use.
- The `Dockerfile` runs `chmod -R 777` on `storage`, `resources`, and `bootstrap/cache` for convenience. Tighten these permissions for production deployments.
- Set a unique `APP_KEY` (via `php artisan key:generate`), keep `APP_DEBUG=false` outside local development, and never commit a real `.env`. The `app` service runs `php artisan migrate --force` on start — review migrations before pointing it at real data.
