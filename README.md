# infra-compose-stacks

Сборник Docker Compose-стеков инфраструктуры разработки: GitLab CE,
Rancher, ELK (Elasticsearch + Kibana + Grafana), Semaphore UI,
VictoriaMetrics и Postgres + pgAdmin. Стеки проверены на однохостовой
инфраструктуре с единой точкой входа — nginx-реверс-прокси, от которого
сервисы раздаются по подпутям (`/gitlab`, `/kibana`, `/grafana`,
`/semaphore`, `/victoriametrics`).

## Состав

| Каталог | Сервисы |
| --- | --- |
| `gitlab/` | GitLab CE + gitlab-runner (docker-executor, доступ к docker.sock) |
| `rancher/` | Rancher Server v2.14 (управление k3s-кластером) |
| `elk/` | Elasticsearch 8.6 + Kibana + Grafana |
| `semaphore/` | Semaphore UI (веб-GUI для Ansible) + Postgres |
| `victoriametrics/` | VictoriaMetrics single-node с promscrape |
| `otk/` | Postgres 16 + pgAdmin 4 |

## Пароли и домены

Реальные секреты в стеках заменены переменными окружения — задайте их в
`.env` в корне (рядом с docker-compose) или пробросьте извне:

```env
PUBLIC_BASE_URL=https://example.com      # внешний URL реверс-прокси
KIBANA_PASSWORD=changeme
KIBANA_ENCRYPTION_KEY=changeme
GRAFANA_ADMIN_PASSWORD=changeme
GITLAB_INITIAL_ROOT_PASSWORD=changeme
POSTGRES_PASSWORD=changeme
PGADMIN_DEFAULT_PASSWORD=changeme
CATTLE_BOOTSTRAP_PASSWORD=admin
SEMAPHORE_DB_PASS=changeme
SEMAPHORE_ADMIN_PASSWORD=changeme
VM_HTTPAUTH_PASSWORD=changeme
```

Имя пользователя gitlab-runner для примонтированного домашнего каталога —
`HOME_MOUNT`, SSH-ключи Semaphore — `SSH_KEYS_DIR`.

## Запуск

```bash
cd gitlab && docker compose -f gitlab.yaml --env-file ../.env up -d
```

Порты по умолчанию (при необходимости поправьте в yaml): GitLab —
8180/8443/8222, Rancher — 880/8443, Elasticsearch — 9200, Kibana — 5601,
Grafana — 3000, Semaphore — 3030, VictoriaMetrics — 8428, Postgres — 5432,
pgAdmin — 8080.

## Лицензия

MIT — см. [LICENSE](LICENSE).
