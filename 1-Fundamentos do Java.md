# Fundamentos de Java e Orientação a Objetos: A Base do Seu Back-End
Para quem busca dominar o desenvolvimento back-end em Java, ter uma base sólida nos fundamentos da linguagem e nos princípios da Orientação a Objetos (OO) é mais do que essencial: é mandatório. Mesmo que você já tenha algum contato com Java, uma revisão focada nos pilares que sustentam as aplicações corporativas modernas garante que você construa códigos eficientes, manuteníveis e escaláveis.
## Sintaxe, Estruturas de Dados e o Coração da Linguagem
A familiaridade com a sintaxe básica e as estruturas de dados de Java é o ponto de partida para qualquer desenvolvedor. É o vocabulário e a gramática que você usará para "conversar" com a máquina.
### Variáveis, Tipos de Dados e Operadores
Java é uma linguagem fortemente tipada, o que significa que toda variável deve ter um tipo definido.
* __Tipos de Dados Primitivos__: Armazenam valores simples diretamente. São eles: _byte_, _short_, _int_, _long_ (para números inteiros); _float_, _double_ (para números de ponto flutuante); _char_ (para caracteres); e _boolean_ (para valores verdadeiro/falso).
* __Tipos de Dados Wrapper__: São classes que "empacotam" os tipos primitivos, permitindo que eles sejam tratados como objetos. Isso é crucial para uso em coleções (que só aceitam objetos) e para oferecer métodos úteis (ex: _Integer.parseInt()_).
    * Ex: _Integer_ para _int_, _Double_ para _double_, _Boolean_ para _boolean_.
* __Variáveis__: Contêineres que armazenam valores de um determinado tipo.
```java
int idade = 30; // Variável primitiva
String nome = "Maria"; // Variável de referência (String é uma classe)
Double preco = 99.99; // Variável wrapper
boolean ativo = true;
```
* __Operadores__: Símbolos usados para realizar operações.
    * __Aritméticos__: _+_, _-_, _*_, _/_, _%_
    * __Relacionais__: _==_, _!=_, _<_, _>_, _<=_, _>=_
    * __Lógicos__: _&&_ (E), _||_ (OU), _!_ (NÃO)
    * __Atribuição__: _=_, _+=_, _-=_, _*=_ etc.
