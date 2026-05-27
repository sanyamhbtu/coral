# PagerDuty source for Coral

Query PagerDuty incidents, services, and on-call schedules as SQL.

## Get your API key
PagerDuty → Integrations → API Access Keys → Create new key

## Setup
coral source add pagerduty

## Tables
| Table    | What it gives you                        |
|----------|------------------------------------------|
| incidents| triggered/acknowledged/resolved alerts   |
| services | service health status                    |
| oncalls  | who is currently on call                 |

## Example queries
-- active incidents right now
SELECT id, title, status, created_at, service_name
FROM pagerduty.incidents WHERE status = 'triggered';

-- join with github to find the culprit
SELECT i.title, c.author, c.message
FROM pagerduty.incidents i
JOIN github.commits c
  ON c.pushed_at BETWEEN i.created_at - INTERVAL '2h' AND i.created_at
WHERE i.status = 'triggered';