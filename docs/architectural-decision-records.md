# Architectural Decision Records (ADR) - Kairos PDI App

## ADR 001: Escolha da Stack Tecnológica Principal

### Contexto
O projeto Kairos necessita de uma arquitetura robusta, escalável e moderna para suportar uma aplicação web de Planejamento de Desenvolvimento Individual. Considerações incluem produtividade do desenvolvedor, performance, manutenção e curva de aprendizado.

### Decisão
Adotaremos a stack: Angular 15+ para frontend, .NET 8 (ASP.NET Core) para backend e MySQL 8.0 para banco de dados, tudo containerizado com Docker.

### Razões
- **Angular:** Framework maduro com TypeScript, forte tipagem, componentes reutilizáveis e grande ecossistema. Melhor para aplicações complexas de longa duração.
- **.NET 8:** Plataforma enterprise com alto desempenho, segurança robusta e ferramentas avançadas (EF Core). Suporte LTS e compatibilidade com Windows/Linux.
- **MySQL:** Banco relacional confiável, amplamente usado, com boa performance para dados estruturados. Fácil integração com .NET via EF Core.
- **Docker:** Padronização de ambientes, facilidade de deploy e desenvolvimento colaborativo.

### Consequências
- Curva de aprendizado inicial para desenvolvedores não familiarizados com .NET/Angular
- Dependência de containers Docker para desenvolvimento
- Benefícios: produtividade a longo prazo, escalabilidade e manutenção facilitada

## ADR 002: Arquitetura em Camadas (Backend)

### Contexto
O backend precisa ser organizado para facilitar manutenção, testes e evolução. Separação de responsabilidades é crucial.

### Decisão
Implementar arquitetura em camadas: Domain, Application, Infrastructure e API.

### Razões
- **Domain:** Contém regras de negócio, entidades e interfaces de repositório
- **Application:** Serviços de aplicação, DTOs e mapeamentos
- **Infrastructure:** Implementações concretas (EF Core, repositórios)
- **API:** Controllers e configuração da Web API

### Consequências
- Código mais organizado e testável
- Facilita mudanças futuras na infraestrutura
- Aumento inicial no número de arquivos/projetos

## ADR 003: Containerização com Docker Compose

### Contexto
Necessidade de ambiente de desenvolvimento consistente e deploy simplificado.

### Decisão
Usar Docker Compose com serviços separados para frontend, backend, banco de dados e nginx.

### Razões
- Isolamento de serviços
- Facilidade para desenvolvimento local
- Mesmas configurações para dev/prod
- Nginx como proxy reverso para roteamento

### Consequências
- Dependência de Docker instalado
- Overhead de recursos (CPU/memória)
- Benefícios: deploy consistente e escalável

## ADR 004: Persistência de Dados com Entity Framework Core

### Contexto
Mapeamento objeto-relacional eficiente e produtivo para .NET.

### Decisão
Usar EF Core com MySQL para todas as operações de banco de dados.

### Razões
- Produtividade: migrations automáticas, LINQ queries
- Type safety e intellisense
- Suporte nativo a MySQL
- Padrão da comunidade .NET

### Consequências
- Abstração sobre SQL puro
- Possível perda de performance em queries complexas (otimizável)
- Facilita mudanças de banco futuro

## ADR 005: Frontend com Angular e Bootstrap

### Contexto
Interface responsiva e moderna para gestão de PDI.

### Decisão
Angular 15+ com Bootstrap 5 para componentes UI.

### Razões
- Component-based architecture adequada para app complexa
- Bootstrap: responsividade rápida e consistente
- RxJS para reactive programming
- TypeScript para type safety

### Consequências
- Bundle size maior comparado a frameworks menores
- Curva de aprendizado para Angular
- Benefícios: manutenção e escalabilidade

## ADR 006: Documentação Estruturada

### Contexto
Projeto complexo necessita documentação clara e acessível.

### Decisão
Separar documentação em PRD (requisitos) e ADR (decisões arquiteturais), mantendo README minimalista.

