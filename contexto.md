# Contexto do Projeto

**Projeto de código:** `from-scratch` (`c:\Projetos\from-scratch`)
**Este mapa:** `project-map` (`c:\Projetos\project-map`) — não versiona código, versiona a visão estrutural.

## Problema

Para pessoas que precisam organizar tarefas do dia a dia e perdem o controle do que já foi feito, vamos criar um gerenciador de tarefas simples (backend + frontend), para que consigam criar, listar, concluir e excluir tarefas com persistência real em banco de dados.

## Público principal

Usuário único, local, sem autenticação — perfil de projeto de aprendizado/MVP pessoal (não multiusuário, não multi-tenant).

## Resultado esperado

Um fluxo CRUD completo, validado ponta a ponta (frontend React consumindo API FastAPI, dados persistidos em MongoDB), servindo como base extensível para features futuras (autenticação, filtros, prazos, etc.).

## Fora de escopo (MVP atual)

- Autenticação / autorização / multiusuário
- Deploy em produção, containers, CI/CD
- Testes automatizados (backend/frontend ainda não têm suíte de testes)
- Internacionalização (UI em pt-BR fixo)

## Como este mapa se relaciona com o código

| Este repositório (`project-map`) | Repositório de código (`from-scratch`) |
| --- | --- |
| Intenção, requisitos, arquitetura, decisões | Implementação, dependências, execução |
| Atualizado quando o **comportamento ou desenho** muda | Atualizado a cada commit de feature |
| Fonte de verdade para "o que o sistema faz e por quê" | Fonte de verdade para "como o sistema roda" |

Consulte [capacidades.md](capacidades.md) para o mapa de capacidades consolidado (visão para humanos e para agentes de IA) e [capabilities.json](capabilities.json) para a versão estruturada/legível por máquina do mesmo mapa.
