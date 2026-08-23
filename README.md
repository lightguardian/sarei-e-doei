<div align="center">
  <img src="./logo.png" alt="Sarei e Doei" width="480" />
  <br/>
  <br/>

  <strong>Plataforma solidária de empréstimo e doação de equipamentos médicos de apoio.</strong>

  <br/>
  <br/>

  [![CI - Develop](https://github.com/lightguardian/sarei-e-doei/actions/workflows/ci.yml/badge.svg?branch=develop)](https://github.com/lightguardian/sarei-e-doei/actions/workflows/ci.yml)
  [![CI - Main](https://github.com/lightguardian/sarei-e-doei/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/lightguardian/sarei-e-doei/actions/workflows/ci.yml)
  ![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=flat&logo=nestjs&logoColor=white)
  ![Vue.js](https://img.shields.io/badge/Vue.js%203-4FC08D?style=flat&logo=vue.js&logoColor=white)
  ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat&logo=postgresql&logoColor=white)
  ![Prisma](https://img.shields.io/badge/Prisma-2D3748?style=flat&logo=prisma&logoColor=white)

  <br/>

  🔗 **[Produção em breve]** &nbsp;·&nbsp; 📋 **[Kanban em breve]** &nbsp;·&nbsp; 📖 **[Swagger em breve]**

</div>

---

## O Problema

O empréstimo e doação de equipamentos médicos de apoio — muletas, cadeiras de rodas, andadores, camas hospitalares — acontece hoje de forma totalmente manual: via WhatsApp, ONGs presenciais e formulários físicos. Não existe uma plataforma digital centralizada, aberta e acessível para isso.

## A Solução

O **Sarei e Doei** conecta quem tem equipamentos parados em casa com quem precisa deles temporariamente. Quem sarou, doa. Quem precisa, solicita. Com fila gerenciada, chat direto entre doador e receptor, e histórico de transferências.

---

## Documentação Oficial

Toda a especificação do sistema está versionada em `/docs`:

| Documento | Descrição |
|---|---|
| 📋 [PRD — Product Requirements Document](./docs/prd.md) | Visão do produto, atores, casos de uso, user stories e regras de negócio |
| 📐 [SDD — Software Design Document](./docs/sdd.md) | Diagrama ER (Mermaid), módulos NestJS, contratos da API e DTOs |
| ✅ [Checklist de Avaliação](./docs/checklist.md) | Controle de entrega dos IDs e RAs da disciplina |

---

## Stack Tecnológica

| Camada | Tecnologia |
|---|---|
| Backend | [NestJS](https://nestjs.com) + [Prisma ORM](https://prisma.io) |
| Banco de Dados | [PostgreSQL](https://postgresql.org) — hospedado no [Neon.tech](https://neon.tech) |
| Frontend | [Vue.js 3](https://vuejs.org) (SPA) |
| Autenticação | JWT (Bearer Token) |
| Tempo Real | WebSocket via [Socket.io](https://socket.io) |
| Containerização | [Docker](https://www.docker.com/) + Docker Compose |
| Ambiente de Desenvolvimento | Docker Compose + PostgreSQL |
| Deploy | Docker + Render |
| Cron Jobs | `@nestjs/schedule` |
---

## Infraestrutura e Containerização

O projeto utiliza **Docker** e **Docker Compose** para padronizar os ambientes de desenvolvimento e produção.

A decisão de utilizar containers foi tomada para garantir que o ambiente de execução da aplicação seja reproduzível independentemente do sistema operacional utilizado pelo desenvolvedor, evitando diferenças de versões e dependências entre ambientes.

São mantidas configurações distintas para cada finalidade:

- `compose.dev.yml` — ambiente de desenvolvimento, com hot reload e PostgreSQL local;
- `compose.prod.yml` — ambiente de produção, utilizando imagens otimizadas e conexão com o PostgreSQL hospedado no Neon.

Cada aplicação possui seu próprio `Dockerfile`:

- `apps/api/Dockerfile` — imagem do backend NestJS;
- `apps/web/Dockerfile` — imagem do frontend Vue.js.

O Docker Compose é responsável por orquestrar os serviços, enquanto o `Makefile` fornece comandos simplificados para operações recorrentes, como iniciar, parar, construir e consultar os logs dos containers.

A containerização também permite que o mesmo processo de build utilizado no desenvolvimento seja reproduzido no ambiente de deploy, reduzindo divergências entre os ambientes.

## Estrutura do Monorepo

```
sarei-e-doei/
├── README.md
├── package.json
├── .env.example
├── .gitignore
├── Makefile
├── compose.dev.yml
├── compose.prod.yml
│
├── apps/
│   ├── api/                  # Backend NestJS
│   └── web/                  # Frontend Vue.js 3
│
├── docker/
│   ├── api/
│   │   └── Dockerfile        # Imagem do backend
│   └── web/
│       └── Dockerfile        # Imagem do frontend
│
├── docs/
│   ├── prd.md                # Requisitos e regras de negócio
│   ├── sdd.md                # Arquitetura e design do sistema
│   └── checklist.md          # Indicadores de desempenho
│
└── specs/
    └── ...                   # Especificações das histórias
```

---

## Quick Start

**Pré-requisitos:** Node.js 20+ · npm 10+

```bash
git clone https://github.com/lightguardian/sarei-e-doei.git
cd sarei-e-doei
npm install
```

**Backend:**
```bash
cd apps/api
cp .env.example .env
# configure as variáveis de ambiente
npx prisma migrate dev
npm run start:dev
```

**Frontend:**
```bash
cd apps/web
npm run dev
```

---

## Funcionalidades Principais

- **Feed público** de equipamentos disponíveis — sem necessidade de login
- **Fila de solicitação** gerenciada e anônima
- **Chat privado** entre doador e receptor com criptografia em repouso (AES-256-GCM)
- **Expiração automática** de vagas via cron job (1 semana para retirada)
- **Confirmação dupla** de transferência (doador + receptor)
- **Indisponibilidade** com fila congelada (ninguém perde a posição)
- **Painel admin** com histórico completo e gestão de tipos de equipamento

---

## Branches

| Branch | Propósito |
|---|---|
| `main` | Produção estável |
| `develop` | Integração contínua |
| `feature/*` | Features isoladas via Pull Requests |
