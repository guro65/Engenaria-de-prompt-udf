# Ajuda com Códigos

### Primeira interação

#### Promt: Crie um exercício de C# para praticar
#### Resposta:
Com certeza! Vamos deixar o quiz de lado e focar na **construção passo a passo** do código. Praticar C# através de um sistema bancário é excelente porque você aplica os quatro pilares da Programação Orientada a Objetos (POO) de uma só vez.

Abaixo, apresento um guia detalhado para você implementar.

---

## O Desafio: Sistema de Gestão de Conta Bancária

O seu objetivo é criar uma classe chamada `ContaBancaria` que gerencie o dinheiro de um cliente, garantindo que o saldo nunca seja manipulado de forma indevida.

### 1. Estrutura da Classe (Encapsulamento)
Em C#, não queremos que qualquer parte do programa altere o saldo diretamente (ex: `conta.saldo = 1000000;`). Usamos **propriedades** para controlar o acesso.

* **Campos Privados:** Onde os dados moram.
* **Propriedades Públicas:** A "porta de entrada" controlada.

```csharp
public class ContaBancaria
{
    // O titular pode ser lido por todos, mas só definido na criação
    public string Titular { get; }

    // O saldo pode ser lido por todos, mas apenas a classe altera (private set)
    public decimal Saldo { get; private set; }

    // Construtor: Garante que a conta comece com um titular e, opcionalmente, um saldo
    public ContaBancaria(string nomeTitular, decimal depositoInicial = 0)
    {
        Titular = nomeTitular;
        Saldo = depositoInicial;
    }
}
```

### 2. Comportamentos (Métodos)
Agora, precisamos de lógica para as operações. Um método deve validar a entrada antes de agir.

* **Depositar:** O valor deve ser positivo.
* **Sacar:** O valor deve ser positivo e o cliente deve ter saldo suficiente.

```csharp
public void Depositar(decimal valor)
{
    if (valor > 0)
    {
        Saldo += valor;
        Console.WriteLine($"Depósito de {valor:C} realizado com sucesso!");
    }
}

public bool Sacar(decimal valor)
{
    if (valor > 0 && valor <= Saldo)
    {
        Saldo -= valor;
        return true; // Sucesso
    }
    return false; // Falha (saldo insuficiente ou valor inválido)
}
```

### 3. Representação (Override)
Para facilitar a visualização, podemos sobrescrever o método `ToString()`. Assim, ao dar um `Console.WriteLine(minhaConta)`, o C# já sabe o que exibir.

```csharp
public override string ToString()
{
    return $"Titular: {Titular} | Saldo Atual: {Saldo:C}";
}
```

---

## Exercício Prático para Você Fazer

Tente montar o programa completo no seu VS Code ou Visual Studio seguindo este roteiro:

1.  **Crie a classe `ContaBancaria`** com as propriedades `Titular` e `Saldo`.
2.  **Adicione um Construtor** que receba o nome do titular.
3.  **Implemente o método `Depositar`**.
4.  **Implemente o método `Sacar`** (Dica: tente adicionar uma pequena taxa de saque, como R$ 5,00, para praticar a lógica).
5.  **No método `Main`**, crie um objeto, faça um depósito, um saque e exiba o saldo final na tela.

### Por que usar `decimal` em vez de `double`?
Para exercícios de matemática simples, usamos `double` ou `float`. Mas para **dinheiro**, sempre usamos `decimal`. O `double` trabalha com base binária e pode gerar erros de arredondamento bizarros (como $0.1 + 0.2 = 0.30000000000000004$). O `decimal` é mais lento, porém é exato para finanças.

**Dica de Ouro:** Tente usar a interpolação de strings com formatação de moeda: `$"Saldo: {Saldo:C}"`. O `:C` coloca automaticamente o símbolo da moeda do seu sistema (R$, $, etc).

O que você acha de tentar escrever esse código agora? Se travar em alguma parte da sintaxe, é só me chamar!
#### Feedback:
Ficou bem explicado e completo principalmente pelo fato de eu já ter utilizado o Gemini para me ajudar com problemas de scripts em c#.

### Segunda interação

#### Prompt: Como eu trabalho com C# há cerca de 3 anos, tenho um bom domínio das referências da linguagem e consigo explicar de forma clara, graças à minha formação na área de tecnologia e à minha experiência com ensino. Com base nisso, crie uma atividade para praticar o desenvolvimento de código em C#.

