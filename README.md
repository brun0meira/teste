## Resumo Detalhado do Artigo "Passo a passo: Criar e executar testes de unidade para código gerenciado"

### Tecnologia Utilizada
- **Linguagem**: C#
- **Ferramentas**: Visual Studio, Gerenciador de Testes do Visual Studio
- **Frameworks**: MSTest

### Conceitos Aprendidos
1. **Testes de Unidade**: Testes que verificam o funcionamento correto de partes específicas do código, isoladas de outras partes do sistema.
2. **Asserts**: Declarações usadas em testes de unidade para verificar se um resultado esperado é igual ao resultado real.
3. **MSTest**: Framework de testes de unidade para .NET que permite escrever e executar testes de forma integrada com o Visual Studio.

### Passos do Artigo

1. **Criar um projeto para teste**
   - Iniciar um projeto C# no Visual Studio.
   - Definir uma classe `BankAccount` com métodos `Debit` e `Credit`.

2. **Criar um projeto de teste de unidade**
   - Criar um projeto de teste de unidade MSTest para testar a classe `BankAccount`.

3. **Criar a classe de teste**
   - Criar a classe `BankAccountTests` com métodos de teste para os métodos `Debit` e `Credit`.

4. **Criar o primeiro método de teste**
   - Verificar se o método `Debit` subtrai corretamente o valor do débito do saldo da conta.
   - Criar o método `Debit_WithValidAmount_UpdatesBalance` na classe `BankAccountTests`.

![Screenshot from 2024-04-10 14-56-49](https://github.com/brun0meira/teste/assets/99202553/88c8f9f9-4985-4848-a9a6-7add9ac0a2a5)

O procedimento é direto: primeiramente, ele instancia um novo objeto BankAccount com um saldo inicial e, em seguida, realiza uma retirada de um valor válido. Em seguida, ele utiliza o método Assert.AreEqual para confirmar se o saldo final corresponde ao esperado. Métodos como Assert.AreEqual, Assert.IsTrue e similares são comumente empregados em testes de unidade.

![Screenshot from 2024-04-10 14-53-22](https://github.com/brun0meira/teste/assets/99202553/67ac6ffa-355a-476d-9bd9-1ba999a566d5)

Nesse caso, o teste falhou e para o teste passar é necessario fazer mudanças no código. O código será alterado pois o teste de unidade revelou um bug que quando o app roda o valor do saque é adicionado ao saldo da conta quando deveria ser subtraído.

Antes da mudança:

```
  m_balance += amount;
```

Depois da mudança:

```
  m_balance -= amount;
```

Agora podemos rodar o teste novamente:

![Screenshot from 2024-04-10 15-09-05](https://github.com/brun0meira/teste/assets/99202553/4854203a-311c-4d69-a512-cb3361dbf107)

   - Definir um saldo inicial, um valor de débito e um saldo esperado.
   - Criar uma instância de `BankAccount` e chamar o método `Debit`.
   - Usar `Assert.AreEqual` para verificar se o saldo final é o esperado.

5. **Continuar a análise**
   - Criar métodos de teste para verificar outros comportamentos do método `Debit`.
   
  ![Screenshot from 2024-04-10 15-11-47](https://github.com/brun0meira/teste/assets/99202553/973c8684-eeb1-4391-8294-1ffcde93088c)

   - Verificar se o método lança uma exceção quando o valor do débito é maior que o saldo ou menor que zero.

Teste rodando com sucesso:

![Screenshot from 2024-04-10 15-13-04](https://github.com/brun0meira/teste/assets/99202553/e22650fc-cf5b-43cc-a07c-837dda3fe063)

5. **Refatorar o código em teste**
   - Refatorar o método `Debit` para lançar exceções com mensagens específicas quando necessário.

Antes da refatoração do app:

```
        if (amount > m_balance)
            {
                throw new ArgumentOutOfRangeException("amount");
            }

            if (amount < 0)
            {
                throw new ArgumentOutOfRangeException("amount");
            }
```

Depois da refatoração do app:

```
        public const string DebitAmountExceedsBalanceMessage = "Debit amount exceeds balance";
        public const string DebitAmountLessThanZeroMessage = "Debit amount is less than zero";

        if (amount > m_balance)
            {
                throw new System.ArgumentOutOfRangeException("amount", amount, DebitAmountExceedsBalanceMessage);
            }

            if (amount < 0)
            {
                throw new System.ArgumentOutOfRangeException("amount", amount, DebitAmountLessThanZeroMessage);
            }
```

Refatoração do código:

```
        [TestMethod]
        public void Debit_WhenAmountIsMoreThanBalance_ShouldThrowArgumentOutOfRange()
        {
            // Arrange
            double beginningBalance = 11.99;
            double debitAmount = 20.0;
            BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);
        
            // Act
            try
            {
                account.Debit(debitAmount);
            }
            catch (System.ArgumentOutOfRangeException e)
            {
                // Assert
                StringAssert.Contains(e.Message, BankAccount.DebitAmountExceedsBalanceMessage);
                return;
            }
        
            Assert.Fail("The expected exception was not thrown.");
        }
```

6. **Testar, gravar e analisar novamente**
   - Ajustar os métodos de teste para lidar corretamente com exceções lançadas pelo método `Debit`.
   - Verificar se os testes falham quando esperado e se passam após a correção do código.
  
Evidência todos testes rodando:

![Screenshot from 2024-04-10 15-24-28](https://github.com/brun0meira/teste/assets/99202553/1b492ce5-6aea-4e3d-a0a1-d37c604f5197)

### Conclusão
A implementação de testes de unidade para o método `Debit` da classe `BankAccount` resultou em métodos de teste mais robustos e informativos, além de melhorar o código em teste.

Este resumo detalha os passos do artigo, fornecendo uma visão mais específica do processo de criação e execução de testes de unidade para código gerenciado.