### Estruturas de Controle: O Fluxo da Lógica
Essas estruturas permitem controlar o fluxo de execução do seu programa com base em condições ou repetições.
* _if_/_else if_/_else_: Para execução condicional.
```java
int temperatura = 25;
if (temperatura > 30) {
    System.out.println("Está muito quente!");
} else if (temperatura < 10) {
    System.out.println("Está muito frio!");
} else {
    System.out.println("Temperatura agradável.");
}
```
* _switch_: Para múltiplas opções baseadas em um único valor.
```java
char opcao = 'B';
switch (opcao) {
    case 'A':
        System.out.println("Opção A selecionada.");
        break;
    case 'B':
        System.out.println("Opção B selecionada.");
        break;
    default:
        System.out.println("Opção inválida.");
}
```
* Loops (_for_, _while_, _do-while_, _for-each_): Para repetir blocos de código.
```java
// For tradicional
for (int i = 0; i < 5; i++) {
    System.out.println("Contador: " + i);
}

// While
int count = 0;
while (count < 3) {
    System.out.println("Contagem while: " + count);
    count++;
}
```
### Arrays e Coleções: Organizando Seus Dados
* __Arrays__: Estruturas de dados de tamanho fixo que armazenam elementos do mesmo tipo.
```java
String[] nomes = new String[3];
nomes[0] = "Alice";
nomes[1] = "Bob";
System.out.println(nomes[0]); // Saída: Alice
```
* __Coleções (Java Collections Framework)__: Estruturas de dados flexíveis e dinâmicas, que são a espinha dorsal para manipular grupos de objetos em Java.
_List_: Coleção ordenada que permite elementos duplicados e acesso por índice. Implementações comuns: _ArrayList_, _LinkedList_.
```java
import java.util.ArrayList;
import java.util.List;
List<String> cidades = new ArrayList<>();
cidades.add("São Paulo");
cidades.add("Rio de Janeiro");
cidades.add("São Paulo"); // Permite duplicados
System.out.println(cidades.get(0)); // Saída: São Paulo
```
_Set_: Coleção que não permite elementos duplicados. Não garante ordem. Implementações comuns: _HashSet_, _TreeSet_.
```java
import java.util.HashSet;
import java.util.Set;
Set<String> cores = new HashSet<>();
cores.add("Vermelho");
cores.add("Azul");
cores.add("Vermelho"); // Não adiciona duplicado
System.out.println(cores); // Saída: [Azul, Vermelho] (ordem pode variar)
```
_Map_: Armazena pares de chave-valor, onde cada chave é única. Implementações comuns: _HashMap_, _TreeMap_.
```java
import java.util.HashMap;
import java.util.Map;
Map<String, Integer> idades = new HashMap<>();
idades.put("Carlos", 40);
idades.put("Ana", 25);
System.out.println(idades.get("Ana")); // Saída: 25
```
## Programação Orientada a Objetos (POO) - A Essência do Java
Java é uma linguagem fundamentalmente orientada a objetos. Compreender os conceitos de POO é vital para escrever código modular, reutilizável e de fácil manutenção, características cruciais para sistemas de back-end.
### Conceitos Fundamentais da POO
* __Classes__: São os "moldes" ou "plantas" para criar objetos. Definem atributos (características) e métodos (comportamentos) comuns a um grupo de objetos.
* __Objetos__: São instâncias concretas de uma classe. Cada objeto tem seus próprios valores para os atributos definidos na classe.
* __Atributos (Campos/Variáveis de Instância)__: São as características ou propriedades de um objeto.
* __Métodos (Funções/Comportamentos)__: São as ações que um objeto pode realizar ou as operações que podem ser realizadas nele.
* __Construtores__: Métodos especiais usados para inicializar um objeto quando ele é criado. Têm o mesmo nome da classe e não possuem tipo de retorno.
### Exemplo de Classe e Objeto:
```java
public class Carro {
    // Atributos (características)
    String marca;
    String modelo;
    int ano;
    String cor;

    // Construtor
    public Carro(String marca, String modelo, int ano, String cor) {
        this.marca = marca;
        this.modelo = modelo;
        this.ano = ano;
        this.cor = cor;
    }

    // Métodos (comportamentos)
    public void ligar() {
        System.out.println(marca + " " + modelo + " ligado!");
    }

    public void exibirDetalhes() {
        System.out.println("Marca: " + marca + ", Modelo: " + modelo + ", Ano: " + ano + ", Cor: " + cor);
    }

    public static void main(String[] args) {
        // Criando objetos (instâncias da classe Carro)
        Carro meuCarro = new Carro("Toyota", "Corolla", 2020, "Prata");
        Carro outroCarro = new Carro("Honda", "Civic", 2022, "Preto");

        // Acessando atributos e chamando métodos dos objetos
        meuCarro.ligar(); // Saída: Toyota Corolla ligado!
        meuCarro.exibirDetalhes(); // Saída: Marca: Toyota, Modelo: Corolla, Ano: 2020, Cor: Prata

        outroCarro.exibirDetalhes();
    }
}
```
### Pilares da POO
* __Encapsulamento__: O princípio de agrupar dados (atributos) e os métodos que operam sobre esses dados em uma única unidade (a classe), e de restringir o acesso direto a alguns dos componentes do objeto. Geralmente alcançado usando modificadores de acesso (_private_, _public_, _protected_). __A ideia é esconder os detalhes de implementação e expor apenas uma interface clara__.
```java
public class ContaBancaria {
    private String numeroConta; // Atributo privado
    private double saldo;       // Atributo privado

    // Métodos públicos (Getters e Setters) para acessar e modificar dados de forma controlada
    public String getNumeroConta() {
        return numeroConta;
    }

    public double getSaldo() {
        return saldo;
    }

    public void depositar(double valor) {
        if (valor > 0) {
            this.saldo += valor;
        }
    }
    // ... outros métodos
}
```
* __Herança__: Permite que uma classe (subclasse/filha) herde atributos e métodos de outra classe (superclasse/pai). Promove a __reutilização de código__ e estabelece uma relação "É UM" (Ex: _Carro É UM Veiculo_). Usa a palavra-chave _extends_.
```java
class Veiculo {
    String cor;
    void mover() { System.out.println("Veículo movendo."); }
}

class Carro extends Veiculo { // Carro herda de Veiculo
    String modelo;
    void buzinar() { System.out.println("Beep beep!"); }
}
```
* __Polimorfismo__: Significa "muitas formas". Permite que objetos de diferentes classes sejam tratados como objetos de uma superclasse comum. Métodos podem ser sobrescritos na subclasse (_@Override_). Ocorre em tempo de execução (polimorfismo dinâmico).
```java
class Animal {
    void fazerBarulho() { System.out.println("Animal faz barulho."); }
}

class Cachorro extends Animal {
    @Override
    void fazerBarulho() { System.out.println("Au au!"); }
}

class Gato extends Animal {
    @Override
    void fazerBarulho() { System.out.println("Miau!"); }
}

public class TestePolimorfismo {
    public static void main(String[] args) {
        Animal meuAnimal1 = new Cachorro();
        Animal meuAnimal2 = new Gato();

        meuAnimal1.fazerBarulho(); // Saída: Au au!
        meuAnimal2.fazerBarulho(); // Saída: Miau!
    }
}
```
* __Abstração__: O processo de mostrar apenas os detalhes relevantes para o usuário do objeto e esconder os detalhes complexos da implementação. Pode ser alcançada usando classes abstratas e interfaces. __A ideia é focar no "o quê" um objeto faz, e não no "como" ele faz__.
### Interfaces e Classes Abstratas: Contratos e Hierarquias Flexíveis
* __Classes Abstratas__: Classes que não podem ser instanciadas diretamente e podem conter métodos abstratos (sem implementação) e métodos concretos. São usadas para criar uma hierarquia base onde algumas funcionalidades são comuns, mas outras precisam ser implementadas pelas subclasses. Usam a palavra-chave _abstract_.
```java
abstract class FormaGeometrica {
    String cor;
    abstract double calcularArea(); // Método abstrato, sem implementação
    void exibirCor() { System.out.println("Cor: " + cor); } // Método concreto
}

class Circulo extends FormaGeometrica {
    double raio;
    @Override
    double calcularArea() { return Math.PI * raio * raio; }
}
```
* __Interfaces__: Definem um "contrato" de métodos que as classes devem implementar. Uma classe pode implementar múltiplas interfaces, promovendo a herança múltipla de "comportamento" (mas não de estado). Todos os métodos em interfaces (antes do Java 8) eram implicitamente _public abstract_. Com Java 8+, podem ter métodos _default_ e _static_. Usam a palavra-chave _interface_.
```java
interface Desenhavel {
    void desenhar(); // Método abstrato
    default void exibirMensagem() { // Método default (Java 8+)
        System.out.println("Sou desenhável!");
    }
}

class Quadrado implements Desenhavel {
    @Override
    public void desenhar() { System.out.println("Desenhando um quadrado."); }
}
```
## Tratamento de Exceções, Streams API e Generics
Estes são recursos mais avançados, mas indispensáveis para o desenvolvimento de aplicações Java robustas e modernas.
### Tratamento de Exceções: Lidando com o Inesperado
Exceções são eventos que interrompem o fluxo normal de um programa. O tratamento de exceções é crucial para que sua aplicação possa se recuperar graciosamente de erros.
* _try-catch-finally_: Blocos de código para lidar com exceções.
    * _try_: Onde o código que pode gerar uma exceção é colocado.
    * _catch_: Captura e trata a exceção se ela ocorrer.
    * _finally_: Bloco opcional que sempre é executado, independentemente de uma exceção ter sido lançada ou não (útil para fechar recursos).
