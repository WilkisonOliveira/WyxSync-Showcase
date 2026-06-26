<div align="center">

<img src="https://github.com/user-attachments/assets/cb9ddf30-c094-4891-95db-3a8c8eff290a" alt="WyxSync Logo" width="120" />

# WyxSync

**Plataforma fitness com IA — do biotipo ao objetivo, a Wyx cuida de tudo.**

[![Deploy Status](https://img.shields.io/badge/frontend-vercel-black?logo=vercel)](https://wyxsync-web.vercel.app)
[![Backend](https://img.shields.io/badge/backend-railway-blueviolet?logo=railway)](https://railway.app)
[![Stack](https://img.shields.io/badge/stack-ASP.NET%20Core%208%20%2B%20React-blue)](https://github.com/WilkisonOliveira)
[![Status](https://img.shields.io/badge/status-MVP%20em%20produção-brightgreen)]()
[![License](https://img.shields.io/badge/license-MIT-green)]()

[🌐 Acessar App](https://wyxsync-web.vercel.app) · [📋 Reportar Bug](https://wyxsync-web.vercel.app) · [💬 Contato](https://wa.me/5532991511457)

</div>

---

## 📌 Sobre o Projeto

WyxSync é uma plataforma fitness completa desenvolvida com foco em **experiência mobile-first**, integrando treinos personalizados, controle nutricional e um **assistente de IA** (Wyx) que acompanha a evolução do usuário e oferece recomendações baseadas em seu biotipo e objetivos.

O projeto nasceu como solução para um problema real: a fragmentação de ferramentas usadas por praticantes de atividade física — planilhas de treino, apps de dieta e chatbots de IA separados, sem integração entre si. O WyxSync centraliza tudo em uma única experiência coesa.

> **Este é meu principal projeto de portfólio.** Desenvolvi todas as camadas do sistema — arquitetura, backend, frontend, integrações e deploy — de forma independente, aplicando na prática os princípios de engenharia de software que venho estudando.

---

## ✨ Funcionalidades Principais

### 🔐 Autenticação e Segurança
- Cadastro com verificação de e-mail via [Resend](https://resend.com)
- Login com e-mail/senha e autenticação via Google OAuth 2.0
- Controle de sessão com **JWT + Refresh Token**
- Rate limiting nas rotas de autenticação para prevenção de ataques de força bruta

### 🏋️ Gestão de Treinos
- Agenda semanal com filtros por objetivo e grupo muscular
- Treinos personalizados gerados ou adaptados pela IA
- Controle de progresso com histórico de sessões

### 🥗 Módulo Nutricional
- Controle de calorias e macronutrientes por refeição
- Registro diário com histórico acumulado
- Upload de imagem para análise alimentar via IA

### 🤖 Assistente Virtual Wyx (IA)
- Chat integrado com contexto do usuário (biotipo, objetivo, histórico)
- Sugestões de treinos e refeições personalizadas
- Integração com **OpenAI GPT** e **Groq AI** (fallback para latência otimizada)
- Interação por texto e voz

### 💳 Sistema de Assinatura
- Plano gratuito com limite de uso monitorado (429 handling no frontend)
- Plano premium com pagamento via **Mercado Pago** (Checkout Pro)
- Controle de limites por plano direto no backend (sem dependência do frontend)

---

## 🎬 Demonstração

[![Demo WyxSync](https://img.youtube.com/vi/nheyalTu7_o/maxresdefault.jpg)](https://youtu.be/nheyalTu7_o)

> Demonstração completa do fluxo: autenticação, dashboard, treinos, nutrição e assistente Wyx.

<details>
<summary><strong>Ver screenshots do produto</strong></summary>

### Login
> Autenticação com e-mail/senha ou Google OAuth. JWT emitido no backend com expiração configurável.

<img width="600" alt="Tela de login" src="https://github.com/user-attachments/assets/cb9ddf30-c094-4891-95db-3a8c8eff290a" />

### Cadastro
> Fluxo de registro com validação de campos e envio de e-mail de verificação via Resend API.

<img width="600" alt="Tela de cadastro" src="https://github.com/user-attachments/assets/ec5ee414-afb0-4b53-8cb7-4a86869047b1" />

### Dashboard
> Visão consolidada do progresso do usuário — treinos da semana, calorias e interação com a Wyx.

<img width="300" alt="Dashboard" src="https://github.com/user-attachments/assets/1e1fa036-9db4-4aaa-b365-0ad6aca70932" />

### Módulo de Treinos
> Agenda semanal com treinos personalizados, filtros por objetivo e controle de progresso.

<img width="300" alt="Treinos" src="https://github.com/user-attachments/assets/9b67e68c-9556-4b80-bb10-aa8d799e8c58" />

### Módulo Nutricional
> Registro de refeições com contagem de macros e calorias diárias.

<img width="300" alt="Nutrição" src="https://github.com/user-attachments/assets/852ed5bd-a57c-4df1-b207-d24a57fc96e1" />

### Assistente Virtual Wyx
> Chat com IA contextualizada — a Wyx conhece o histórico, biotipo e objetivo do usuário para gerar respostas relevantes.

<img width="350" alt="Assistente IA" src="https://github.com/user-attachments/assets/15a296d1-0a70-4b34-a88d-8973c49cc65e" />

</details>

---

## 🏗️ Arquitetura

```
┌─────────────────────────────────────────────────────┐
│                    CLIENT LAYER                     │
│  React + Vite (SPA)  ──  Capacitor (Android APK)    │
└─────────────────────────┬───────────────────────────┘
                          │ HTTPS / REST
┌─────────────────────────▼───────────────────────────┐
│                    API LAYER                        │
│            ASP.NET Core 8 (Railway)                 │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────┐   │
│  │  Auth    │  │  Domain  │  │  Subscription    │   │
│  │  Module  │  │  Logic   │  │  Module          │   │
│  │  + JWT   │  │  (EF     │  │  (Mercado Pago)  │   │
│  │  + OAuth │  │   Core)  │  │                  │   │
│  └──────────┘  └──────────┘  └──────────────────┘   │
└────────┬─────────────────────────┬──────────────────┘
         │                         │
┌────────▼──────────┐   ┌──────────▼─────────────────┐
│   DATA LAYER      │   │      EXTERNAL SERVICES     │
│  PostgreSQL       │   │  OpenAI API  (GPT)         │
│  (Railway)        │   │  Groq AI     (fallback)    │
│                   │   │  Google OAuth 2.0          │
│                   │   │  Resend      (e-mail)      │
│                   │   │  Mercado Pago (payments)   │
└───────────────────┘   └────────────────────────────┘
```

**Decisões de design:**
- **Separação frontend/backend em repositórios distintos** — permite deploy independente e ciclos de release separados.
- **EF Core com PostgreSQL** — migrations versionadas, sem SQL manual na aplicação.
- **JWT stateless** com refresh token — sem session storage no servidor, escalável horizontalmente.
- **OpenAI como fallback** — reduz latência e custo em cenários de alta demanda.
- **Capacitor** para empacotar o React como APK sem reescrever em React Native.

---

## 🛠️ Stack Tecnológica

| Camada | Tecnologia | Versão | Propósito |
|--------|-----------|--------|-----------|
| Frontend | React | 18 | SPA com roteamento e estado local |
| Frontend | Vite | 5 | Build tool, HMR em dev |
| Frontend | Capacitor | 5 | Empacotamento Android (APK/AAB) |
| Backend | ASP.NET Core | 8 | API REST, DI, middleware pipeline |
| Backend | Entity Framework Core | 8 | ORM, migrations, repositórios |
| Backend | JWT Bearer | — | Autenticação stateless |
| Banco de Dados | PostgreSQL | 15 | Persistência principal |
| Pagamento | Mercado Pago | — | Checkout Pro, webhooks |
| IA Principal | OpenAI API | GPT-5o | Assistente Wyx, análise nutricional |
| IA Fallback | OpenAI API | — | Latência otimizada, custo reduzido |
| Auth Social | Google OAuth 2.0 | — | Login social |
| E-mail | Resend | — | Verificação de conta |
| Deploy Frontend | Vercel | — | CI/CD automático via GitHub |
| Deploy Backend | Railway | — | Container, env vars, PostgreSQL |
| Containerização | Docker | — | Ambiente reproduzível |
| Versionamento | Git + GitHub | — | Controle de versão, histórico |

---

## ⚙️ Como Rodar Localmente

### Pré-requisitos

- [Node.js 20+](https://nodejs.org)
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [PostgreSQL 15+](https://www.postgresql.org) ou [Docker](https://docker.com)
- [Git](https://git-scm.com)

## 🚀 Demonstração

O WyxSync encontra-se em fase final de preparação para lançamento.

### Principais recursos disponíveis

* Autenticação com e-mail e Google
* Controle de treinos e agenda semanal
* Monitoramento nutricional
* Registro de refeições por imagem
* Assistente virtual com IA (Wyx)
* Controle de planos Free e Premium
* Sistema de notificações
* Compartilhamento de treinos
* Acompanhamento de progresso

### Tecnologias Utilizadas

**Frontend**

* React
* JavaScript
* Vite
* Capacitor

**Backend**

* ASP.NET Core
* Entity Framework Core
* PostgreSQL
* JWT Authentication

**Infraestrutura**

* Vercel
* Railway
* Docker

**Integrações**

* Google OAuth
* IA Generativa
* Resend
* Mercado Pago


## 🚧 Desafios Técnicos Enfrentados

### 1. Migração SQLite → PostgreSQL em produção
O projeto foi iniciado com SQLite para agilizar o desenvolvimento local. A migração para PostgreSQL em produção expôs incompatibilidades de tipo (`DateTime` vs `timestamptz`) e comportamentos diferentes de comparação de strings. Solução: auditoria completa das migrations e ajuste das queries com `DateTimeKind.Utc` explícito.

### 2. Concorrência e race conditions no registro de usuário
O fluxo de cadastro gerava duplicação ocasional de usuários quando duas requisições chegavam em paralelo antes do `SaveChanges`. Solução: índice único na coluna `email` no PostgreSQL + tratamento da exceção `DbUpdateException` no controller para retornar 409 Conflict de forma controlada.

### 3. Controle de limites do plano gratuito sem depender do frontend
Implementar o controle de uso (chamadas à IA, registros de treino) apenas no backend, retornando 429 com payload estruturado, e tratar esse status no frontend com feedback visual adequado — sem bloquear o usuário de forma abrupta.

### 4. Latência da OpenAI em dispositivos móveis
Chamadas à OpenAI com prompts longos geravam timeouts perceptíveis no mobile. Solução: implementação do Groq AI como fallback com critério de seleção por complexidade do prompt, reduzindo a latência média de ~4s para ~1.2s nos casos mais simples.

### 5. Rotação de credenciais após exposição em repositório público
Durante uma sessão de deploy, chaves de API foram commiteadas acidentalmente. Processo de resposta: rotação imediata de todas as credenciais (OpenAI, Mercado Pago, JWT secret, Google OAuth), remoção do histórico via `git filter-repo`, e implementação de `.env` no `.gitignore` de todos os repositórios.

---

## 🔑 Diferenciais Técnicos

- **IA com contexto persistente**: a Wyx não é um chatbot genérico — ela recebe o perfil do usuário (biotipo, objetivo, histórico de treinos e nutrição) em cada chamada, gerando respostas personalizadas.
- **Plano free Premium real com billing**: controle de uso por plano implementado no backend, com integração de pagamento via Mercado Pago Checkout Pro e webhook para ativação automática do premium.
- **Mobile-first com distribuição nativa**: o app é construído em React mas empacotado como APK via Capacitor, com plano de publicação na Google Play Store com Google Play Billing.
- **Segurança em produção**: rate limiting em endpoints de auth, verificação de e-mail obrigatória, rotação de secrets documentada, headers de segurança no backend.
- **Deploy totalmente automatizado**: push para `main` dispara build e deploy automático no Vercel (frontend) e Railway (backend).

---

## 🗺️ Roadmap

| Status | Feature |
|--------|---------|
| ✅ Concluído | MVP funcional em produção |
| ✅ Concluído | Sistema de pagamento Mercado Pago |
| ✅ Concluído | Verificação de e-mail |
| ✅ Concluído | Rate limiting em auth |
| ✅ Concluído | Notificações push (FCM) |
| ✅ Concluído | Análise de progresso com gráficos |
| ✅ Concluído | Compartilhamento de treinos entre usuários |
| 🔄 Em progresso | Publicação na Google Play Store |
| 🔄 Em progresso | Google Play Billing (in-app purchase) |
| 📋 Planejado | Domínio customizado `wyxsync.com.br` |



---

## 👨‍💻 Desenvolvedor

**Wilkison Cândido de Oliveira Silva**

Desenvolvedor Full Stack com foco em **C# / ASP.NET Core** e **React**, cursando Análise e Desenvolvimento de Sistemas na Newton Paiva (previsão 2027). Tenho 3 anos de experiência prévia em suporte técnico e infraestrutura de TI no Exército Brasileiro, o que me dá uma perspectiva prática sobre ambientes de produção, confiabilidade de sistemas e resolução de problemas sob pressão.

O WyxSync é meu projeto principal de portfólio — construído de ponta a ponta por mim: arquitetura, backend, frontend, integrações com APIs externas, deploy e produto.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-wilkisonoliveira-0077B5?logo=linkedin)](https://linkedin.com/in/wilkisonoliveira)
[![GitHub](https://img.shields.io/badge/GitHub-WilkisonOliveira-181717?logo=github)](https://github.com/WilkisonOliveira)
[![E-mail](https://img.shields.io/badge/Email-wilkisonoliveira0%40gmail.com-D14836?logo=gmail)](mailto:wilkisonoliveira0@gmail.com)

---


<div align="center">
<sub>Desenvolvido com foco, disciplina e muita iteração. ⚙️</sub>
</div>
