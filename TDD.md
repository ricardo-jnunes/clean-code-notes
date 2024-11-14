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
