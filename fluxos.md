# Fluxos

Diagramas de sequência (Mermaid `sequenceDiagram`), padrão UML, para os fluxos observáveis do sistema. Cada fluxo referencia o REQ correspondente em [requisitos.md](requisitos.md).

## Fluxo 1 — Criar tarefa (REQ-001, REQ-007)

```mermaid
sequenceDiagram
    actor U as Usuário
    participant F as Frontend (App.jsx)
    participant A as api.js (axios)
    participant B as Backend (FastAPI)
    participant M as MongoDB

    U->>F: Preenche título + descrição, clica "Adicionar"
    F->>F: valida campos não vazios (client-side)
    F->>A: createTask({title, description})
    A->>B: POST /api/tasks
    B->>B: valida payload (Pydantic TaskCreate)
    B->>M: insert_one(task_dict)
    M-->>B: inserted_id
    B->>M: find_one({_id})
    M-->>B: documento criado
    B-->>A: 201 Created + Task
    A-->>F: task criada
    F->>A: listTasks() (recarrega lista)
    A->>B: GET /api/tasks
    B-->>F: lista atualizada
    F-->>U: exibe nova tarefa na lista
```

## Fluxo 2 — Listar tarefas ao carregar a página (REQ-002, REQ-007)

```mermaid
sequenceDiagram
    actor U as Usuário
    participant F as Frontend (App.jsx)
    participant B as Backend
    participant M as MongoDB

    U->>F: Abre a aplicação
    F->>F: useEffect → loadTasks()
    F->>B: GET /api/tasks
    B->>M: find().skip(0).limit(100)
    M-->>B: documentos
    B-->>F: 200 OK + array de tarefas
    alt erro de rede/backend fora do ar
        F-->>U: mensagem "Não foi possível carregar as tarefas..."
    else sucesso
        F-->>U: renderiza lista (ou "Nenhuma tarefa ainda")
    end
```

## Fluxo 3 — Concluir/reabrir tarefa (REQ-004, REQ-007)

```mermaid
sequenceDiagram
    actor U as Usuário
    participant F as Frontend
    participant B as Backend
    participant M as MongoDB

    U->>F: Clica checkbox da tarefa
    F->>B: PATCH /api/tasks/{id} {completed: !atual}
    B->>B: valida ObjectId
    alt id inválido
        B-->>F: 400 Bad Request
    else id não encontrado
        B-->>F: 404 Not Found
    else ok
        B->>M: find_one_and_update({_id}, {$set: {completed, updated_at}})
        M-->>B: documento atualizado
        B-->>F: 200 OK + Task
        F->>B: GET /api/tasks (recarrega)
    end
```

## Fluxo 4 — Excluir tarefa (REQ-005, REQ-007)

```mermaid
sequenceDiagram
    actor U as Usuário
    participant F as Frontend
    participant B as Backend
    participant M as MongoDB

    U->>F: Clica "Excluir"
    F->>B: DELETE /api/tasks/{id}
    alt id inválido
        B-->>F: 400 Bad Request
    else não encontrado
        B-->>F: 404 Not Found
    else ok
        B->>M: delete_one({_id})
        B-->>F: 204 No Content
        F->>B: GET /api/tasks (recarrega)
    end
```

## Fluxo 5 — Health check (REQ-006)

```mermaid
sequenceDiagram
    participant C as Cliente (curl/monitor)
    participant B as Backend
    participant M as MongoDB

    C->>B: GET /health
    B->>M: admin.command("ping")
    alt Mongo acessível
        M-->>B: pong
        B-->>C: 200 OK {status: saudável}
    else Mongo inacessível
        B-->>C: 503 Service Unavailable {detail: erro}
    end
```
