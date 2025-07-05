# Gerenciamento de Esquema de Banco de Dados com Liquibase: Uma Visão Abrangente
No desenvolvimento de software moderno, a base de dados é muitas vezes o coração da aplicação. Manter o esquema dessa base de dados (tabelas, colunas, índices, stored procedures, etc.) consistente e versionado em diferentes ambientes (desenvolvimento, teste, produção) é um desafio constante. É aqui que entra o __Liquibase__, uma ferramenta open source que simplifica e automatiza o gerenciamento de mudanças no banco de dados.
## O Que É Liquibase?
Liquibase é uma solução agnóstica a banco de dados para controle de versão de esquema de banco de dados. Ele permite que você defina suas mudanças de banco de dados em arquivos de texto (como XML, YAML, JSON ou SQL), organize-as de forma sequencial e aplique-as de maneira controlada e rastreável. Em essência, é como usar Git para o seu código-fonte, mas para o seu banco de dados.
## Por Que Usar Liquibase?
O gerenciamento manual de mudanças de banco de dados é propenso a erros, especialmente em equipes grandes ou ambientes complexos. O Liquibase resolve problemas comuns como:
1. __Consistência de Ambiente__: Garante que o esquema do banco de dados seja idêntico em todos os ambientes (desenvolvimento, QA, produção), evitando o famoso "funcionou na minha máquina".
2. __Rastreamento de Mudanças__: Cada alteração é versionada e registrada. Você sabe exatamente quem fez o quê, quando e por quê.
3. __Rollbacks Controlados__: Se uma mudança causar um problema, o Liquibase pode reverter o banco de dados para um estado anterior de forma segura e confiável.
4. __Automação de Implantação (CI/CD)__: Integra-se facilmente a pipelines de CI/CD, automatizando a aplicação de mudanças no banco de dados como parte do processo de deploy.
5. __Colaboração em Equipe__: Facilita o trabalho de múltiplos desenvolvedores no mesmo banco de dados sem sobrescrever as alterações uns dos outros.
6. __Agnóstico a Banco de Dados__: Suporta uma vasta gama de bancos de dados, como MySQL, PostgreSQL, Oracle, SQL Server, H2, e muitos outros. Você escreve a mudança uma vez e o Liquibase a traduz para a sintaxe específica do banco de dados.
## Como o Liquibase Funciona?
O coração do Liquibase são os Changelogs e os Changesets.
* __Changelog__: É o arquivo principal (geralmente _db.changelog-master.xml_ ou similar) que atua como um índice para todas as suas mudanças de banco de dados. Ele define a ordem em que os changesets devem ser aplicados.
* __Changeset__: Representa uma única unidade de mudança no seu banco de dados (por exemplo, criar uma tabela, adicionar uma coluna, inserir dados). Cada changeset possui um _id_ único e um _author_, o que garante sua idempotência (pode ser executado várias vezes sem causar erro ou duplicidade de dados/estrutura se já foi aplicado).
### Quando o Liquibase é executado:
1. Ele verifica duas tabelas internas que cria no seu banco de dados: _DATABASECHANGELOG_ e _DATABASECHANGELOGLOCK_.
2. A tabela _DATABASECHANGELOG_ registra todos os changesets que já foram aplicados, usando a combinação de _id_, _author_ e _filepath_ para identificá-los.
3. A tabela _DATABASECHANGELOGLOCK_ garante que apenas uma instância do Liquibase esteja rodando por vez, evitando conflitos.
4. O Liquibase compara os changesets definidos no seu changelog com os changesets registrados na tabela _DATABASECHANGELOG_.
5. Ele aplica apenas os changesets que ainda não foram executados, na ordem definida no seu changelog.
## Exemplo Prático: Integrando Liquibase em um Projeto Spring Boot
Vamos ver como usar o Liquibase em um projeto Spring Boot com Maven.

### 1. Adicionar Dependências no _pom.xml_

Para usar Liquibase com Spring Boot e MySQL, adicione as seguintes dependências ao seu _pom.xml_:
```XML
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <scope>runtime</scope>
    </dependency>

    <dependency>
        <groupId>org.liquibase</groupId>
        <artifactId>liquibase-core</artifactId>
    </dependency>

    </dependencies>
```
### 2. Configurar o _application.properties_

