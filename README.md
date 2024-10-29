# Clean Code Notes

Princípios da Engenharia de Software, do livro de Robert C. Martin.
Isto não é um guia de estilos. É um guia para se produzir código legível, reutilizável e refatorável.

### Por que escrever código ruim?
- Você está com pressa?
- Está tentando ser "rápido"?
- Não tem tempo para fazer um bom trabalho?
- Está cansado de trabalhar no mesmo programa/módulo?
- Seu chefe está te pressionando para terminar logo?
- Esses argumentos podem criar um pântano de código sem sentido.

Se você disser "Eu volto para corrigir isso depois", pode cair na Lei de LeBlanc "Depois significa nunca".

Você é um profissional, e o código é sua responsabilidade. Vamos analisar a seguinte anedota:
>E se você fosse um médico e tivesse um paciente que exigisse que você parasse com toda aquela "besteira" de lavar as mãos antes da cirurgia porque isso estava demorando muito? Claramente, o paciente é o chefe; e ainda assim, o médico deveria recusar-se a obedecer. Por quê? Porque o médico sabe mais que o paciente sobre os riscos de doenças e infecções. Seria antiético (sem falar em criminoso) para o médico cumprir essa exigência.

Da mesma forma, é antiético que programadores cedam à vontade de gerentes que não compreendem os riscos de fazer "gambiarras".

Talvez em algum momento você pense em ser rápido para cumprir o prazo. A única maneira de ser rápido é manter o código o mais limpo possível o tempo todo.

### A Regra do Escoteiro / The Boy Scout Rule
Não basta escrever o código bem. O código precisa ser mantido limpo ao longo do tempo. Todos nós já vimos código se deteriorar e perder qualidade à medida que o tempo passa. Portanto, devemos assumir um papel ativo em prevenir essa degradação.

