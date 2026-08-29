# Project Map

Este é um repositório independente para registrar a visão estrutural, as especificações, as skills e o plano de um projeto usando desenvolvimento orientado por especificação (SDD).

Ele fica ao lado do repositório de código, em `c:\Projetos\project-map`, e não dentro de `from-scratch`.

## Arquivos sugeridos

```text
project-map/
├── README.md
├── plano-do-projeto.md
├── .github/
│   └── skills/
│       └── README.md
├── contexto.md
├── requisitos.md
├── arquitetura.md
├── fluxos.md
├── modelo-de-dados.md
└── decisoes.md
```

Comece pelo contexto e pelos requisitos do MVP. Atualize arquitetura, fluxos e decisões quando o comportamento ou a estrutura do projeto mudar.

As skills específicas devem ser colocadas em `.github/skills/<nome-da-skill>/SKILL.md`. O plano geral fica em [plano-do-projeto.md](plano-do-projeto.md).

## Publicar separadamente

Crie um repositório vazio chamado `project-map` no GitHub e, a partir desta pasta, execute:

```bash
cd /c/Projetos/project-map
git add .
git commit -m "chore: inicia mapa do projeto"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/project-map.git
git push -u origin main
```

## Regra de atualização

Toda mudança relevante deve responder:

- Qual requisito ou fluxo ela atende?
- Quais partes do sistema são afetadas?
- Como será validada?
- Qual decisão ou risco novo precisa ser registrado?
