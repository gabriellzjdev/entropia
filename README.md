# Entropia

Gerador de senhas criptograficamente seguro, 100% backend — projeto de estudo.

O objetivo é gerar senhas com **≥ 128 bits de entropia** a partir de uma fonte criptográfica (`SecureRandom`), sem viés na escolha dos caracteres, e **sem nunca persistir ou registrar a senha gerada**.

## Stack

| Camada | Tecnologia |
|---|---|
| Linguagem | Java 21 |
| Framework | Spring Boot 3.5 |
| Arquitetura | Monólito modular (Spring Modulith) com arquitetura hexagonal por módulo |
| Banco de dados | PostgreSQL + Flyway |
| Identidade | Keycloak (OAuth2 / OpenID Connect) |
| Testes | JUnit 5, AssertJ, Testcontainers |
| Infraestrutura | Docker, Docker Compose, GitHub Actions |
| AWS (simulada) | LocalStack + Terraform |

## Módulos

```
io.github.gabriellzjdev.entropia
├── generator   geração de senhas e passphrases, cálculo de entropia
├── policy      regras de geração (tamanho, conjuntos de caracteres)
└── audit       registro de eventos de geração (sem a senha)
```

Cada módulo separa `domain` (regras puras, sem framework), `application` (casos de uso) e `adapters` (web, banco, eventos).

## Status

🚧 Em desenvolvimento — núcleo de geração (módulo `generator`).

## Como executar

Requisitos: Java 21.

```bash
./mvnw spring-boot:run   # sobe a aplicação
./mvnw test              # executa os testes
```

## Princípios de segurança

- Aleatoriedade exclusivamente via `SecureRandom`, sem viés de módulo.
- Senhas geradas não são gravadas em banco, log, cache ou evento.
- Respostas com `Cache-Control: no-store`.
- Autenticação delegada ao Keycloak — sem implementação própria de login.