É uma boa prática aplicar a [Regra do Escoteiro](https://www.oreilly.com/library/view/97-things-every/9780596809515/ch08.html): "Deixe o acampamento mais limpo do que você encontrou".

## **Variáveis**
### Meaningful Names - Use nomes de variáveis que revelam seu propósito e que sejam pronunciáveis

**Ruim:**
```javascript
const yyyymmdstr = moment().format('YYYY/MM/DD');
```

**Bom:**
```javascript
const currentDate = moment().format('YYYY/MM/DD');
```

**Ruim:**
```java
class DtaRcrd102 {
  private Date genymdhms;
  private Date modymdhms;
  private final String pszqint = "102";
  /* ... */
};
```

**Bom:**
```java
class Customer {
  private Date generationTimestamp;
  private Date modificationTimestamp;;
  private final String recordId = "102";
  /* ... */
};
```

### Use o mesmo vocabulário para o mesmo tipo de variável

**Ruim:**
```java
getUserInfo();
getClientData();
getCustomerRecord();
```

**Bom:**
```java
getUser();
```

### Use nomes que revelam a intenção

**Ruim:**
```java
public List<int[]> getThem() {
  List<int[]> list1 = new ArrayList<int[]>();
  for (int[] x : theList)
    if (x[0] == 4)
      list1.add(x);
  return list1;
}
```

**Bom:**
```java
public List<int[]> getFlaggedCells() {
  List<int[]> flaggedCells = new ArrayList<int[]>();
  for (int[] cell : gameBoard)
    if (cell[STATUS_VALUE] == FLAGGED)
      flaggedCells.add(cell);
  return flaggedCells;
}
```

**Melhor:**
```java
public List<Cell> getFlaggedCells() {
  List<Cell> flaggedCells = new ArrayList<Cell>();
  for (Cell cell : gameBoard)
    if (cell.isFlagged())
      flaggedCells.add(cell);
  return flaggedCells;
}
```

### Use nomes pesquisáveis

**Ruim:**
```javascript
// Para que diabos serve 86400000?
setTimeout(blastOff, 86400000);

```

**Bom:**
```javascript
// Declare-as como `const` global em letras maiúsculas.
const MILLISECONDS_PER_DAY = 86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);

```

### Evite Mapeamento Mental
Explicito é melhor que implícito.

**Ruim:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((l) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Espera, para que serve o `l` mesmo?
  dispatch(l);
});
```

**Bom:**
```javascript
const locations = ['Austin', 'New York', 'San Francisco'];
locations.forEach((location) => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```


### Funções

#### Mantenha-as pequenas!
A primeira regra das funções é que elas devem ser pequenas.

>Funções devem fazer uma única coisa. Devem fazer isso bem. Devem fazer somente isso.

#### Use Nomes Descritivos
Você sabe que está trabalhando com código limpo quando cada rotina faz exatamente o que você esperava.

Quanto menor e mais focada for a função, mais fácil será escolher um nome descritivo.

Não tenha medo de dar um nome longo. Um nome descritivo longo é melhor do que um nome curto e enigmático. Um nome descritivo longo é melhor do que um longo comentário descritivo. Use uma convenção de nomenclatura que permita a leitura fácil de várias palavras nos nomes das funções e, em seguida, utilize essas palavras para dar à função um nome que diga exatamente o que ela faz.

**Ruim:**
```java
public class Order {
    public void procOrd(Order o, double t) {
        if (o != null) {
            if (t > 100) {
                System.out.println("Order with discount");
                o.total -= 10;
            }
        }
    }
}
```

**Bom:**
```java
public class Order {
    public void processOrderWithDiscount(Order order, double total) {
        if (isOrderValid(order)) {
            if (isTotalEligibleForDiscount(total)) {
                applyDiscount(order);
            }
        }
    }

    private boolean isOrderValid(Order order) {
        return order != null;
    }

    private boolean isTotalEligibleForDiscount(double total) {
        return total > 100;
    }

    private void applyDiscount(Order order) {
        System.out.println("Order with discount");
        order.total -= 10;
    }
}
```

### Comentários
#### Comentários Não Compensam Código Ruim

**Ruim:**
```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```
**Bom:**
```java
if (employee.isEligibleForFullBenefits())
```

#### Não deixe código comentado na sua base de código
Controle de versão existe por uma razão. Deixar códigos velhos no seu histórico.

**Ruim:**
```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Bom:**
```javascript
doStuff();
```
### Bons comentários
Alguns comentários são bons e essenciais.

#### Comentários Legais
Às vezes, nossos padrões de codificação corporativos nos obrigam a escrever certos comentários por razões legais. Por exemplo, declarações de copyright e de autoria são necessárias e razoáveis para serem incluídas em um comentário no início de cada arquivo de código-fonte.
> The MIT License (MIT)
> Copyright (c) 2024 
> Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software")

#### Comentários Informativos
Às vezes, é útil fornecer informações básicas por meio de um comentário.

```java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

#### Aviso sobre Consequências
Às vezes, é útil alertar outros programadores sobre certas consequências.

```java
// Don't run unless you
// have some time to kill.
public void _testWithReallyBigFile() {
  writeLinesToFile(10000000);
  response.setBody(testFile);
  response.readyToSend(this);
  String responseString = output.toString();
  assertSubString("Content-Length: 1000000000", responseString);
  assertTrue(bytesSent > 1000000000);
}
```
#### Comentários TODO

Às vezes, é razoável deixar notas de 'Para fazer' na forma de comentários //TODO. 
No caso a seguir, o comentário TODO explica por que a função tem uma implementação degenerada e qual deve ser o futuro dessa função.

```java
/**
     * Checks if the given string is a palindrome.
     * The current implementation always returns false, which is not correct.
     *
     * TODO: Implement the logic to properly check for palindromes.
     * Consider handling case sensitivity and ignoring spaces and punctuation.
     */
    public boolean isPalindrome(String str) {
        // Degenerate implementation; always returns false
        return false; // Placeholder return value
    }
```

#### Regras de Equipe

Todo programador tem suas regras suas formas de formatação prediletas, mas se ele for trabalhar em equipe, as regras são delas.

Uma equipe de desenvoldedores deve escolher um único estilo e então todos da equipe devem usá-lo.
Não queremos que pensem que o código foi escrito por pessoas em desacordo.

#### Objetos e Estruturas de Dados

Há um motivo para declararmos nossas variáveis privadas. Não queremos que ninguém dependa delas.

Objetos devem representar entidades específicas, ter responsabilidades claras e encapsular comportamento.

**Ruim:**
```java
public class User {
    public String name;
    public String email;
    public boolean isActive;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
        this.isActive = false;
    }
}

