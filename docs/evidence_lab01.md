# Evidence LAB01

## 1. Obiettivo compreso
Spiego con parole mie il flusso:
GitHub -> Azure DevOps -> ACR -> ACI
- Il codice viene versionato su GitHub
- Azure DevOps esegue la pipeline
- Viene costruita l'immagine su Docker
- L'immagine viene pushata su ACR
- Il container viene deployato su ACI ed esposto pubblicamente

## 2. Repository GitHub
Indico il nome del repository usato e il branch principale.
- Repository: app-v3-devops
- Branch principale: main

## 3. Azure DevOps
Indico:
- nome del progetto Azure DevOps: corso-observability-francescodiiorio
- nome della pipeline: azure-pipeline.yml
- tipo di connessione usata verso GitHub: collegamento repository
- nome della Azure Resource Manager service connection: sc-obs-azure-rg

## 4. Build e push immagine
Spiego quale immagine è stata costruita e con quale tag.
- Immagine Docker: obsapp-v3
- Tag: BuildId (es. 11) 
- obsacrc830a3.azurecr.io/obsapp-v3:11
- La pipeline esegue build Docker image
- Push su Azure Container Registry

## 5. Deploy su ACI
Indico:
- Resource Group: rg-observability-lab12-standalone
- nome del container: obsapp-v3-aci
- Location westeurope
- Porta esposta: 8000
- FQDN finale: obsapp-francesco-devops-11.westeurope.azurecontainer.io

## 6. Test applicativi
Incollo l’output di:
- GET /
```bash
{
  "app": "obsapp",
  "version": "v3",
  "status": "running",
  "timestamp": "2026-04-14T08:19:27Z"
}
```

- GET /health
```bash
{
  "status": "ok",
  "timestamp": "2026-04-14T08:19:31Z"
}
```

- GET /time
```bash
{
  "time": "2026-04-14T08:19:34Z"
}
```

- POST /echo
```bash
{
  "lab": "01"
}
```
---
```bash
{
  "received": {
    "lab": "01"
  },
  "status": 200
}
```

- GET /error
```bash
{
  "error": "simulated_error",
  "status": 500
}
```

---

## 7. Log del container
Incollo alcune righe JSON e commento i campi principali.
```bash
{
  "timestamp": "2026-04-14T08:16:00.066020+00:00",
  "level": "ERROR",
  "message": "request_completed",
  "request_id": "3ba82886-b98a-406c-b513-a65635e46feb",
  "method": "GET",
  "path": "/nop",
  "status": 404,
  "latency_ms": 0.14,
  "client_ip": "10.92.0.9",
  "user_agent": "Mozilla/5.0"
}
```

```bash
{
  "timestamp": "2026-04-14T08:19:38.221475+00:00",
  "level": "INFO",
  "message": "request_completed",
  "request_id": "ee813267-46cf-4013-b990-8655ee3ba2f0",
  "method": "POST",
  "path": "/echo",
  "status": 200,
  "latency_ms": 0.17,
  "client_ip": "10.92.0.10",
  "user_agent": "curl/8.5.0"
}
```
## ANALISI LOG:
- level: INFO o ERROR in base allo status HTTP
- request_id: identificativo univoco richiesta
- latency_ms: tempo risposta API
- client_ip: IP del client (INTERNO ACI)
- status: codice HTTP (200, 404)

---

## 8. Problemi incontrati
Descrivo eventuali errori e come li ho risolti.
- Docker non funzionante:
- Errore: failed to connect to docker API
- Soluzione: avvio Desktop Docker

---

- DNS non valido/già occupato:
- Errore: DnsNameLabelAlreadyTaken
- Soluzione: DNS dinamico con BuildId

---

- InvalidOsType:
- Errore durante az container create:
- Soluzione: aggiunto --os-type Linux

---

- Timeout browser:
- URL errato (DNS VECCHIO)
- Soluzione: uso FQDN corretto generato dalla pipeline

---

- Il laboratorio ha permesso di implementare una pipeline completa DevOps con:
- CI/CD automatizzata
- containerizzazione con Docker
- registry su ACR
- deployment su ACI
- logging applicativo strutturato
- esposizione pubblica dell’app