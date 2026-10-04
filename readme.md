# swauth-ui

Implementação de referência que integra o módulo `sw_auth` numa aplicação real,
pensada para servir de base a produtos SaaS futuros.

## Stack

- PHP >= 8.1
- [Slim PHP 4](https://www.slimframework.com/) — routing e middleware, sem autenticação própria
- PHP-DI — container PSR-11
- vlucas/phpdotenv — configuração via `.env`
- `seeware/sw-auth` — módulo de autenticação (dependência local via path repository)

## Arquitetura

Monólito modular, dividido em `Core` e `Modules`:

- **Core** — infraestrutura partilhada: ponte com a `sw_auth`, middleware de
  autenticação e de contexto de conta, kernel de carregamento de módulos,
  ligação à base de dados.
- **Modules** — funcionalidades, cada uma isolada e seguindo o mesmo padrão
  interno (Controller → Service → Repository):

Modules/<Nome>/
Controllers/
Services/
Repositories/
Entities/
Views/
migrations/
routes.php

### Multi-tenancy

A `sw_auth` conhece apenas utilizadores — não tem conceito de conta/equipa.
Esse conceito vive na aplicação: toda a informação de negócio pertence a uma
`account` (não a um `user` diretamente), através da tabela `account_users`
(utilizador ↔ conta ↔ papel: `owner`/`member`). Isto permite que uma conta
tenha vários utilizadores, mesmo quando usada por uma pessoa só.