### Razões
- PRD: foco em funcionalidades e requisitos do produto
- ADR: registro de decisões técnicas para futuro
- README: apenas essentials (instalação, uso básico)

### Consequências
- Documentação distribuída em múltiplos arquivos
- Necessidade de manter consistência
- Benefícios: clareza e organização

## ADR 007: Estrutura do Projeto

### Contexto
Organização clara e padronizada do código fonte para facilitar desenvolvimento, manutenção e escalabilidade do projeto Kairos.

### Decisão
Adotar a estrutura de pastas descrita abaixo, seguindo convenções de projetos Angular e .NET, com separação clara de responsabilidades.

### Estrutura do Projeto

```text
kairos-app/
├── 📁 src/                          # Código fonte principal
│   ├── 📁 frontend/                 # Aplicação Angular
│   │   ├── 📁 src/app/
│   │   │   ├── 📁 core/             # Serviços essenciais, interceptors
│   │   │   ├── 📁 modules/          # Módulos funcionais (PDI, objetivos, etc.)
│   │   │   ├── 📁 shared/           # Componentes/diretivas/pipes reutilizáveis
│   │   │   └── 📁 layouts/          # Layouts da aplicação
│   │   ├── angular.json
│   │   ├── package.json
│   │   └── Dockerfile
│   │
│   └── 📁 backend/                  # API .NET
│       ├── 📁 src/
│       │   ├── 📁 Kairos.API/         # Controllers, Filters, Middlewares
│       │   ├── 📁 Kairos.Application/ # Application Services, DTOs, Mappers
│       │   ├── 📁 Kairos.Domain/      # Entities, Value Objects, Domain Services, Repository Interfaces
│       │   └── 📁 Kairos.Infrastructure/ # EF Core, Repositories, Email, Storage
│       ├── Kairos.sln
│       └── Dockerfile
│
├── 📁 docker/                       # Configurações Docker
│   ├── docker-compose.yml           # Compose principal
│   ├── docker-compose.dev.yml       # Configurações desenvolvimento
│   ├── docker-compose.prod.yml      # Configurações produção
│   ├── 📁 mysql/                    # Configurações MySQL
│   │   ├── init.sql                 # Script inicialização BD
│   │   └── my.cnf                   # Configurações personalizadas
│   └── 📁 nginx/                    # Configurações Nginx
│       ├── nginx.conf               # Config principal
│       ├── default.conf             # Config virtual host
│       └── ssl/                     # Certificados SSL (produção)
│
├── 📁 docs/                         # Documentação
│   ├── 📁 architecture/             # Diagramas de arquitetura
│   ├── 📁 api/                      # Documentação API (Swagger/OpenAPI)
│   └── 📁 database/                 # Modelos ER, migrations
│
├── 📁 scripts/                      # Scripts utilitários
│   ├── init-db.sh                   # Inicialização banco de dados
│   ├── seed-data.sh                 # Dados iniciais
│   └── deploy-local.sh              # Deploy local
│
├── 📁 .github/                      # Configurações GitHub
│   └── 📁 workflows/                # CI/CD pipelines
│
├── .env.example                     # Variáveis de ambiente exemplo
├── .gitignore
├── docker-compose.yml               # Link simbólico para docker/docker-compose.yml
├── Makefile                         # Comandos úteis
└── README.md                        # Este arquivo
```

### Razões
- **Separação clara:** Frontend e backend isolados em pastas distintas
- **Convenções estabelecidas:** Segue padrões do Angular (core/shared/modules) e .NET (camadas Domain/Application/Infrastructure)
- **Containerização:** Configurações Docker organizadas por serviço
- **Documentação estruturada:** Pastas específicas para diferentes tipos de docs
- **Scripts utilitários:** Centralização de automações em uma pasta dedicada

### Consequências
- Facilita navegação e localização de arquivos
- Padronização para novos desenvolvedores
- Suporte à escalabilidade do projeto
- Manutenção mais eficiente do código