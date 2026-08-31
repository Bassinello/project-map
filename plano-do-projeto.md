# Plano do projeto

Use este arquivo como índice do plano de trabalho do projeto descrito por este `project-map`.

## Objetivo

- **Problema que será resolvido:** Pessoas que perdem o controle de tarefas do dia a dia precisam de um jeito simples de criar, listar, concluir e excluir tarefas com persistência real.
- **Público principal:** Usuário único, local, sem autenticação (projeto de aprendizado/MVP pessoal).
- **Resultado esperado:** CRUD completo validado ponta a ponta (React → FastAPI → MongoDB), servindo de base extensível para features futuras.

Detalhamento completo em [contexto.md](contexto.md).

## MVP

1. [x] Criar, listar, concluir e excluir tarefas (backend + frontend)
2. [x] Health check da API e do MongoDB
3. [ ] Autenticação/autorização (fora do MVP atual)

## Mapa de capacidades

O mapa consolidado de tudo o que o sistema faz, com rastreabilidade REQ → endpoint → arquivo, vive em [capacidades.md](capacidades.md) (leitura humana) e [capabilities.json](capabilities.json) (leitura por agentes/ferramentas). Comece por lá para uma visão geral rápida.

## Requisitos

Consulte [requisitos.md](requisitos.md) para os requisitos funcionais (REQ-001 a REQ-007) e critérios de aceite em notação EARS.

## Arquitetura e design

- [arquitetura.md](arquitetura.md) — visão C4 (contexto/container/componente) e stack
- [modelo-de-dados.md](modelo-de-dados.md) — entidades e schema
- [fluxos.md](fluxos.md) — diagramas de sequência dos fluxos principais

## Execução

| Ordem | Entrega | Dependências | Validação | Status |
| --- | --- | --- | --- | --- |
| 1 | Backend CRUD + health check | MongoDB local | Testado manualmente (curl) | ✅ Concluído |
| 2 | Frontend (listar/criar/concluir/excluir) | Backend rodando | `vite build` sem erros, testado manualmente | ✅ Concluído |
| 3 | Autenticação/autorização | — | — | Não iniciado |
| 4 | Testes automatizados (backend + frontend) | — | — | Não iniciado |

## Decisões e riscos

- Registre decisões técnicas em [decisoes.md](decisoes.md) (ADR log).
- Riscos abertos hoje: R-001 a R-005, também em [decisoes.md](decisoes.md).

## Critério de conclusão

O plano estará concluído quando todas as entregas do MVP tiverem critérios de aceite atendidos e validação registrada. O MVP core (entregas 1 e 2) já está concluído; entregas 3 e 4 seguem em aberto.
