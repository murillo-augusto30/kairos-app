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

## ADR 006: CI/CD com GitHub Actions

### Contexto
Automação de build, teste e deploy para qualidade consistente.

### Decisão
Implementar pipelines com GitHub Actions.

### Razões
- Integração nativa com GitHub
- Suporte a Docker builds
- Fácil configuração via YAML
- Gratuito para repositórios públicos

### Consequências
- Dependência de GitHub
- Configuração inicial necessária
- Benefícios: automação e feedback rápido

## ADR 007: Documentação Estruturada

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