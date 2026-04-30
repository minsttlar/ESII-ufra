# Resenha Aula 3 — Modelos UML e Design de Componentes

**Aluno:** Karla Geovanna Almeida de Sousa

**Data;** 29/04/2026

---

## Questão 1 — Modelos UML como ferramentas de modelagem

### (a) Estrutura × comportamento

O diagrama de classes representa a estrutura estática do sistema, mostrando classes, atributos e relacionamentos, mas não evidencia como ocorre a execução. Já o diagrama de sequência representa o comportamento dinâmico, destacando a troca de mensagens entre objetos ao longo do tempo.

Segundo Valente (Cap. 4, seção “Diagrama de Classes” e “Diagrama de Sequência”), esses modelos são complementares porque cada um omite aspectos que o outro evidencia. No sistema analisado, por exemplo, é possível identificar entidades como `Equipamento` e `Emprestimo`, mas não fica claro como ocorre o fluxo de execução apenas com a estrutura. Assim, usar apenas um dos diagramas leva a uma compreensão parcial do sistema.

### (b) Consequência prática

Na prática, o diagrama de classes orienta decisões estruturais, como definição de responsabilidades e organização das classes. Já o diagrama de sequência auxilia na definição do fluxo de execução, indicando quais métodos precisam existir e como os objetos interagem.

Por exemplo, o diagrama de sequência evidencia que antes de registrar um empréstimo é necessário verificar a disponibilidade do equipamento, o que influencia diretamente na criação de métodos e na separação de responsabilidades.

### (c) Aplicação ao UC01

No UC01, um diagrama de sequência mostraria a seguinte ordem: `Sistema.registrar()` chama `Equipamento.verificar_disponibilidade()`. Se o resultado for positivo, ocorre a chamada `Sistema.criar_emprestimo()`, seguida de `Emprestimo.definir_data_devolucao()`.

Essa sequência evidencia que o método `registrar` deve delegar responsabilidades aos objetos corretos. No código atual, essa lógica está acoplada a estruturas globais, mas o diagrama deixa claro que cada objeto deveria ter seu próprio comportamento, melhorando a organização do sistema.

---

## Questão 2 — Arquitetura, design e os princípios de decomposição

### (a) Definições

Coesão é o grau em que um módulo mantém foco em uma única responsabilidade. Acoplamento é o nível de dependência entre módulos. Ocultamento de informação é o princípio de esconder detalhes internos, expondo apenas o necessário por meio de interfaces (Valente, Cap. 7, seção “Coesão e Acoplamento”).

Esses conceitos são fundamentais para organizar sistemas de forma sustentável.

### (b) Relações entre os princípios

O ocultamento de informação reduz o acoplamento, pois impede que outros módulos dependam de detalhes internos. Além disso, módulos com alta coesão facilitam esse processo, pois possuem responsabilidades bem definidas.

Por outro lado, separar demais pode aumentar a quantidade de interações entre módulos, elevando o acoplamento. Assim, é necessário equilíbrio entre esses princípios.

### (c) Aplicação ao projeto v2.0

Na versão 2.0:

* **models/**: `Equipamento` e `Emprestimo` possuem alta coesão, pois concentram dados e comportamento, além de ocultar atributos como `disponivel` por meio de métodos.
* **services/**: `CalculadoraMulta` tem alta coesão (apenas cálculo) e baixo acoplamento, pois recebe dados por parâmetro.
* **repositories/**: encapsulam o acesso a dados, aplicando ocultamento de informação e reduzindo acoplamento com o restante do sistema.
* **main.py**: mantém baixa coesão intencional, apenas orquestrando chamadas.

Essa divisão reduz o acoplamento global observado no código atual e melhora a organização.

---

## Questão 3 — Crítica fundamentada à documentação do sistema legado

### (a) Pontos frágeis

Um problema evidente é o acoplamento por variável global: as listas `equipamentos` (linha 4) e `emprestimos_registrados` (linha 10) são acessadas diretamente pelos métodos, violando o ocultamento de informação (Valente, Cap. 7, seção “Ocultamento de Informação”).

Outro ponto é a baixa coesão e duplicação de lógica. O cálculo da multa aparece em `devolver` (linhas 56–64) e em `listar_atrasados` (linhas 74–77), utilizando estruturas `if/elif`. Além disso, há mistura de responsabilidades, como lógica de negócio e saída de dados (`print("[EMAIL]")` na linha 39), dificultando manutenção.

### (b) Ponto forte

A documentação reconhece explicitamente a dívida técnica. Por exemplo, o item **DT03** aponta que o cálculo de multa está acoplado ao tipo de equipamento. Isso demonstra que o problema já foi identificado e que há direção para melhoria, alinhando-se com boas práticas de design.

### (c) Síntese

Segundo Valente (Cap. 1, seção “Dívida Técnica”), quando problemas são conhecidos, trata-se de dívida deliberada. Nesse caso, a tabela DT01–DT07 mostra que o desenvolvedor tinha consciência das limitações.

Essa transparência demonstra maturidade e facilita a evolução para a versão 2.0, permitindo transformar cada débito em uma melhoria concreta, como separar o cálculo de multa em uma classe específica e eliminar variáveis globais.

---

## Questão 4 — Tipos como contratos: dicionários × classes

### (a) Prevenção de erros

No código atual, o uso de dicionários (`equipamento["disponivel"]`) pode gerar erros como digitação incorreta da chave. Por exemplo, usar `"disponível"` (com acento) gera um `KeyError` em tempo de execução.

Além disso, não há garantia de que a chave exista ou que o tipo esteja correto. Com uma classe `Equipamento`, esses problemas seriam reduzidos, pois os atributos seriam definidos explicitamente (Valente, Cap. 5, seção “Sistema de Tipos”).

### (b) Capacidade de evolução

Classes permitem adicionar novos comportamentos sem afetar quem as utiliza. Por exemplo, um método `calcular_multa()` pode ser incluído sem alterar o restante do sistema.

Já com dicionários, mudanças na estrutura podem quebrar partes do código que dependem diretamente das chaves, dificultando evolução.

### (c) Comunicação do design

Valente (Cap. 4, seção “Classes e Objetos”) argumenta que tipos nomeados comunicam o modelo do sistema. Uma classe `Equipamento` deixa claro o significado dos dados, enquanto um `dict` é genérico.

Essa clareza não é apenas estética, mas parte do projeto, pois melhora a compreensão e reduz ambiguidades. 