Configure a conexão com o banco de dados e habilite o Liquibase no seu _src/main/resources/application.properties_:
```Properties
spring.datasource.url=jdbc:mysql://localhost:3306/board_db?createDatabaseIfNotExist=true
spring.datasource.username=root
spring.datasource.password=sua_senha
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Configuração do Liquibase
spring.liquibase.change-log=classpath:db/changelog/db.changelog-master.xml
spring.liquibase.enabled=true # Liquibase habilitado
# spring.liquibase.drop-first=true # CUIDADO: Usar apenas em DEV/TEST para recriar o DB
```
### 3. Criar o Diretório e o Master Changelog

Crie a estrutura de diretórios e o arquivo _db/changelog/db.changelog-master.xml_ dentro de _src/main/resources_:
```text
src/main/resources/
├── application.properties
└── db/
    └── changelog/
        └── db.changelog-master.xml
db.changelog-master.xml:
```
```XML
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
         http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <include file="db/changelog/changes/01-create-tasks-table.xml"/>
    <include file="db/changelog/changes/02-add-description-column.xml"/>
    <include file="db/changelog/changes/03-insert-initial-data.xml"/>

</databaseChangeLog>
```
### 4. Criar os Changesets

Crie a pasta _db/changelog/changes_ e seus respectivos arquivos XML (ou YAML, JSON, SQL):

_db/changelog/changes/01-create-tasks-table.xml_:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
         http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <changeSet id="01-create-tasks-table" author="giliano">
        <comment>Cria a tabela de tarefas</comment>
        <createTable tableName="tasks">
            <column name="id" type="BIGINT" autoIncrement="true">
                <constraints primaryKey="true" nullable="false"/>
            </column>
            <column name="title" type="VARCHAR(255)">
                <constraints nullable="false"/>
            </column>
            <column name="status" type="VARCHAR(50)">
                <constraints nullable="false"/>
            </column>
        </createTable>
    </changeSet>

</databaseChangeLog>
```
_db/changelog/changes/02-add-description-column.xml_:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
         http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <changeSet id="02-add-description-column" author="giliano">
        <comment>Adiciona a coluna de descrição na tabela tasks</comment>
        <addColumn tableName="tasks">
            <column name="description" type="TEXT"/>
        </addColumn>
    </changeSet>

</databaseChangeLog>
```
_db/changelog/changes/03-insert-initial-data.xml_:
```XML
<?xml version="1.0" encoding="UTF-8"?>
<databaseChangeLog
        xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog
         http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-latest.xsd">

    <changeSet id="03-insert-initial-data" author="giliano">
        <comment>Insere dados iniciais na tabela tasks</comment>
        <insert tableName="tasks">
            <column name="title" value="Comprar Leite"/>
            <column name="status" value="PENDING"/>
            <column name="description" value="Lembrar de comprar leite integral."/>
        </insert>
        <insert tableName="tasks">
            <column name="title" value="Pagar Contas"/>
            <column name="status" value="DONE"/>
            <column name="description" value="Pagar contas de água e luz."/>
        </insert>
    </changeSet>

</databaseChangeLog>
```
### 5. Executar a Aplicação Spring Boot

Quando você iniciar sua aplicação Spring Boot (por exemplo, via _mvn spring-boot:run_ ou executando a classe _main_), o Liquibase será executado automaticamente. Ele criará as tabelas _DATABASECHANGELOG_ e _DATABASECHANGELOGLOCK_, aplicará os changesets na ordem definida e registrará cada um deles.

Você verá logs indicando o progresso do Liquibase. Se tudo estiver correto, ao verificar seu banco de dados, você encontrará a tabela _tasks_ com as colunas e dados especificados, além das tabelas de controle do Liquibase.
## Considerações Finais
O Liquibase é uma ferramenta indispensável para qualquer projeto que envolva gerenciamento de banco de dados em ambientes colaborativos ou de CI/CD. Ele profissionaliza a forma como as mudanças de esquema são tratadas, reduzindo riscos e aumentando a produtividade da equipe. Comece a usá-lo e diga adeus aos problemas de migração de banco de dados!