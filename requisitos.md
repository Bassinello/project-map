# Requisitos

Requisitos escritos em notação EARS (Easy Approach to Requirements Syntax) para serem testáveis e não ambíguos. Cada requisito tem um ID rastreável usado em [capacidades.md](capacidades.md), [capabilities.json](capabilities.json) e no código-fonte (`from-scratch`).

## Funcionais

### REQ-001 — Criar tarefa

- **AC-001:** QUANDO o usuário submete `POST /api/tasks` com `title` (1–200 chars) preenchido, O SISTEMA DEVE criar a tarefa e responder `201 Created` com o objeto criado.
- **AC-002:** QUANDO `title` está ausente ou vazio, O SISTEMA DEVE rejeitar a requisição com `422 Unprocessable Entity`.
- **AC-003:** QUANDO uma tarefa é criada, O SISTEMA DEVE preencher `completed=false`, `created_at` e `updated_at` automaticamente.

### REQ-002 — Listar tarefas

- **AC-001:** QUANDO o usuário submete `GET /api/tasks`, O SISTEMA DEVE responder `200 OK` com um array (vazio se não houver tarefas).
- **AC-002:** O SISTEMA DEVE suportar paginação via `skip` e `limit` (padrão `skip=0`, `limit=100`).

### REQ-003 — Obter tarefa por ID

- **AC-001:** QUANDO o usuário submete `GET /api/tasks/{id}` com um ID válido existente, O SISTEMA DEVE responder `200 OK` com a tarefa.
- **AC-002:** QUANDO o ID não é um ObjectId válido, O SISTEMA DEVE responder `400 Bad Request`.
- **AC-003:** QUANDO o ID é válido mas não existe, O SISTEMA DEVE responder `404 Not Found`.

### REQ-004 — Atualizar tarefa (parcial)

- **AC-001:** QUANDO o usuário submete `PATCH /api/tasks/{id}` com qualquer subconjunto de `{title, description, completed}`, O SISTEMA DEVE atualizar apenas os campos enviados e o `updated_at`.
- **AC-002:** QUANDO o ID não existe, O SISTEMA DEVE responder `404 Not Found`.
- **AC-003:** QUANDO o ID é malformado, O SISTEMA DEVE responder `400 Bad Request`.

### REQ-005 — Excluir tarefa

- **AC-001:** QUANDO o usuário submete `DELETE /api/tasks/{id}` com ID existente, O SISTEMA DEVE remover a tarefa e responder `204 No Content`.
- **AC-002:** QUANDO o ID não existe, O SISTEMA DEVE responder `404 Not Found`.
- **AC-003 (risco):** O endpoint `DELETE /api/tasks` (sem ID) remove **todas** as tarefas sem confirmação — marcado como utilitário de desenvolvimento, não deve ser exposto em produção sem proteção adicional (ver [decisoes.md](decisoes.md), risco R-001).

### REQ-006 — Verificar saúde do sistema

- **AC-001:** QUANDO o usuário submete `GET /health`, O SISTEMA DEVE verificar a conexão com o MongoDB via `ping` e responder `200 OK` se saudável.
- **AC-002:** QUANDO o MongoDB está inacessível, O SISTEMA DEVE responder `503 Service Unavailable` com detalhe do erro.

### REQ-007 — Interface web de tarefas

- **AC-001:** O SISTEMA DEVE exibir a lista de tarefas ao carregar a página, buscando dados em `GET /api/tasks`.
- **AC-002:** O SISTEMA DEVE permitir criar tarefa via formulário (título + descrição obrigatórios no cliente).
- **AC-003:** O SISTEMA DEVE permitir alternar `completed` via checkbox e excluir via botão, recarregando a lista após cada ação.
- **AC-004:** QUANDO a API não responde, O SISTEMA DEVE exibir mensagem de erro amigável ("Não foi possível carregar as tarefas...").

## Não funcionais

### NFR-001 — CORS

O SISTEMA DEVE permitir requisições de `http://localhost:5173` e `http://localhost:3000` (origens padrão do Vite/CRA) com todos os métodos e headers.

### NFR-002 — Ausência de autenticação (aceito para o MVP)

O SISTEMA NÃO IMPLEMENTA autenticação/autorização. Qualquer cliente na rede local com acesso à porta 8000 pode ler/escrever tarefas. Aceito como decisão de escopo (ver AD-004 em [decisoes.md](decisoes.md)).

### NFR-003 — Persistência

O SISTEMA DEVE persistir tarefas em MongoDB (`tasks_db.tasks`), sobrevivendo a reinícios do backend.

## Rastreabilidade

| REQ | Endpoint(s) | Arquivo (from-scratch) | Feature (capacidades.md) |
| --- | --- | --- | --- |
| REQ-001 | `POST /api/tasks` | `backend/main.py::create_task` | Criar tarefa |
| REQ-002 | `GET /api/tasks` | `backend/main.py::list_tasks` | Listar tarefas |
| REQ-003 | `GET /api/tasks/{id}` | `backend/main.py::get_task` | Obter tarefa |
| REQ-004 | `PATCH /api/tasks/{id}` | `backend/main.py::update_task` | Concluir/editar tarefa |
| REQ-005 | `DELETE /api/tasks/{id}`, `DELETE /api/tasks` | `backend/main.py::delete_task`, `delete_all_tasks` | Excluir tarefa |
| REQ-006 | `GET /health`, `GET /` | `backend/main.py::health`, `root` | Health check |
| REQ-007 | UI | `frontend/src/App.jsx`, `frontend/src/api.js` | Interface web |
