---
unique-name: 06frontendimplementation
display-name: 06 Frontend Implementation Guide
category: GENERAL
description: Guida di implementazione frontend Angular per la piattaforma AI Dev, con architettura, moduli, routing, API e quality gates.
tags: frontend
---

# Frontend Implementation Guide

## Obiettivo
Implementare il frontend della piattaforma AI Dev in Angular, garantendo usabilità, tracciabilità dei workflow e integrazione robusta con il backend Spring Boot.

## Scope
- Interfaccia utente per SDLC assistito AI (Requirements, Design, Development, Testing, Approval/Deployment).
- Gestione stato delle run/workflow.
- Visualizzazione risultati, quality gate, audit trail e approvazioni.
- Integrazione con API REST backend.

## Prerequisiti
- Node.js LTS (>=20)
- Angular CLI (>=18)
- Accesso all’ambiente backend e specifiche OpenAPI/endpoint

## Architettura Frontend
### Strati
1. **Presentation Layer**
   - Pagine, componenti, layout, design system.
2. **Application Layer**
   - Use case UI, orchestration di chiamate API, gestione workflow UI.
3. **Data Layer**
   - API client, mapper DTO ↔ model, gestione errori HTTP e retry controllati.

### Moduli consigliati
- `core`: config, interceptor, auth/session, error handler.
- `shared`: componenti riusabili (table, form controls, status badge, timeline).
- `features/requirements`
- `features/design`
- `features/development`
- `features/testing`
- `features/approval`
- `features/audit`

## Routing
- `/dashboard`
- `/workflow/:runId`
- `/requirements`
- `/design`
- `/development`
- `/testing`
- `/approval`
- `/audit`

Regole:
- Guard per route protette.
- Resolver per preload dati critici (run detail, stato quality gate).

## Stato applicativo
Adottare un approccio prevedibile (Signals store o NgRx, secondo standard team):
- stato sessione utente
- stato run corrente
- stato step SDLC
- stato approvazioni e quality gate

Pattern:
- stato locale per form semplici
- stato centralizzato per workflow e dati condivisi cross-page

## Contratto API
### Endpoint minimi frontend
- `POST /api/runs` avvio workflow
- `GET /api/runs/{id}` dettaglio run
- `GET /api/runs/{id}/steps` stato step
- `POST /api/runs/{id}/approve` approvazione umana
- `GET /api/runs/{id}/audit` evidenze e audit trail

### Gestione errori
- Mappare codici HTTP in messaggi utente chiari.
- Non nascondere errori di policy/permessi: mostrare motivazione e azione consigliata.

## Sicurezza e governance UI
- Interceptor auth per token/sessione.
- Role-based UI rendering (visibilità azioni per ruolo).
- Conferma esplicita per azioni critiche (approve/reject/deploy).
- Tracciamento eventi UI rilevanti (run start, approve, reject, export evidenze).

## UX minima per MVP
1. Dashboard con stato run e KPI base.
2. Wizard Requirements → Design → Development → Testing.
3. Schermata Approval con evidenze e quality gate.
4. Vista Audit con timeline eventi e filtri.

## Piano di implementazione
1. **Bootstrap progetto**
   - setup Angular, lint, test, env, CI frontend.
2. **Core foundation**
   - layout, routing, interceptor, auth/session, design tokens.
3. **Workflow shell**
   - dashboard, dettaglio run, progress step.
4. **Feature SDLC**
   - requirements/design/development/testing/approval.
5. **Audit & observability UI**
   - timeline eventi, filtri, export base.
6. **Hardening**
   - performance, accessibilità, gestione errori edge case.

## Qualità
- Unit test componenti/servizi critici.
- Integration test per flow principali.
- E2E su scenari core (run completa + approval).
- Gate di qualità su lint, test e coverage minima concordata.

## Definition of Done
- Flusso end-to-end utilizzabile da utente target.
- Step umano di approvazione operativo.
- Audit trail consultabile da UI.
- Error handling consistente.
- Build e test frontend in pipeline CI.

## Note di estendibilità
- Nuove skill e nuovi step SDLC devono poter essere aggiunti via configurazione modulo/route senza refactor invasivo del core UI.
- Mantenere componenti e API client disaccoppiati tramite adapter/mapper.