Jak rozwiązać problem z dużym zużyciem pamięci przez kontener Docker?

Oto kilka sposobów na rozwiązanie problemu z dużym zużyciem pamięci przez kontener Docker:

1. Ustaw limity pamięci:
```bash
docker run --memory="2g" --memory-swap="2g" nazwa-kontenera
```
Możesz też dodać te limity w docker-compose.yml:
```yaml
services:
  app:
    image: nazwa-obrazu
    mem_limit: 2g
    memswap_limit: 2g
```

2. Zoptymalizuj obraz Docker:
- Używaj lekkich obrazów bazowych (np. alpine)
- Usuwaj niepotrzebne pliki i cache podczas budowania
- Używaj wieloetapowego budowania (multi-stage builds)
- Łącz komendy RUN w jednej warstwie

3. Monitoruj i analizuj zużycie:
```bash
docker stats
docker stats --no-stream
```

4. Zoptymalizuj aplikację:
- Sprawdź wycieki pamięci w kodzie
- Zaimplementuj mechanizmy czyszczenia pamięci
- Użyj garbage collectora
- Zoptymalizuj strukturę danych

5. Skonfiguruj swap:
```bash
docker run --memory="2g" --memory-swap="4g" nazwa-kontenera
```

6. Użyj narzędzi do monitorowania:
- cAdvisor
- Prometheus
- Grafana

7. Regularnie czyść nieużywane kontenery i obrazy:
```bash
docker system prune
docker container prune
docker image prune
```

8. Rozważ skalowanie poziome:
- Uruchom więcej instancji z mniejszymi limitami pamięci
- Użyj load balancera do rozdzielenia ruchu

9. Sprawdź konfigurację systemu:
- Dostosuj parametry jądra
- Sprawdź ustawienia cgroups
- Zoptymalizuj konfigurację Docker daemon

10. Użyj najlepszych praktyk:
- Regularnie aktualizuj obrazy
- Używaj .dockerignore
- Implementuj health checks
- Monitoruj metryki wydajności


Jako doświadczony DevOps Engineer, podzielę się sprawdzonymi metodami rozwiązywania problemów z pamięcią w kontenerach Docker:

1. Najpierw zdiagnozuj problem:
```bash
# Monitoruj zużycie zasobów w czasie rzeczywistym
docker stats

# Sprawdź szczegółowe metryki kontenera
docker inspect nazwa-kontenera | grep -i memory

# Przeanalizuj logi kontenera
docker logs nazwa-kontenera
```

2. Zastosuj limity pamięci:
```bash
# Podstawowy limit pamięci
docker run --memory="2g" --memory-swap="2g" nazwa-kontenera

# Lub w docker-compose.yml
services:
  app:
    image: nazwa-obrazu
    deploy:
      resources:
        limits:
          memory: 2G
        reservations:
          memory: 1G
```

3. Zoptymalizuj obraz Docker:
```dockerfile
# Użyj wieloetapowego budowania
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=builder /app/dist ./dist
COPY package*.json ./
RUN npm ci --only=production
```

4. Zaimplementuj monitoring:
```yaml
# docker-compose.yml z monitoringiem
services:
  app:
    image: nazwa-obrazu
    deploy:
      resources:
        limits:
          memory: 2G
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

5. Skonfiguruj automatyczne czyszczenie:
```bash
# Część skryptu CI/CD
docker system prune -f --volumes
docker image prune -f
```

6. Zaimplementuj skalowanie:
```yaml
# docker-compose.yml z konfiguracją skalowania
services:
  app:
    image: nazwa-obrazu
    deploy:
      replicas: 3
      resources:
        limits:
          memory: 1G
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
```

7. Dodaj alerting:
```yaml
# prometheus.yml
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['alertmanager:9093']

rule_files:
  - "alert.rules"
```

8. Zoptymalizuj aplikację:
- Zaimplementuj mechanizmy czyszczenia pamięci
- Użyj connection pooling
- Zoptymalizuj zapytania do bazy danych
- Implementuj caching

9. Regularne utrzymanie:
```bash
# Skrypt do monitorowania i czyszczenia
#!/bin/bash
THRESHOLD=85
MEMORY_USAGE=$(docker stats --no-stream --format "{{.MemPerc}}" nazwa-kontenera | sed 's/%//')

if (( $(echo "$MEMORY_USAGE > $THRESHOLD" | bc -l) )); then
    echo "Wysokie zużycie pamięci: $MEMORY_USAGE%"
    docker-compose down
    docker system prune -f
    docker-compose up -d
fi
```

10. Dokumentacja i procedury:
- Utrzymuj dokumentację konfiguracji
- Zdefiniuj procedury awaryjne
- Regularnie przeglądaj metryki
- Aktualizuj limity w miarę rozwoju aplikacji

Te rozwiązania powinny pomóc w utrzymaniu stabilności i wydajności kontenerów Docker. Pamiętaj, że każda aplikacja jest inna i może wymagać dostosowania tych rozwiązań do konkretnych potrzeb.