* _throws_: Usado na assinatura de um método para indicar que ele pode lançar uma exceção de um determinado tipo.
* __Exceções Checked vs. Unchecked__:
    * __Checked Exceptions__: Exceções que o compilador força você a tratar (usando _try-catch_ ou _throws_). Geralmente representam condições recuperáveis (ex: _IOException_, _SQLException_).
    * __Unchecked Exceptions (Runtime Exceptions)__: Exceções que o compilador não força o tratamento. Geralmente indicam erros de programação (ex: _NullPointerException_, _ArrayIndexOutOfBoundsException_).
### Exemplo de Tratamento de Exceções:
```java
import java.io.FileReader;
import java.io.IOException;

public class ExemploExcecoes {
    public static void main(String[] args) {
        try {
            // Código que pode lançar uma checked exception (IOException)
            FileReader reader = new FileReader("arquivo.txt");
            System.out.println("Arquivo lido com sucesso!");
            reader.close(); // Importante fechar recursos!
        } catch (IOException e) { // Captura a exceção específica
            System.err.println("Erro ao ler o arquivo: " + e.getMessage());
        } catch (NullPointerException e) { // Captura outra possível exceção unchecked
            System.err.println("Erro de ponteiro nulo: " + e.getMessage());
        } finally {
            System.out.println("Bloco finally sempre é executado.");
        }

        // Exemplo de método que declara que pode lançar uma exceção
        try {
            dividir(10, 0);
        } catch (ArithmeticException e) {
            System.err.println("Erro de divisão por zero capturado no main: " + e.getMessage());
        }
    }

    public static double dividir(int a, int b) throws ArithmeticException {
        if (b == 0) {
            throw new ArithmeticException("Divisão por zero não permitida!"); // Lança uma unchecked exception
        }
        return (double) a / b;
    }
}
```
### Streams API (Java 8+): Manipulação Funcional de Coleções
A Streams API, introduzida no Java 8, trouxe um novo paradigma para processar coleções de dados de forma funcional, declarativa e mais eficiente. Ela permite que você encadeie operações para filtrar, mapear, agrupar e reduzir dados sem a necessidade de loops explícitos.
__Benefícios__:
* __Concisão__: Código mais curto e legível.
* __Expressividade__: Foca no "o quê" fazer, não no "como".
* __Paralelismo__: Suporte fácil para operações paralelas para melhorar a performance em grandes datasets.
__Exemplo de Streams API__:
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class ExemploStreams {
    public static void main(String[] args) {
        List<String> palavras = Arrays.asList("apple", "banana", "cherry", "date", "elderberry");

        // Filtrar palavras que começam com 'a', transformá-las para maiúsculas e coletar em uma nova lista
        List<String> resultado = palavras.stream()
            .filter(p -> p.startsWith("a"))      // Operação intermediária: filtra
            .map(String::toUpperCase)           // Operação intermediária: mapeia para maiúsculas
            .collect(Collectors.toList());      // Operação terminal: coleta em uma lista

        System.out.println(resultado); // Saída: [APPLE]

        // Contar o número de palavras com mais de 5 caracteres
        long count = palavras.stream()
            .filter(p -> p.length() > 5)
            .count();
        System.out.println("Palavras com mais de 5 caracteres: " + count); // Saída: 2
    }
}
```
### Generics: Programação Genérica e Segurança de Tipo
Generics (ou Generics) permitem que você escreva classes, interfaces e métodos que operam com tipos de dados de forma __genérica__, mas com __segurança de tipo__ em tempo de compilação. Isso significa que você pode criar estruturas que funcionam com qualquer tipo, mas o compilador garante que os tipos sejam consistentes, evitando _ClassCastException_ em tempo de execução.
__Benefícios__:
* __Segurança de Tipo__: Erros de tipo são detectados em tempo de compilação, não em tempo de execução.
* __Reutilização de Código__: O mesmo código pode ser usado para diferentes tipos de dados.
* __Clareza__: O código é mais claro sobre os tipos com os quais ele opera.
__Exemplo de Generics__:
```java
// Classe genérica que pode armazenar qualquer tipo de dado
class Caixa<T> { // T é o tipo genérico
    private T conteudo;

