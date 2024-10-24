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
