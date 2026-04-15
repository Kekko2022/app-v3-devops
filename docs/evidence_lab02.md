# Evidence LAB02

## 1. Obiettivo compreso
Spiego con parole mie perché questa pipeline è migliore di quella del LAB01.
L’obiettivo di questo laboratorio è automatizzare il ciclo CI/CD completo su Azure utilizzando una pipeline Azure DevOps composta da tre stage (Build, Deploy e SmokeTest).

## 2. Repository GitHub
Indico il nome del repository usato e il branch principale.
Il repository utilizzato è:
- nome repo: app-v3-devops
- Owner: Kekko2022
- Branch principale: main

## 3. Nuova struttura pipeline
Descrivo i tre stage:
- Build: 
1) Build dell'immagine Docker
2) Push su Azure Container Registry (ACR)
- Deploy:
1) Eliminazione del container precedente
2) Creazione di un nuovo Azure Container Instance
3) Pull dell'immagine dal registry
- SmokeTest:
1) Recupero FQDN del container
2) Attesa stato Running
3) Test automatici sugli endpoint dell'app

## 4. Tag immagine
Indico il tag usato e spiego perché è stato scelto.
- $(Build.BuildId)
- Garantisce che ogni build abbia un identificatio univoco.

## 5. Verifica stage Build
Descrivo cosa accade nello stage Build.
- Nello stage build viene eseguito:
1) Login ad Azure Container Registry
2) Build dell'immagine Docker partendo dal Dockerfile
3) Push dell'immagine nel registry
- Risultato l'immagine viene correttamente caricata su ACT con tag univoco

## 6. Verifica stage Deploy
Descrivo cosa accade nello stage Deploy.
- Nello stage Deploy viene eseguito:
1) Recupero credenziali ACR
2) Eliminazione del container ACI precedente
3) Creazione di un nuovo container
4) configurazione di CPU - MEMORIA - PORTA - DNS
-Risultato: Il container viene creato correttamente e diventa accessibile oubblicamente

## 7. Verifica stage SmokeTest
Riporto i test eseguiti sugli endpoint e l’esito.
- Lo smoketest esegue automaticamente:
1) Recupero FQDN del container
2) Attesa dello stato Running
3) Test degli endopoint

## 8. Log del container
Incollo alcune righe JSON e commento i campi principali.

## 9. Differenze rispetto al LAB01
Elenco almeno 5 miglioramenti introdotti.
- Introduzione di pipeline CI/CD completa (non solo build)
- Deploy automatico su Azure Container Instances
- Uso di Azure Container Registry per le immagini
- Smoke test automatici sugli endpoint
- Versionamento immagini con BuildId

## 10. Problemi incontrati
Descrivo eventuali errori e come li ho risolti.

Durante il laboratorio sono stati riscontrati alcuni problemi:
- FQDN inizialmente vuoto o non valorizzato correttamente
RISOLTO: con trimming e controllo IsNullOrWhiteSpace
- Errore su Invoke-WebRequest in modalità non interattiva
RISOLTO aggiungendo -UseBasicParsing
- Problemi di timing tra stato Running e disponibilità DNS
RISOLTO con loop di attesa sullo stato del container