// Manipulação direta dos dados
User user = new User("Alice", "alice@example.com");
user.isActive = true; // Mau uso, expõe detalhes internos

```

**Bom:**
```java
public class User {
    private String name;
    private String email;
    private boolean isActive;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
        this.isActive = false;
    }

    public void activate() {
        this.isActive = true;
    }

    public void deactivate() {
        this.isActive = false;
    }

    public boolean isActive() {
        return this.isActive;
    }
}

```

**Ruim:**
```java
import java.util.HashMap;
import java.util.Map;

public class UserService {
    private static Map<Integer, String> userDatabase = new HashMap<>();

    public static void addUser(int id, String name) {
        userDatabase.put(id, name);
    }

    public static void displayUser(int id) {
        System.out.println("Usuário: " + userDatabase.get(id));
    }
}

```

**Bom:**
```java
//Aqui, displayUser recebe os dados explicitamente, mantendo o código mais claro e desacoplado da estrutura de dados.
public class UserService {
    public static void displayUser(int id, String name) {
        System.out.println("Usuário: " + name);
    }
}

// Uso
UserService.displayUser(1, "Alice");

```

#### Lei de Demeter
Há um heurística conhecida chamada [Lei de Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter), um módulo não deve enxergar o interior dos objetos que ele manipula.

#### Objeto de transferência de dados - DTO
Estrtura de dados perfeita é uma classe com variáveis públicas e nehnhuma função.

**Bom:**
```java
public class UserDTO {
    private Long id;
    private String name;
    private String email;

    public UserDTO() {
    }

    public UserDTO(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    // Getters e Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    @Override
    public String toString() {
        return "UserDTO{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

```

### Tratamento de Erro


#### Tratamento Básico de Exceções com try-catch

**Ruim:**
```java
/// Evite capturar exceções genéricas (Exception ou Throwable), pois elas podem esconder erros que você não esperava.
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Divisão por zero não é permitida.");
} catch (Exception e) {
    System.out.println("Ocorreu um erro desconhecido."); // Evite isso sempre que possível
}

try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // Evite Silenciar Exceções, evite blocos catch vazios. Prefira lançar uma exceção ou logar o erro.
    e.printStackTrace(); // Imprime o stack trace no console, sem contexto adicional
}

```

**Bom:**
```java
public class BasicExceptionHandling {
    public static void main(String[] args) {
        String input = "123a"; // Entrada inválida

        try {
            int number = Integer.parseInt(input); // Pode lançar NumberFormatException
            System.out.println("Número: " + number);
        } catch (NumberFormatException e) {
            System.out.println("Erro: A entrada não é um número válido.");
        }
    }
}

```

#### Exceções Personalizadas

**Bom:**
```java
// Exceção personalizada
class InvalidAgeException extends Exception {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class CustomExceptionExample {
    public static void main(String[] args) {
        try {
            validateAge(15); // Exemplo com idade inválida
        } catch (InvalidAgeException e) {
            System.out.println(e.getMessage());
        }
    }

    public static void validateAge(int age) throws InvalidAgeException {
        if (age < 18) {
            throw new InvalidAgeException("Erro: Idade mínima é 18 anos.");
        }
        System.out.println("Idade válida.");
    }
}

```

#### Eviste oso de Exceções para Controle de Fluxo

```java
public int findIndex(int[] arr, int value) {
    try {
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] == value) {
                return i;
            }
        }
        throw new Exception("Valor não encontrado"); // Usa exceção para controle de fluxo
    } catch (Exception e) {
        return -1; // Retorna -1 se o valor não for encontrado
    }
}

```
