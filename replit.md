# Gnexus eSocial

Sistema web para automação dos eventos eSocial S-2500 e S-2501, desenvolvido para escritórios contábeis e jurídicos.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — rodar o servidor API (porta 8080)
- `pnpm --filter @workspace/esocial run dev` — rodar o frontend (porta configurada por PORT)
- `pnpm run typecheck` — typecheck completo em todos os pacotes
- `pnpm run build` — typecheck + build de todos os pacotes
- `pnpm --filter @workspace/api-spec run codegen` — regenerar hooks e schemas Zod do OpenAPI spec
- `pnpm --filter @workspace/db run push` — aplicar mudanças no schema do DB (apenas dev)
- Required env: `DATABASE_URL` — string de conexão Postgres

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)
- Frontend: React + Vite + TailwindCSS + shadcn/ui

## Where things live

- `lib/api-spec/openapi.yaml` — contrato OpenAPI (source of truth)
- `lib/api-client-react/src/generated/` — hooks React Query gerados pelo Orval
- `lib/api-zod/src/generated/` — schemas Zod gerados pelo Orval
- `lib/db/src/schema/` — schema do banco de dados (Drizzle ORM)
- `artifacts/api-server/src/routes/` — rotas do servidor Express
- `artifacts/esocial/src/pages/` — páginas do frontend React
- `artifacts/esocial/src/context/AuthContext.tsx` — contexto de autenticação
- `attached_assets/` — logos e assets do projeto

## Architecture decisions

- Auth por sessão (express-session + bcrypt) — sem JWT
- eSocial XML gerado server-side com assinatura digital via certificado .pfx (node-forge + xml-crypto)
- Schema multi-cliente: funcionários e transmissões são vinculados ao campo `cliente`
- Ambiente dual: homologação e produção do eSocial configurável por transmissão
- Consulta de protocolo via webservice eSocial (SOAP) após envio

## Product

- Landing page pública com apresentação dos serviços
- Login com autenticação por sessão
- Dashboard com resumo de transmissões e estatísticas
- Gestão de clientes e funcionários (importação via planilha Excel/CSV)
- Criação e envio de transmissões S-2500/S-2501 ao eSocial
- Consulta de status de protocolo no webservice eSocial
- Upload e gestão de certificados digitais (.pfx)
- Formulários de processo (preenchidos pelos clientes)
- Painel administrativo (clientes, escritórios, usuários)

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- Sempre rodar `pnpm --filter @workspace/api-spec run codegen` após mudanças no openapi.yaml
- DB schema push: `pnpm --filter @workspace/db run push`
- Assets de imagem do projeto estão em `attached_assets/` (mapeado como `@assets` no Vite)
- O servidor API usa pino para logs — usar `req.log` em route handlers, nunca `console.log`

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
