# Decomposição em camadas

## models/equipamento.py
Responsável por representar os equipamentos do sistema usando uma dataclass.

## models/emprestimo.py
Responsável por representar os empréstimos realizados no sistema.

## services/servico_emprestimo.py
Contém as regras de negócio relacionadas aos empréstimos.

## services/notificador.py
Responsável pelo envio de notificações aos usuários.

## repositories/repositorio_emprestimo.py
Responsável pelo armazenamento e recuperação de dados.

## main.py
Responsável pela interação com o usuário e fluxo principal do sistema.

---

# Diagramas de sequência

## UC01 — Registrar Empréstimo

```mermaid
sequenceDiagram
    actor Atendente
    participant main as main.py
    participant servico as ServicoEmprestimo
    participant repo as RepositorioEmprestimo
    participant notif as Notificador

    Atendente->>main: informa equip_id, nome, email, dias
    main->>servico: registrar(equip_id, nome, email, dias)
    servico->>repo: buscar_equipamento(equip_id)
    repo-->>servico: Equipamento

    alt equipamento disponível
        servico->>repo: salvar_emprestimo(emprestimo)
        servico->>repo: marcar_indisponivel(equip_id)
        servico->>notif: notificar_emprestimo(email, data_devolucao)
        servico-->>main: True
    else equipamento indisponível
        servico-->>main: False
    end
```

## UC02 — Registrar Devolução

```mermaid
sequenceDiagram
    actor Atendente
    participant main as main.py
    participant servico as ServicoEmprestimo
    participant repo as RepositorioEmprestimo

    Atendente->>main: informa id_emprestimo
    main->>servico: registrar_devolucao(id_emprestimo)
    servico->>repo: buscar_emprestimo(id_emprestimo)
    repo-->>servico: Emprestimo

    alt empréstimo encontrado
        servico->>repo: marcar_disponivel(equip_id)
        servico->>repo: finalizar_emprestimo(id_emprestimo)
        servico-->>main: True
    else empréstimo não encontrado
        servico-->>main: False
    end
```

## UC03 — Listar Empréstimos em Atraso

```mermaid
sequenceDiagram
    actor Atendente
    participant main as main.py
    participant servico as ServicoEmprestimo
    participant repo as RepositorioEmprestimo

    Atendente->>main: solicitar atrasados
    main->>servico: listar_atrasados()
    servico->>repo: buscar_emprestimos()
    repo-->>servico: lista_emprestimos

    loop para cada empréstimo
        servico->>servico: verificar_atraso()
    end

    servico-->>main: lista_atrasados
```