    public Caixa(T conteudo) {
        this.conteudo = conteudo;
    }

    public T getConteudo() {
        return conteudo;
    }

    public void setConteudo(T conteudo) {
        this.conteudo = conteudo;
    }
}

public class ExemploGenerics {
    public static void main(String[] args) {
        // Criando uma Caixa para Strings
        Caixa<String> caixaDeString = new Caixa<>("Olá, Generics!");
        String texto = caixaDeString.getConteudo(); // Não precisa de cast!
        System.out.println("Conteúdo da caixa de String: " + texto);

        // Criando uma Caixa para Integers
        Caixa<Integer> caixaDeNumero = new Caixa<>(123);
        int numero = caixaDeNumero.getConteudo(); // Não precisa de cast!
        System.out.println("Conteúdo da caixa de Número: " + numero);

        // Erro em tempo de compilação: caixaDeString.setConteudo(456); // Isso não compila!
    }
}
```
## Conclusão
Os fundamentos de Java, desde a sintaxe básica e estruturas de dados até os pilares da Orientação a Objetos, o tratamento de exceções, a moderna Streams API e o poder dos Generics, formam a base indispensável para qualquer desenvolvedor que busca construir um back-end robusto e eficiente. Dominar esses conceitos não é apenas sobre escrever código, mas sobre pensar de forma estruturada, modular e resiliente, preparando o terreno para mergulhar em frameworks complexos como o Spring e construir aplicações de alto nível.