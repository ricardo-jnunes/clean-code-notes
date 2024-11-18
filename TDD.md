# TDD (Test Driven Development)

O TDD (Test Driven Development) foi desenvolvido na metodologia XP (Extreme Programming) por Kent Beck, um engenheiro de software signatário do Manifesto Ágil. 

# Ciclos
O TDD é a prática repetida de pequenos ciclos divididos em três fases: Red (failing test), Green (passing test), Blue (Refactoring).

- :red_square: Fase Red: escrever um teste que falhe.
- :green_square: Fase Green: escrever o mínimo possível para o teste passar.
- :blue_square: Fase Blue: refatorar o código escrito.

Para garantir que o fluxo está correto existem 3 leis para nos auxiliar. São as 3 leis do TDD. Essas leis nos forçam a escrever código escalável e desacoplado.

- 1ª Lei: Você não deve escreve nenhum código em produção sem antes ter um spec falhando.
  ```java
  import static org.junit.jupiter.api.Assertions.assertEquals;
  import org.junit.jupiter.api.Test;
  
  public class CalculatorTest {
      @Test
      public void testSum() {
          Calculator calculator = new Calculator();
          int result = calculator.sum(2, 3);
          assertEquals(5, result);
      }
  }

- 2ª Lei: Você não pode escrever mais do que um teste de unidade para fazer seu spec falhar. Não compilar é falhar.
  ``` java
  public class Calculator {
    public int sum(int a, int b) {
        return 0; // implementação provisória para compilar
      }
  }

- 3ª Lei: Você não pode escrever mais do que o necessário para fazer o seu teste passar.
  ```java
  public class Calculator {
    public int sum(int a, int b) {
        return a + b; // implementação mínima para passar o teste
      }
  }


Agora que o código necessário foi escrito e os testes estão passando é o momento para reservar um tempo para refatorar o código, eliminando más práticas e simplificando-o no que for possível, garantindo que esteja manutenível para os que forem mexer no código depois de você.

# Exemplo

### Classe Calculator

```java
package com.example.calculator;

/**
 * Calculator class for basic arithmetic operations.
 * Demonstrates clean code practices with clear method names, comments, and exception handling.
 */
public class Calculator {

    /**
     * Adds two numbers.
     *
     * @param a the first number
     * @param b the second number
     * @return the sum of the two numbers
     */
    public double add(double a, double b) {
        return a + b;
    }

    /**
     * Multiplies two numbers.
     *
     * @param a the first number
     * @param b the second number
     * @return the product of the two numbers
     */
    public double multiply(double a, double b) {
        return a * b;
    }

    /**
     * Divides one number by another.
     *
     * @param numerator   the numerator
     * @param denominator the denominator
     * @return the result of the division
     * @throws IllegalArgumentException if the denominator is zero
     */
    public double divide(double numerator, double denominator) {
        if (denominator == 0) {
            throw new IllegalArgumentException("Denominator cannot be zero.");
        }
        return numerator / denominator;
    }

    /**
     * Subtracts one number from another.
     *
     * @param a the first number
     * @param b the second number
     * @return the difference between the two numbers
     */
    public double subtract(double a, double b) {
        return a - b;
    }
}

```

### Testes Unitários com JUnit

```java
package com.example.calculator;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

/**
 * Unit tests for the Calculator class.
 */
class CalculatorTest {

    private final Calculator calculator = new Calculator();

    @Test
    void testAddition() {
        assertEquals(5.0, calculator.add(2.0, 3.0), 0.001);
        assertEquals(-1.0, calculator.add(-2.0, 1.0), 0.001);
    }

    @Test
    void testMultiplication() {
        assertEquals(6.0, calculator.multiply(2.0, 3.0), 0.001);
        assertEquals(0.0, calculator.multiply(0.0, 5.0), 0.001);
    }

    @Test
    void testDivision() {
        assertEquals(2.0, calculator.divide(6.0, 3.0), 0.001);

        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(6.0, 0.0);
        });
        assertEquals("Denominator cannot be zero.", exception.getMessage());
    }

    @Test
    void testSubtraction() {
        assertEquals(1.0, calculator.subtract(3.0, 2.0), 0.001);
        assertEquals(-5.0, calculator.subtract(-2.0, 3.0), 0.001);
    }
}

```

### Práticas de Código Limpo Utilizadas
1. Nomes claros e descritivos: Os nomes de métodos e variáveis são autoexplicativos (add, multiply, divide, subtract).
2. Documentação: Cada método da classe tem comentários que explicam o propósito, parâmetros, e exceções lançadas.
3. Tratamento de exceções: O método divide lida com divisões por zero lançando uma exceção apropriada.
4. Testes robustos: Os testes cobrem cenários básicos e casos extremos (como divisão por zero).
5. Separação de responsabilidades: A lógica está isolada em uma classe específica (Calculator), facilitando a manutenção e os testes.
6. Precisão nos testes: Os métodos de teste utilizam assertEquals com margem de erro (0.001) para cálculos de ponto flutuante.
