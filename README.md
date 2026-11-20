<div align="center">
<img src="assets/brand-logo.svg" alt="Arya Fatthurahman" width="120" />
</div>

# Firebird Lab

<sub>Run Firebird 5 in Docker, model a small sales schema, and practice SQL with isql and JDBC.</sub>

<p>
  <img src="https://img.shields.io/badge/Database-Firebird-F97316?style=flat-square" alt="Database: Firebird" />
  <img src="https://img.shields.io/badge/Category-Relational%20%28SQL%29-6366F1?style=flat-square" alt="Category: Relational (SQL)" />
  <img src="https://img.shields.io/badge/Runs%20in-Docker-2496ED?style=flat-square&logo=docker&logoColor=white" alt="Runs in Docker" />
  <img src="https://img.shields.io/badge/License-MIT-22C55E?style=flat-square" alt="License: MIT" />
  <a href="https://github.com/devarent/firebird-lab/actions/workflows/compose-check.yml"><img src="https://github.com/devarent/firebird-lab/actions/workflows/compose-check.yml/badge.svg" alt="compose-check" /></a>
</p>

## What this lab is

Firebird is a compact, embeddable relational database with strong SQL support and a low operational footprint. This lab gives you a real Firebird 5 server on port 3050, a schema you can extend, and the exact JDBC and isql connection details.

## Quickstart

```bash
docker compose up -d
docker compose cp examples/01_schema.sql firebird:/tmp/01_schema.sql
docker compose exec firebird isql-fb -user SYSDBA -password masterkey -i /tmp/01_schema.sql localhost:/firebird/data/lab.fdb
docker compose exec firebird isql-fb -user SYSDBA -password masterkey localhost:/firebird/data/lab.fdb
```

## Connection details

| Field | Value |
| --- | --- |
| Host | `localhost` |
| Port | `3050` |
| Database | `lab.fdb` |
| User | `SYSDBA` |
| Password | `masterkey` |
| JDBC URL | `jdbc:firebirdsql://localhost:3050/lab.fdb` |
| isql | `isql-fb -user SYSDBA -password masterkey localhost:/firebird/data/lab.fdb` |

## Examples

| File | What it shows |
| --- | --- |
| [`docker-compose.yml`](docker-compose.yml) | Engine, ports, and persistent volume. |
| [`.github/workflows/compose-check.yml`](.github/workflows/compose-check.yml) | Validates the Compose file on every push. |
| [`examples/01_schema.sql`](examples/01_schema.sql) | Customers and orders with identity columns, a foreign key, and an index. |
| [`examples/02_queries.sql`](examples/02_queries.sql) | Seed data, a join with aggregation, a CTE, and a window function. |

## Exercises

1. Add a `products` table and an `order_items` table, then calculate order totals from the line items.
2. Run `EXPLAIN PLAN` for the revenue query and compare the plan before and after adding an index.
3. Create a stored procedure that returns the top N customers by revenue.

## Notes and limits

- The root password in Compose is a local lab credential. Change it before exposing the port.
- Firebird stores the database file inside the named volume `firebird-data`.
- The image is pinned to Firebird 5.0.4 so the lab stays reproducible.

## Run it from anywhere

This lab publishes its ports on `127.0.0.1` only. To use it from another country, do **not** open the database port to the internet. Pick one:

- **VPS + private tunnel:** run the lab on a small Ubuntu VPS, keep the port closed, and connect through WireGuard or `ssh -L`.
- **Managed service:** use a managed free tier for this engine and point the same examples at its connection string.
- **HTTP UI only:** if the engine has a web UI, put it behind Caddy with TLS and basic auth.

The shared checklist, provider map, and commands are in **[REMOTE-ACCESS.md](https://github.com/devarent/database-labs/blob/main/REMOTE-ACCESS.md)**.

## Cleanup

```bash
docker compose down -v
```

`-v` also removes the named volume, so the next start is a clean slate.

## References

- [Firebird documentation](https://firebirdsql.org/en/documentation/)
- [Firebird Docker image](https://hub.docker.com/r/firebirdsql/firebird)
- [Jaybird JDBC driver](https://firebirdsql.org/en/jdbc-driver/)

---

<sub>Part of the <a href="https://github.com/devarent/database-labs">database-labs</a> collection: 26 database engines, one index.</sub>

## License

MIT (c) 2026 Arya Fatthurahman. See [LICENSE](LICENSE).
