# ADR-001 — Arquitetura em Camadas

## Contexto
O sistema precisa atender aos requisitos RNF03 (manutenibilidade) e RNF04 (testabilidade),
permitindo adicionar novos tipos de equipamentos sem alterar múltiplos módulos e testar regras isoladamente.

## Opções consideradas
- Arquivo único: baixa organização, difícil manutenção e testes.
- MVC: boa separação, porém complexidade desnecessária para CLI.
- Em camadas: equilíbrio entre organização, simplicidade e testabilidade.

## Decisão
Será adotada arquitetura em camadas:

- core: regras de negócio (cálculo de multa, validações)
- services: fluxo da aplicação (registrar, devolver, listar)
- infra: persistência (futuro)
- interface: CLI (menu e interação)

## Consequências
Melhora organização, facilita testes e evolução.
Aumenta um pouco a complexidade inicial.
