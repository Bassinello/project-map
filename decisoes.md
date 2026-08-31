# Decisões (ADR Log)

Formato inspirado em MADR (Markdown Architectural Decision Records). Cada decisão tem status, contexto, decisão e consequências. IDs são estáveis e nunca reaproveitados mesmo se a decisão for revertida (uma reversão vira um novo ADR que supersede o anterior).

---

## AD-001 — Framework de frontend

- **Status:** Aceita (2026-08-29)
- **Contexto:** Escolher entre React, Vue, Svelte para a SPA.
- **Decisão:** React 19 + Vite.
- **Justificativa:** Ecossistema maior, mais recursos de aprendizado, `create-vite` oferece dev server rápido.
- **Consequências:** Sem SSR/roteamento por padrão (aceito, MVP é single-page). Se crescer para múltiplas telas, adicionar `react-router` será uma decisão nova.

## AD-002 — Banco de dados

- **Status:** Aceita (2026-08-29)
- **Contexto:** MongoDB local vs. Docker vs. Atlas (cloud).
- **Decisão:** MongoDB local via `winget`.
- **Justificativa:** Zero dependência de container/rede externa; simplicidade para ambiente de aprendizado.
- **Consequências:** Sem isolamento (dados vivem na máquina do desenvolvedor); sem backup automático; migração futura para Atlas/Docker exige apenas trocar `MONGODB_URL`.

## AD-003 — Framework backend

- **Status:** Aceita (2026-08-29)
- **Contexto:** FastAPI vs. Flask vs. Django.
- **Decisão:** FastAPI + Uvicorn.
- **Justificativa:** Validação automática via Pydantic, OpenAPI/Swagger gerado automaticamente, suporte assíncrono nativo.
- **Consequências:** Chamadas ao MongoDB via `pymongo` síncrono não aproveitam o event loop async do FastAPI (ver risco R-002). Migrar para `motor` (driver async) é uma evolução possível, não decidida ainda.

## AD-004 — Escopo do MVP: sem autenticação

- **Status:** Aceita (2026-08-29)
- **Contexto:** Definir superfície do MVP (5 capacidades core: create, list, get, update, delete + health).
- **Decisão:** CRUD completo, single-user, sem login.
- **Justificativa:** Validar o fluxo fim-a-fim antes de investir em auth.
- **Consequências:** Qualquer cliente na rede local com acesso à porta 8000 pode ler/escrever tarefas (NFR-002). Não expor a API fora de `localhost` sem adicionar autenticação antes.

## AD-005 — Sem camada de serviço/repositório no backend

- **Status:** Aceita implicitamente (código atual, formalizada em 2026-08-31)
- **Contexto:** As rotas em `main.py` chamam `tasks_collection` diretamente, sem repositório/service intermediário.
- **Decisão:** Manter acoplamento direto rota → coleção Mongo enquanto o projeto for Small/Medium.
- **Justificativa:** Menos indireção para um CRUD de 1 entidade; YAGNI.
- **Consequências:** Se uma segunda entidade ou regra de negócio não trivial for adicionada, revisitar esta decisão e introduzir uma camada de repositório antes que a duplicação apareça.

---

## Riscos abertos

| ID | Risco | Impacto | Mitigação proposta |
| --- | --- | --- | --- |
| R-001 | `DELETE /api/tasks` remove todas as tarefas sem confirmação nem autenticação | Perda de dados acidental | Remover em produção ou proteger com flag de ambiente/confirmação |
| R-002 | Driver `pymongo` síncrono dentro de rotas `async def` do FastAPI | Pode bloquear o event loop sob carga | Avaliar migração para `motor` se a carga aumentar |
| R-003 | `requirements.txt` desatualizado vs. ambiente real instalado (STATE.md) | Build não reprodutível em máquina nova | Rodar `pip freeze > requirements.txt` e fixar versões reais |
| R-004 | Sem índices além de `_id`; `list_tasks` não filtra por `completed` | Degradação de performance conforme a coleção cresce | Adicionar índice em `completed` quando filtro for implementado |
| R-005 | Sem testes automatizados (backend e frontend) | Regressões silenciosas | Priorizar antes da próxima feature funcional relevante |
