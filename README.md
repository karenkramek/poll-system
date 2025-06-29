# 🗳️ Poll System - Drupal 10

This is a custom poll system project developed using **Drupal 10**, featuring full CRUD support, public display, and REST API integration.

## Requirements

- [Docker](https://www.docker.com/)
- [Lando](https://lando.dev/) (container-based local environment)

---

## How to run the project - local environment

### 1. Clone the repository

```bash
git clone git@github.com:karenkramek/poll-system.git
cd poll-system
```

### 2. Start the environment with Lando

```bash
lando start
```

> This will create containers with Apache + PHP + MySQL + Drush.

### 3. Install dependencies via Composer

```bash
lando composer install
```

### 4. Import the database

Make sure the dump is present at `db/dump.sql.gz`. Then run:

```bash
lando db-import db/dump.sql.gz
```

### 5. Access the site

Open your browser and go to:

```
http://poll-system.lndo.site/
```

---

## Admin access

Use `drush uli` to generate a one-time login link:

```bash
lando drush uli
```

---

## Project structure

```
poll-system/
├── .lando.yml                  # Local environment config
├── web/                        # Drupal site root
├── db/dump.sql.gz              # Database dump
├── composer.json               # Dependencies
├── modules/custom/poll_system/ # Custom poll system module
└── README.md
```

---

## Running Unit Tests

To run the unit tests for the poll system module, use the following command:

```bash
lando unit-test
```

This will execute the Drupal tests for the `poll_system` module inside the Lando environment. The command uses the configuration defined in `.lando.yml` and outputs verbose test results.

---

## REST API

### GET `/api/poll-system/{identifier}`

**Description:**  
Fetch poll data by its identifier. Returns poll details, options, and (if allowed) results. Requires the user to have the "vote in polls" permission and the voting system to be enabled.

**Example:**
```bash
curl -X GET "http://poll-system.lndo.site:8000/api/poll-system/{do_you_have_pets}" \
  -u admin:admin \
  -H "Accept: application/json"
```

---

### POST `/api/poll-system/{identifier}/vote`

**Description:**  
Submit a vote for a poll option. Requires the user to have the "vote in polls" permission and the voting system to be enabled. The request body must include the `option_id` to vote for.

**Example:**
```bash
curl -X POST "http://poll-system.lndo.site:8000/api/poll-system/{identifier}/vote" \
  --cookie "SESSabcd=your-session-cookie" \
  -H "Content-Type: application/json" \
  -d '{"option_id": 1}'
```

---

Authentication via cookie or token. See the `PollSystemResource` plugin.

---

## Notes

- Make sure `poll_system` is enabled:
  ```bash
  lando drush en poll_system -y
  ```
- You can customize the domain by changing `proxy:` in `.lando.yml`.
