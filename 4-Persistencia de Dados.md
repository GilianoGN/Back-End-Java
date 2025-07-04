# Persistência de Dados: O Coração Data-Driven do Seu Back-End Java
Praticamente toda aplicação moderna precisa de um lugar para armazenar informações de forma duradoura. Seja para guardar perfis de usuários, registros de pedidos ou dados de produtos, o __banco de dados__ é o pilar central da persistência. Conectar sua aplicação Java a um banco de dados, manipular seus dados e garantir a integridade dessas informações são tarefas essenciais para qualquer desenvolvedor back-end.
Este artigo guiará você pelos conceitos fundamentais da persistência de dados em Java, desde o SQL básico até as poderosas ferramentas do ecossistema Spring.
## 1. SQL Básico: A Linguagem dos Bancos de Dados Relacionais
Antes de interagir com bancos de dados através do Java, é fundamental compreender o __SQL (Structured Query Language)__, a linguagem padrão para gerenciar e manipular bancos de dados relacionais. O SQL é dividido principalmente em duas categorias de comandos:
### DDL (Data Definition Language) - Definição de Dados
Comandos DDL são usados para definir, modificar e apagar a estrutura do banco de dados (tabelas, índices, visões, etc.).
* _CREATE TABLE_: Cria uma nova tabela no banco de dados.
```sql
CREATE TABLE produtos (
    id BIGINT PRIMARY KEY AUTO_INCREMENT, -- Ou SERIAL para PostgreSQL
    nome VARCHAR(255) NOT NULL,
    preco DECIMAL(10, 2) NOT NULL,
    data_cadastro TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
* _ALTER TABLE_: Modifica a estrutura de uma tabela existente (adiciona/remove colunas, altera tipos).
```sql
ALTER TABLE produtos
ADD COLUMN descricao VARCHAR(500);
```
* _DROP TABLE_: Apaga uma tabela inteira do banco de dados.
```sql
DROP TABLE produtos;
```
### DML (Data Manipulation Language) - Manipulação de Dados
Comandos DML são usados para manipular os dados dentro das tabelas (inserir, consultar, atualizar, apagar).
* _INSERT INTO__ Adiciona novos registros (linhas) a uma tabela.
```SQL
INSERT INTO produtos (nome, preco)
VALUES ('Smartphone X', 1500.00);
```
* _SELECT_: Recupera dados de uma ou mais tabelas. É o comando mais usado e flexível.
```SQL
SELECT id, nome, preco FROM produtos WHERE preco > 1000.00 ORDER BY nome ASC;
```
* _UPDATE_: Modifica dados existentes em uma tabela.
```SQL
UPDATE produtos
SET preco = 1600.00
WHERE nome = 'Smartphone X';
```
* _DELETE FROM_: Remove registros de uma tabela.
```SQL
DELETE FROM produtos WHERE preco < 500.00;
```
## 2. JDBC (Java Database Connectivity): A Base da Conexão Java
__JDBC__ é a API padrão do Java para conexão e interação com bancos de dados relacionais. Ele fornece um conjunto de interfaces e classes que permitem que aplicações Java executem comandos SQL, manipulem transações e obtenham resultados de consultas.
Embora o JDBC seja de baixo nível e raramente usado diretamente em aplicações Spring modernas, entender seus fundamentos é crucial, pois frameworks como JPA/Hibernate são construídos sobre ele.
### Fluxo Básico do JDBC:
1. __Carregar o Driver JDBC__: O driver específico do banco de dados (ex: MySQL Connector/J, PostgreSQL JDBC Driver).
2. __Estabelecer Conexão__: Usar _DriverManager.getConnection()_ com a URL do banco de dados, usuário e senha.
3. __Criar um Statement__: Objeto para enviar comandos SQL ao banco de dados (Statement para SQL estático, _PreparedStatement_ para SQL com parâmetros – mais seguro e eficiente).
4. __Executar a Consulta__: Usar _execute()_, _executeQuery()_ (para SELECT) ou _executeUpdate()_ (para INSERT, UPDATE, DELETE).
5. __Processar o Resultado (se houver)__: Iterar sobre um _ResultSet_ para obter os dados.
6. __Fechar Recursos__: Fechar _ResultSet_, _Statement_ e _Connection_ para liberar recursos.
### Exemplo Simples de JDBC (Conceitual):
```Java
import java.sql.*;

