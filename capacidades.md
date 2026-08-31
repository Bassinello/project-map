# Mapa de Capacidades

Este é o **documento-mestre** do `project-map`: um índice único e consolidado das capacidades do sistema `from-scratch`, feito para ser lido tanto por humanos quanto por agentes de IA antes de qualquer alteração de código. Sua contraparte legível por máquina é [capabilities.json](capabilities.json) — mesmo conteúdo, formato estruturado.

> **Para agentes de IA:** leia este arquivo (ou `capabilities.json`) antes de propor mudanças no `from-scratch`. Ele responde "o que existe, onde vive, e por quê" sem precisar varrer o repositório de código inteiro. Se a capacidade que você precisa alterar não está listada aqui, o mapa está desatualizado — atualize-o como parte da mudança (ver "Regra de atualização" no [README](README.md)).

## Como este mapa foi montado

| Fonte de verdade | O que fornece |
| --- | --- |
| [contexto.md](contexto.md) | Problema, público, resultado esperado, fora de escopo |
| [requisitos.md](requisitos.md) | REQs/ACs em EARS + tabela de rastreabilidade REQ → endpoint → arquivo |
| [arquitetura.md](arquitetura.md) | C4 (contexto/container/componente), stack, estilo arquitetural |
| [modelo-de-dados.md](modelo-de-dados.md) | ER diagram, schema campo a campo |
| [fluxos.md](fluxos.md) | Sequence diagrams por fluxo observável |
| [decisoes.md](decisoes.md) | ADR log (MADR) + registro de riscos |
| [capabilities.json](capabilities.json) | Tudo acima, normalizado em JSON, com IDs cruzados |

## Matriz de capacidades

| ID | Capacidade | REQ | Status | Camadas | Superfície |
| --- | --- | --- | --- | --- | --- |
| F-001 | Criar tarefa | REQ-001 | ✅ done | backend + frontend | `POST /api/tasks` |
| F-002 | Listar tarefas | REQ-002 | ✅ done | backend + frontend | `GET /api/tasks` |
| F-003 | Obter tarefa por ID | REQ-003 | ✅ done | backend | `GET /api/tasks/{id}` |
| F-004 | Concluir/editar tarefa | REQ-004 | ✅ done | backend + frontend | `PATCH /api/tasks/{id}` |
| F-005 | Excluir tarefa | REQ-005 | ✅ done | backend + frontend | `DELETE /api/tasks/{id}` |
| F-006 | Health check | REQ-006 | ✅ done | backend | `GET /health`, `GET /` |
| F-007 | Interface web de tarefas | REQ-007 | ✅ done | frontend | `App.jsx` |
| F-008 | Autenticação/autorização | — | ⬜ not-started | backend + frontend | — |
| F-009 | Testes automatizados | — | ⬜ not-started | backend + frontend | — |
| F-010 | Filtros/prazos/categorias | — | ⬜ not-started | backend + frontend | — |

## Superfície de API (contrato)

| Método | Rota | Request | Response | Códigos |
| --- | --- | --- | --- | --- |
| GET | `/` | — | `{message, status, version}` | 200 |
| GET | `/health` | — | `{status, database}` | 200, 503 |
| POST | `/api/tasks` | `TaskCreate` | `Task` | 201, 422 |
| GET | `/api/tasks` | query `skip`, `limit` | `Task[]` | 200 |
| GET | `/api/tasks/{id}` | — | `Task` | 200, 400, 404 |
| PATCH | `/api/tasks/{id}` | `TaskUpdate` (parcial) | `Task` | 200, 400, 404 |
| DELETE | `/api/tasks/{id}` | — | — | 204, 400, 404 |
| DELETE | `/api/tasks` | — | — | 204 (⚠️ risco R-001) |

Nenhum endpoint exige autenticação (NFR-002, AD-004). CORS liberado para `localhost:5173` e `localhost:3000` (NFR-001).

## Estado do projeto (snapshot)

- **Escopo:** Small–Medium (CRUD de 1 entidade, 2 camadas, sem auth).
- **Maturidade:** MVP funcional ponta a ponta, validado manualmente; sem suíte de testes automatizada.
- **Dívidas conhecidas:** ver seção "Riscos abertos" em [decisoes.md](decisoes.md) (R-001 a R-005).
- **Próximas capacidades candidatas (não iniciadas):** autenticação (F-008), testes automatizados (F-009), filtros/prazos/categorias (F-010).

## Regras de manutenção deste mapa

1. Toda nova capacidade (endpoint, entidade, fluxo) ganha um REQ em [requisitos.md](requisitos.md) **antes** de ser implementada, e uma linha na matriz acima + em `capabilities.json` **depois**.
2. Toda decisão técnica relevante (escolha de lib, padrão, trade-off) vira um ADR em [decisoes.md](decisoes.md) — nunca um comentário perdido em código ou chat.
3. `capacidades.md` (humano) e `capabilities.json` (máquina) mudam **juntos**, no mesmo commit — nunca um sem o outro.
4. Se uma capacidade for removida/depreciada, mude seu `status` para `deprecated` (não delete a linha) para preservar histórico de rastreabilidade.
