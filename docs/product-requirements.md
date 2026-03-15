# Product Requirements Document (PRD) - Kairos PDI App

## Visão Geral do Produto

O **Kairos** é uma aplicação moderna de Planejamento de Desenvolvimento Individual (PDI) que ajuda pessoas a converterem objetivos de crescimento pessoal e profissional em caminhos concretos, com ações executáveis e acompanhamento contínuo.

### Objetivo Principal
Ajudar usuários a transformarem metas abstratas de crescimento pessoal ou profissional em planos claros, acionáveis e acompanháveis ao longo do tempo.

### Diferenciais Competitivos
- Foco no desenvolvimento de competências (não apenas conclusão de tarefas)
- Flexível e adaptável ao ritmo de cada pessoa
- Histórico visual de progresso e reflexões
- Interface intuitiva com experiência fluída

## Funcionalidades Principais

### 1. Cadastro de Objetivos
- Permitir que o usuário cadastre objetivos de médio e longo prazo
- Categorização e priorização de objetivos
- Definição de prazos e marcos

### 2. Organização em Planos de Desenvolvimento
- Agrupamento de objetivos relacionados em planos estruturados
- Quebra de objetivos em ações pequenas e executáveis
- Dependências entre ações

### 3. Acompanhamento e Progresso
- Registro de check-ins periódicos de progresso
- Reflexões sobre dificuldades e evoluções
- Histórico visual de progresso ao longo do tempo

### 4. Dashboard e Relatórios
- Visão geral do status de objetivos
- Estatísticas de progresso
- Lista de próximas ações pendentes

## Requisitos Técnicos

### Stack Tecnológica
- **Frontend:** Angular 15+ com TypeScript, Bootstrap 5, RxJS
- **Backend:** .NET 8 (ASP.NET Core Web API), Entity Framework Core
- **Banco de Dados:** MySQL 8.0
- **Proxy/Web Server:** Nginx
- **Containerização:** Docker + Docker Compose
- **CI/CD:** GitHub Actions (planejado)

### Estrutura do Banco de Dados
- Tabela `Objetivos`: armazenamento de objetivos com campos como título, descrição, prazo, status
- Tabela `Acoes`: ações vinculadas a objetivos, com status de conclusão
- Relacionamentos: Um objetivo pode ter múltiplas ações

### Endpoints da API
- `GET /api/objetivos` - Listar objetivos
- `POST /api/objetivos` - Criar objetivo
- `PUT /api/objetivos/{id}` - Atualizar objetivo
- `DELETE /api/objetivos/{id}` - Excluir objetivo
- `GET /api/objetivos/{id}/acoes` - Listar ações de um objetivo
- `POST /api/objetivos/{id}/acoes` - Criar ação para objetivo
- `PATCH /api/acoes/{id}/status` - Atualizar status da ação
- `GET /api/dashboard/estatisticas` - Estatísticas do dashboard
- `GET /api/dashboard/proximas-acoes` - Próximas ações
- `GET /api/dashboard/progresso-objetivos` - Progresso dos objetivos

## Plano de Desenvolvimento

### Fase 1: Ambiente e Infraestrutura (Semana 1)
- Configuração Inicial do Docker
- Banco de Dados MySQL

### Fase 2: Backend .NET Core (Semanas 2-3)
- Projeto .NET Core Web API
- CRUD de Objetivos
- CRUD de Ações/Tarefas
- Endpoints de Dashboard

### Fase 3: Frontend Angular (Semanas 4-5)
- Setup Angular
- Módulo Objetivos
- Módulo Ações
- Dashboard Principal

### Fase 4: Integração e Polimento (Semana 6)
- Integração Frontend-Backend
- Melhorias de UX/UI

### Fase 5: Deploy e Documentação (Semana 7)
- Deploy Local/Produção
- Documentação

## Critérios de Aceitação
- Aplicação funcional em containers Docker
- CRUD completo de objetivos e ações
- Dashboard com estatísticas e progresso
- Interface responsiva e intuitiva
- Documentação completa do projeto

## Métricas de Sucesso
- Tempo de desenvolvimento: 7 semanas
- Funcionalidades implementadas conforme checklist
- Código limpo e bem documentado
- Ambiente de desenvolvimento replicável