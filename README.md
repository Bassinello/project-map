# Project Map

Este repositorio guarda a documentacao tecnica e o planejamento do projeto `from-scratch`. Ele nao e a aplicacao executavel: seu objetivo e explicar o contexto, os requisitos, a arquitetura, os fluxos, o modelo de dados, as decisoes e as proximas entregas.

O mapa fica separado do codigo para que a documentacao possa ser consultada e versionada sem misturar artefatos de planejamento com o backend e o frontend.

## Copiar o projeto

Clone o repositorio de documentacao separadamente:

```bash
cd /c/Projetos
git clone https://github.com/SEU_USUARIO/project-map.git
cd project-map
```

Para atualizar uma copia existente:

```bash
cd /c/Projetos/project-map
git pull origin main
```

Para trabalhar com os dois repositorios lado a lado:

```text
/c/Projetos/
├── from-scratch/  # codigo da aplicacao
└── project-map/   # documentacao e planejamento
```

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
├── decisoes.md
├── capacidades.md         # mapa-mestre de capacidades (leitura humana)
└── capabilities.json      # mesmo mapa, estruturado para agentes/ferramentas
```

Comece a leitura por [capacidades.md](capacidades.md) para uma visao geral rapida do que o sistema faz. Depois consulte [contexto.md](contexto.md) e [requisitos.md](requisitos.md) para entender o problema e os criterios de aceite.

Atualize arquitetura, fluxos e decisoes quando o comportamento ou a estrutura do projeto mudar.

As skills especificas devem ser colocadas em `.github/skills/<nome-da-skill>/SKILL.md`. O plano geral fica em [plano-do-projeto.md](plano-do-projeto.md). As especificacoes da feature de autenticacao ficam em `.specs/features/auth/`.

## Relacao com o codigo

O codigo esta no repositorio `from-scratch`, com este fluxo local:

```text
React/Vite -> Axios -> FastAPI -> PyMongo -> MongoDB
```

O `project-map` descreve esse fluxo, mas nao inicia a API nem o frontend. Para executar a aplicacao, siga o README do repositorio de codigo.

## Publicar separadamente

Se o repositorio remoto ainda nao existir, crie um repositorio vazio chamado `project-map` no GitHub e, a partir desta pasta, execute:

```bash
cd /c/Projetos/project-map
git add .
git commit -m "chore: inicia mapa do projeto"
git branch -M main
git remote add origin https://github.com/SEU_USUARIO/project-map.git
git push -u origin main
```

Para criar uma branch de documentacao:

```bash
git switch main
git pull origin main
git switch -c docs/atualiza-readme
git add .
git commit -m "docs: atualizar guias dos projetos"
git push -u origin docs/atualiza-readme
```

Abra entao um Pull Request de `docs/atualiza-readme` para `main`.

## Regra de atualização

Toda mudança relevante deve responder:

- Qual requisito ou fluxo ela atende?
- Quais partes do sistema são afetadas?
- Como será validada?
- Qual decisão ou risco novo precisa ser registrado?
