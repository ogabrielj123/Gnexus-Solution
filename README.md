# Gnexus Solution

Sistema web para automação dos eventos eSocial S-2500 e S-2501, desenvolvido para escritórios contábeis e jurídicos.

🌐 **Website:** [www.gnexussolution.com.br](https://www.gnexussolution.com.br)

## 📋 Sobre o Projeto

**Gnexus Solution** é uma plataforma de automação eSocial que simplifica a geração, assinatura digital e envio de eventos eSocial S-2500 e S-2501. Projetada especificamente para escritórios contábeis e jurídicos, oferece:

- ✅ Dashboard analítico com estatísticas de transmissões
- ✅ Gestão centralizada de clientes e funcionários
- ✅ Importação via planilha (Excel/CSV)
- ✅ Criação e envio de transmissões S-2500/S-2501
- ✅ Assinatura digital automática via certificado .pfx
- ✅ Consulta de status de protocolo no webservice eSocial
- ✅ Multi-cliente com autenticação segura por sessão
- ✅ Suporte a ambientes de homologação e produção

## 🚀 Quick Start

### Pré-requisitos
- Node.js 24+
- pnpm 9+
- PostgreSQL 14+

### Instalação e Desenvolvimento

```bash
# Instalar dependências
pnpm install

# Rodar servidor API (porta 8080)
pnpm --filter @workspace/api-server run dev

# Rodar frontend (porta 5173)
pnpm --filter @workspace/esocial run dev

# Rodar typecheck completo
pnpm run typecheck

# Build completo
pnpm run build
```

### Variáveis de Ambiente

Crie um arquivo `.env` na raiz:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/gnexus
```

## 🏗️ Arquitetura

### Stack Tecnológico

**Backend:**
- Express 5 com Node.js 24
- PostgreSQL + Drizzle ORM
- TypeScript 5.9
- Autenticação por sessão (express-session + bcrypt)
- node-forge para assinatura digital XML

**Frontend:**
- React 19 + Vite
- TailwindCSS v4
- shadcn/ui components
- React Query para data fetching
- Zod para validação

**Monorepo:**
- pnpm Workspaces
- Catálogo de dependências versionadas
- OpenAPI/Orval para code generation

### Estrutura de Diretórios

```
artifacts/
├── api-server/          # Backend Express
│   └── src/routes/      # Endpoints da API
├── esocial/             # Frontend React
│   ├── src/pages/       # Páginas da aplicação
│   └── src/context/     # React Context (auth, etc)

lib/
├── api-spec/            # OpenAPI spec (source of truth)
├── api-client-react/    # Hooks React Query (gerado)
├── api-zod/             # Schemas Zod (gerado)
├── db/                  # Schema PostgreSQL (Drizzle)
└── integrations/        # Clientes externos (eSocial SOAP, etc)

scripts/                 # Utilitários e scripts
```

## 🔒 Segurança

- Schema multi-cliente: dados isolados por `cliente`
- Autenticação por sessão (sem JWT)
- Assinatura digital XML via certificado .pfx
- Validação Zod em todo input
- HTTPS/TLS em produção
- Minimun release age: 1 dia para npm packages (defesa contra supply-chain attacks)

## 📊 Arquitetura de Dados

**Tabelas principais:**
- `users` - Usuários por escritório
- `clientes` - Dados de clientes
- `funcionarios` - Funcionários dos clientes
- `transmissoes` - Histórico de transmissões S-2500/S-2501
- `certificados` - Certificados digitais (.pfx)

Todos os dados são vinculados ao campo `cliente` para isolamento.

## 🔄 Fluxo eSocial

1. **Autenticação**: Login com sessão segura
2. **Upload de Certificado**: Usuário faz upload do .pfx
3. **Criação de Transmissão**: Sistema gera XML com eventos S-2500/S-2501
4. **Assinatura**: XML é assinado digitalmente server-side
5. **Envio**: Transmissão é enviada ao webservice eSocial
6. **Consulta**: Status é consultado e armazenado no DB
7. **Dashboard**: Usuário acompanha em tempo real

## 📝 Comandos Úteis

```bash
# Typecheck
pnpm run typecheck

# Build de todos os pacotes
pnpm run build

# Regenerar code generation (após mudanças no openapi.yaml)
pnpm --filter @workspace/api-spec run codegen

# Database migrations
pnpm --filter @workspace/db run push
pnpm --filter @workspace/db run migrate

# Logs do servidor (pino logger)
# Use req.log em route handlers, nunca console.log
```

## 🤝 Contribuindo

Contribuições são bem-vindas! Por favor:

1. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
2. Commit suas mudanças (`git commit -m 'Add AmazingFeature'`)
3. Push para a branch (`git push origin feature/AmazingFeature`)
4. Abra um Pull Request

## 📄 Licença

MIT - veja LICENSE para detalhes.

## 📞 Contato

- Email: contato@gnexussolution.com.br
- Website: [www.gnexussolution.com.br](https://www.gnexussolution.com.br)
- GitHub: [ogabrielj123/Gnexus-Solution](https://github.com/ogabrielj123/Gnexus-Solution)

---

Desenvolvido com ❤️ para automação contábil.