####Resposta:
Com base na sua experiência de 3 anos e no seu perfil como educador, o ideal não é um exercício de sintaxe básica, mas sim um desafio de **Refatoração e Design de Código**. 

Como você já domina as referências da linguagem, o foco aqui é aplicar princípios como **SOLID**, **DRY (Don't Repeat Yourself)** e o uso de recursos modernos do C# para tornar o código mais testável e elegante.

---

## O Desafio: De "Código Esparguete" a "Arquitetura Limpa"

Abaixo, apresento um código funcional, mas mal estruturado. Ele simula um sistema de processamento de pedidos. Seu objetivo é refatorá-lo.

### O Código "Sujo" (Legacy)

```csharp
public class OrderService
{
    public void ProcessOrder(string customerName, string item, int quantity, double price, string type)
    {
        if (string.IsNullOrEmpty(customerName)) throw new Exception("Nome inválido");
        
        double total = quantity * price;

        if (type == "VIP")
        {
            total = total * 0.9; // 10% desconto
        }

        Console.WriteLine($"Processando pedido de {item} para {customerName}");
        
        // Simula salvar no banco
        Console.WriteLine($"Salvando no banco: Pedido de {total} reais.");

        // Simula envio de e-mail
        Console.WriteLine($"Enviando e-mail para {customerName}: Seu pedido de {item} foi aprovado.");
    }
}
```

---

## Atividade: Instruções de Refatoração

Para praticar como um desenvolvedor pleno/sênior, aplique as seguintes melhorias:

### 1. Tipagem e Domínio
* Substitua o `double` por `decimal` para cálculos financeiros.
* Crie uma `struct` ou `record` para representar o `Pedido`, em vez de passar 5 parâmetros soltos no método (Primitive Obsession).
* Use um `enum` para o tipo de cliente (VIP, Regular, etc).

### 2. Separação de Responsabilidades (SRP)
O método `ProcessOrder` faz tudo: valida, calcula, salva e envia e-mail. Separe isso:
* Crie uma interface `IOrderRepository` para a persistência.
* Crie uma interface `INotificationService` para o envio de mensagens.

### 3. Injeção de Dependência (DI)
* Refatore a classe `OrderService` para receber essas interfaces via **construtor**. Isso permitirá que você escreva testes unitários usando *Mocks* futuramente.

### 4. Lógica de Desconto (Open/Closed Principle)
* Em vez de um `if (type == "VIP")`, pense em como você implementaria uma estratégia de descontos onde novos tipos de desconto pudessem ser adicionados sem alterar a classe de serviço principal.

### 5. C# Moderno
* Utilize **File-scoped namespaces**.
* Utilize **Target-typed new expressions** (`List<string> list = new();`).
* Utilize **Interpolated strings** e **Pattern Matching** se necessário.

---

## O que este exercício exercita?

Ao finalizar essa refatoração, você terá praticado:
1.  **Testabilidade:** Código desacoplado é fácil de testar.
2.  **Manutenibilidade:** Se o banco de dados mudar de SQL para NoSQL, ou o e-mail mudar para SMS, o núcleo do seu negócio (OrderService) permanece intacto.
3.  **Expressividade:** O código deve ler-se como prosa, algo que sua formação em ensino certamente valoriza.

**Dica de Especialista:** Tente implementar o padrão **Strategy** para os descontos. É um excelente exercício de abstração para quem já tem 3 anos de estrada.

Como você estruturaria a interface de descontos para evitar que o `OrderService` cresça indefinidamente a cada nova promoção?

#### Feedbakc:
No geral, a atividade ficou bem estruturada e com um nível adequado, abordando práticas importantes como SOLID e DI, mas pode melhorar ao tornar a linguagem mais natural, reduzir explicações desnecessárias e dar mais autonomia para quem for resolver o desafio.

### Terceira interação

#### Prompt: `Como eu trabalho com C# há cerca de 3 anos, tenho um bom domínio da linguagem e consigo explicar de forma clara por conta da minha formação na área de tecnologia e da minha experiência ensinando. Com base nisso, crie uma atividade para um aluno iniciante em programação com C#, focada em fundamentos e conceitos mais simples de entender e evoluir. Organize bem a resposta em formato Markdown, e a atividade não deve conter códigos, pois a ideia é que o aluno desenvolva a solução a partir da interpretação do problema.`
#### Resposta:
Excelente! Dada a sua experiência como desenvolvedor e educador, você sabe que o maior desafio para um iniciante não é decorar a sintaxe, mas sim desenvolver o **raciocínio lógico** e a capacidade de **decompor problemas**.

A atividade abaixo foi desenhada para ser um "divisor de águas" no aprendizado de um iniciante: ela utiliza conceitos básicos (variáveis, condicionais e loops), mas exige que o aluno pense na validação de dados e na experiência do usuário.

---

# Atividade de Programação: Sistema de Controle de Fluxo de Caixa

### Contexto
Uma pequena loja de conveniência de bairro trabalha apenas com vendas à vista e precisa de um programa de console simples para registrar as movimentações do dia. O dono da loja não entende de tecnologia e precisa de algo muito intuitivo.

### O Desafio
Você deve desenvolver um programa em **C#** que funcione como um terminal de caixa. O sistema deve permitir que o usuário registre entradas (vendas) e saídas (pagamento de fornecedores ou despesas) e, ao final, apresente o balanço do período.

---

### Requisitos Funcionais

1.  **Abertura de Caixa:** O programa deve começar solicitando ao usuário o valor que já existe na gaveta (Saldo Inicial).
2.  **Menu de Operações:** O sistema deve exibir um menu contínuo com as seguintes opções:
    * `1` - Registrar Entrada (Venda).
    * `2` - Registrar Saída (Gasto/Pagamento).
    * `3` - Finalizar Dia e Gerar Relatório.
3.  **Lógica de Lançamento:**
    * Ao registrar uma **Entrada**, o valor deve ser somado ao saldo atual.
    * Ao registrar uma **Saída**, o valor deve ser subtraído do saldo atual.
4.  **Encerramento:** Ao escolher a opção de finalizar, o programa deve parar de solicitar novos dados e exibir o resumo final.

---

### Regras de Negócio (Onde o aluno exercita o raciocínio)

* **Validação de Valores:** O sistema não pode aceitar valores negativos ou iguais a zero para lançamentos de entrada ou saída.
* **Aviso de Saldo Insuficiente:** Se o usuário tentar registrar uma **Saída** maior do que o saldo disponível em caixa, o programa deve exibir um alerta informando que a operação é arriscada (mas deve permitir, simulando um "fiado" ou saldo negativo).
* **Persistência da Sessão:** O menu deve reaparecer após cada operação até que a opção de finalizar seja escolhida.

---

### O Relatório Final
Ao encerrar o programa, os seguintes dados devem ser apresentados de forma organizada:
1.  O valor com o qual o caixa abriu.
2.  O total acumulado apenas de **Entradas**.
3.  O total acumulado apenas de **Saídas**.
4.  O saldo final (Resultado de: `Inicial + Entradas - Saídas`).
5.  Uma mensagem de status:
    * Se o saldo final for maior que o inicial: **"Lucro no período"**.
    * Se o saldo final for menor que o inicial: **"Prejuízo no período"**.
    * Se forem iguais: **"Equilíbrio no período"**.

---

### Sugestão de Evolução (Para alunos mais rápidos)
Se o aluno terminar cedo, peça para ele implementar um **Contador de Operações**, mostrando no relatório final quantas vendas foram realizadas e quantos pagamentos foram feitos.

---

## Guia para o Mentor (Você)
Como você tem experiência no ensino, aqui estão os pontos que você pode observar na solução do aluno:

* **Uso de Tipos:** Ele usou `float`, `double` ou `decimal`? (Ideal para reforçar por que `decimal` é melhor para dinheiro).
* **Estrutura de Repetição:** Ele usou `while` ou `do-while`? O `do-while` costuma ser mais semântico para menus.
* **Tratamento de Erros:** O programa quebra se o usuário digitar uma letra em vez de um número? (Ótimo gancho para explicar `int.TryParse` ou `try-catch` em uma aula futura).

#### Feedback:
No geral, a explicação ficou muito bem estruturada e didática, principalmente porque foca no raciocínio lógico e na interpretação do problema em vez de apenas na sintaxe. A atividade está clara e bem contextualizada, o que ajuda bastante um iniciante. No entanto, a explicação pode melhorar um pouco ao simplificar alguns trechos e deixar a linguagem mais direta, para não ficar excessivamente guiada. Isso ajudaria a tornar a explicação mais acessível e fácil de entender para todos os usuários, principalmente os iniciantes. A explicação do problema é um ponto forte, pois permite que os usuários entendam melhor o que está sendo pedido e como resolver o problema de forma lógica.  