public class ExemploJDBC {
    public static void main(String[] args) {
        String url = "jdbc:postgresql://localhost:5432/mydatabase";
        String user = "myuser";
        String password = "mypassword";

        try (Connection connection = DriverManager.getConnection(url, user, password);
             Statement statement = connection.createStatement()) {

            // Exemplo de INSERT
            statement.executeUpdate("INSERT INTO produtos (nome, preco) VALUES ('Tablet Y', 800.00)");
            System.out.println("Produto inserido com sucesso!");

            // Exemplo de SELECT
            ResultSet resultSet = statement.executeQuery("SELECT id, nome, preco FROM produtos");
            while (resultSet.next()) {
                long id = resultSet.getLong("id");
                String nome = resultSet.getString("nome");
                double preco = resultSet.getDouble("preco");
                System.out.println("ID: " + id + ", Nome: " + nome + ", Preço: " + preco);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
Este código é mais didático. Em uma aplicação real, você usaria um datasource gerenciado e frameworks ORM.
## 3. Spring Data JPA e Hibernate: A Abstração Essencial
A abordagem JDBC, embora poderosa, pode ser verbosa e repetitiva. É aí que entram as tecnologias de __Mapeamento Objeto-Relacional (ORM)__ como o __Hibernate__, e a abstração fornecida pelo __Spring Data JPA__.
* __ORM (Object-Relational Mapping)__: É uma técnica de programação que permite mapear objetos da sua aplicação (classes Java) para entidades em um banco de dados relacional (tabelas). O ORM age como uma ponte, eliminando a necessidade de escrever SQL manualmente para cada operação CRUD (Create, Read, Update, Delete). Você interage com objetos Java, e o ORM (Hibernate) traduz essas operações para SQL para você.
* __JPA (Java Persistence API)__: É a especificação padrão do Java para ORM. Ela define as interfaces e anotações para o mapeamento e a persistência de objetos. O Hibernate é a implementação mais popular da JPA.
* __Spring Data JPA__: É uma parte do Spring Framework que simplifica ainda mais o uso da JPA/Hibernate. Ele fornece interfaces de repositório que, com base em convenções de nomes de métodos, geram automaticamente as implementações de consulta SQL para você, reduzindo drasticamente o código boilerplate de acesso a dados.
### Mapeamento Objeto-Relacional com Anotações
Para mapear suas classes Java para tabelas de banco de dados, você usa anotações JPA:
* _@Entity_: Marca uma classe Java como uma entidade JPA, indicando que ela representa uma tabela no banco de dados.
```Java
import jakarta.persistence.Entity; // ou javax.persistence.Entity para versões antigas
@Entity
public class Produto { /* ... */ }
```
* _@Table_: (Opcional) Especifica o nome da tabela no banco de dados, se for diferente do nome da classe.
```Java
@Entity
@Table(name = "tb_produtos") // Mapeia a classe Produto para a tabela tb_produtos
public class Produto { /* ... */ }
```
* _@Id_: Marca o campo que será a chave primária da tabela.
* _@GeneratedValue_: Configura como a chave primária é gerada (ex: auto-incremento no banco de dados).
```Java
import jakarta.persistence.*; // Importa todas as anotações JPA

@Entity
@Table(name = "tb_produtos")
public class Produto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // AUTO_INCREMENT para a maioria dos BDs
    private Long id;
    // ...
}
```
* _@Column_: (Opcional) Especifica detalhes da coluna, como nome, nulidade, comprimento, se for diferente das convenções.
```Java

@Column(name = "nome_produto", nullable = false, length = 200)
private String nome;
```
### Relacionamentos entre Entidades
A força dos bancos de dados relacionais está nos relacionamentos. JPA/Hibernate e Spring Data JPA fornecem anotações para mapeá-los:
* _@OneToOne_: Um registro de uma tabela se relaciona com exatamente um registro de outra.
    * __Exemplo__: Um _Pedido_ tem um _EnderecoEntrega_.
* _@OneToMany_: Um registro de uma tabela se relaciona com múltiplos registros de outra.
    * __Exemplo__: Um _Cliente_ pode ter muitos _Pedidos_.
* _@ManyToOne_: Múltiplos registros de uma tabela se relacionam com um único registro de outra. (Este é o lado "muitos" de um _OneToMany_).
    * __Exemplo__: Muitos _Pedidos_ pertencem a um _Cliente_.
* _@ManyToMany_: Múltiplos registros de uma tabela se relacionam com múltiplos registros de outra. Geralmente requer uma tabela intermediária no banco de dados.
    * __Exemplo__: Muitos _Alunos_ podem cursar muitas _Disciplinas_.
## Exemplo de Relacionamento _@ManyToOne_ (Cliente -> Pedido):
```Java
// Classe Cliente
@Entity
public class Cliente {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String nome;

    @OneToMany(mappedBy = "cliente", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Pedido> pedidos = new ArrayList<>(); // Um cliente pode ter muitos pedidos
    // ... construtores, getters, setters
}

// Classe Pedido
@Entity
public class Pedido {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private LocalDate dataPedido;
    private Double valorTotal;

    @ManyToOne // Muitos pedidos para um cliente
    @JoinColumn(name = "cliente_id") // Define a coluna da chave estrangeira na tabela de Pedidos
    private Cliente cliente; // O pedido pertence a um cliente

    // ... construtores, getters, setters
}
```
### Queries com Spring Data JPA
Spring Data JPA simplifica a criação de queries de forma incrível através de interfaces de repositório.
* _JpaRepository<T, ID>_: A interface base que fornece métodos CRUD (Create, Read, Update, Delete) comuns prontos para uso (ex: _save()_, _findById()_, _findAll()_, _deleteById()_).
* __Métodos de Query Derivados__: Você pode simplesmente declarar métodos com nomes específicos na sua interface de repositório, e o Spring Data JPA gerará a implementação da query automaticamente.
```Java
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface ClienteRepository extends JpaRepository<Cliente, Long> {
    // Encontra clientes pelo nome
    List<Cliente> findByNome(String nome);

    // Encontra clientes pelo email E nome, ignorando case no email
    List<Cliente> findByEmailIgnoreCaseAndNome(String email, String nome);

    // Encontra clientes que contêm uma string no nome, ordenado pelo nome
    List<Cliente> findByNomeContainingOrderByNomeAsc(String nome);
}
```
* _@Query_: Para queries mais complexas que não podem ser expressas por nomes de métodos, você pode usar a anotação @Query para escrever suas próprias consultas JPQL (Java Persistence Query Language) ou SQL nativo.
    * __JPQL__/__HQL__: Linguagem orientada a objetos que opera sobre entidades JPA, não sobre tabelas.
    * __SQL Nativo__: Se precisar de funcionalidades específicas do banco de dados ou queries complexas que são difíceis em JPQL.
```Java
import org.springframework.data.jpa.repository.Query;

public interface ClienteRepository extends JpaRepository<Cliente, Long> {
    // JPQL: Seleciona clientes cujo nome começa com uma letra específica
    @Query("SELECT c FROM Cliente c WHERE c.nome LIKE ?1%")
    List<Cliente> buscarClientesPorInicialNome(String inicial);

    // SQL Nativo: Seleciona todos os clientes com mais de 5 pedidos
    @Query(value = "SELECT c.* FROM tb_cliente c JOIN tb_pedido p ON c.id = p.cliente_id GROUP BY c.id HAVING COUNT(p.id) > ?1", nativeQuery = true)
    List<Cliente> buscarClientesComMaisDeXPedidos(int numPedidos);
}
```
### Transações: Garantindo a Consistência dos Dados
Transações são cruciais para a integridade dos dados. Elas garantem que um conjunto de operações de banco de dados seja tratado como uma única unidade atômica: ou todas as operações são bem-sucedidas (commit), ou nenhuma é (rollback).
* _@Transactional_: A anotação _@Transactional_ do Spring é a forma mais comum de gerenciar transações. Ao anotá-la em um método (ou classe), o Spring garante que todo o código dentro desse método será executado dentro de uma transação. Se uma exceção ocorrer, a transação será revertida automaticamente.
```Java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class PedidoService {

    private final PedidoRepository pedidoRepository;
    private final ProdutoRepository produtoRepository;

    public PedidoService(PedidoRepository pedidoRepository, ProdutoRepository produtoRepository) {
        this.pedidoRepository = pedidoRepository;
        this.produtoRepository = produtoRepository;
    }

    @Transactional // Garante que as duas operações sejam atômicas
    public Pedido criarNovoPedido(Pedido pedido, List<Long> idsProdutos) {
        // 1. Salva o pedido
        Pedido pedidoSalvo = pedidoRepository.save(pedido);

        // 2. Associa produtos ao pedido (exemplo simplificado)
        for (Long produtoId : idsProdutos) {
            Produto produto = produtoRepository.findById(produtoId)
                                .orElseThrow(() -> new RuntimeException("Produto não encontrado"));
            // Lógica de associação de produto ao pedido
        }
        // Se qualquer coisa der errado aqui, todo o método será revertido.
        return pedidoSalvo;
    }
}
```
## 4. Bancos de Dados: Escolhendo a Solução Certa
Para o desenvolvimento back-end Java, a escolha do banco de dados relacional é vital.
* __Bancos de Dados para Desenvolvimento (Em Memória)__:
    * __H2 Database__: Extremamente popular para desenvolvimento e testes. É um banco de dados Java em memória que pode ser incorporado diretamente na sua aplicação. Ele é rápido, não exige instalação separada e o schema pode ser recriado a cada inicialização da aplicação, o que é ótimo para testes.
    * __HSQLDB, Derby__: Outras opções de bancos de dados Java em memória.
* __Bancos de Dados Populares para Produção__:
    * __PostgreSQL__: Um dos bancos de dados relacionais de código aberto mais avançados e robustos. É altamente confiável, oferece um rico conjunto de recursos e é uma excelente escolha para a maioria das aplicações.
    * __MySQL__: Outro banco de dados de código aberto extremamente popular, amplamente utilizado em aplicações web. É conhecido por sua velocidade e facilidade de uso.
    * __Oracle, SQL Server__: Bancos de dados comerciais robustos, frequentemente usados em ambientes corporativos grandes e complexos.
A configuração da conexão com o banco de dados é feita geralmente nos arquivos _application.properties_ ou _application.yml_ do Spring Boot, como visto em outros artigos.
## Conclusão
A persistência de dados é o coração de qualquer aplicação transacional. Desde a compreensão dos comandos SQL que interagem diretamente com o banco, passando pela base do JDBC, até a abstração e facilidade oferecidas pelo Spring Data JPA e Hibernate, o caminho para gerenciar dados em Java é bem pavimentado. Ao dominar essas ferramentas e conceitos, você estará apto a construir sistemas back-end Java que não apenas armazenam informações de forma eficiente, mas também garantem a consistência e a integridade dos dados essenciais para o sucesso de qualquer aplicação.