# PECOM-WEB

Frontend do **Sistema de Gestão de Turmas e Tarefas** - interface web para professores e alunos, consumindo a [`pecom-api`](../pecom-api/) via REST.

> Visão geral do projeto, contrato de API e ADRs: ver o repositório [`pecom`](../pecom/).

## Stack

| Item | Tecnologia |
|---|---|
| Framework | Next.js (React) + TypeScript |
| Estilização | Tailwind CSS + shadcn/ui |
| Data fetching / cache | TanStack Query (React Query) |
| Formulários | React Hook Form + Zod |
| Editor de código | Monaco Editor (submissão de algoritmos) |
| Client HTTP | Gerado a partir de `pecom/openapi.yaml` |

## Estrutura de pastas

```
pecom-web/
├── app/
│   ├── (auth)/
│   │   ├── login/
│   │   └── registro/
│   ├── (professor)/
│   │   ├── turmas/
│   │   ├── tarefas/
│   │   └── metricas/
│   ├── (aluno)/
│   │   ├── turmas/
│   │   ├── tarefas/
│   │   └── metricas/
│   └── layout.tsx
├── components/
│   ├── ui/                # componentes shadcn/ui
│   └── shared/             # componentes de domínio reutilizáveis
├── lib/
│   ├── api/                 # client HTTP tipado (gerado a partir do OpenAPI)
│   └── hooks/                # hooks de React Query
├── types/                    # tipos TS compartilhados
└── package.json
```

## Rodando localmente

Este repositório normalmente é executado via `docker compose` a partir do repositório [`pecom`](../pecom/). Para rodar isoladamente:

```bash
cp .env.example .env    # NEXT_PUBLIC_API_URL apontando para a api local

npm install
npm run dev
```

## Gerando o client tipado a partir do contrato

```bash
npm run generate:api    # lê platform/openapi.yaml e gera lib/api/
```

Rodar sempre que o contrato em `platform/openapi.yaml` for alterado.

## Testes e lint

```bash
npm run lint
npm run test
```

## Convenções

- Rotas organizadas por grupo de perfil: `(auth)`, `(professor)`, `(aluno)`.
- Componentes de UI genéricos em `components/ui`; componentes de domínio (ex: card de turma) em `components/shared`.
- Toda chamada à API passa pelo client tipado gerado — nunca `fetch` direto nas telas.
- Validação de formulários sempre via Zod, espelhando os schemas do contrato OpenAPI quando aplicável.
- Commits seguindo [Conventional Commits](https://www.conventionalcommits.org/).

## Repositórios relacionados

- [`pecom-api`](../pecom-api) — backend consumido por este frontend
- [`pecom`](../pecom) — contrato de API, ADRs, orquestração local