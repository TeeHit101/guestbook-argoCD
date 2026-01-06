Guestbook Config (GitOps)

Detta repository innehåller Kubernetes-konfigurationen (YAML-filer) för applikationen Guestbook.
Det fungerar som "Single Source of Truth" för vår ArgoCD-pipeline.

Struktur

openshif-yaml/
├── backend-deploy.yaml       # Deployment för Backend (Go)
├── backend-svc.yaml          # Service för Backend
├── deployment-postgres.yaml  # Deployment för PostgreSQL
├── deployment-redis.yaml     # Deployment för Redis
├── frontend-deploy.yaml      # Deployment för Frontend (Nginx)
├── frontend-route.yaml       # Route för extern åtkomst (Frontend)
├── frontend-svc.yaml         # Service för Frontend
├── postgres-pvc.yaml         # Persistent Volume Claim för databasen
├── postgres-svc.yaml         # Service för databasen
├── redis-pvc.yaml            # Persistent Volume Claim för Redis
└── svc-redis.yaml            # Service för Redis


Hur det fungerar (GitOps Flöde)

Kodändring: När ny kod pushas till vårt kod-repo (guestbook-CI), bygger en GitHub Action en ny Docker-image.

Automatisk Uppdatering: CI-botten går automatiskt in i detta repo och uppdaterar image-taggen i deploy-filerna (t.ex. till :abc1234).

Deploy: ArgoCD övervakar detta repo. När filen ändras, synkar ArgoCD automatiskt ändringen till OpenShift-klustret.

Manuell Hantering

Secrets: Hemligheter (ghcr-secret, patiwat-guestbook-secrets) hanteras manuellt i klustret eller via Sealed Secrets (ligger ej i detta repo av säkerhetsskäl).

ArgoCD: Applikationen styrs av argocd-app.yaml som pekar på mappen openshif-yaml i detta repo.

Detta projekt är en del av kursen Containerteknologi (DevOps24).
