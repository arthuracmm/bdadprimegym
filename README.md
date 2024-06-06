# Prime Gym Database Project

Este repositório contém a modelagem e implementação de um banco de dados para a academia Prime Gym. Abaixo estão as seções detalhadas do projeto.

## 1. Cenário

Prime Gym é uma academia que deseja implementar um sistema de banco de dados para gerenciar suas operações diárias. O sistema deve rastrear membros, funcionários, planos de assinatura, equipamentos e aulas. Abaixo está a descrição detalhada das entidades, atributos e relacionamentos.

### Entidades e Atributos:

1. **Membros**
   - ID_Membro (chave primária)
   - Nome (atributo composto: PrimeiroNome, Sobrenome)
   - DataNascimento
   - Email
   - Telefone
   - Endereço (atributo composto: Rua, Cidade, Estado, CEP)
   - DataInscricao (atributo derivado a partir da data atual)
   - Plano_ID (chave estrangeira)

2. **Funcionários**
   - ID_Funcionario (chave primária)
   - Nome (atributo composto: PrimeiroNome, Sobrenome)
   - CPF (atributo chave)
   - Cargo
   - Salario
   - Telefone
   - Email
   - DataContratacao

3. **Planos**
   - ID_Plano (chave primária)
   - TipoPlano (mensal, trimestral, anual)
   - Preco
   - Descricao

4. **Equipamentos**
   - ID_Equipamento (chave primária)
   - NomeEquipamento
   - Quantidade (atributo multivalorado)
   - DataAquisicao

5. **Aulas**
   - ID_Aula (chave primária)
   - NomeAula
   - DiaSemana
   - Horario
   - ID_Professor (chave estrangeira para Funcionários)
   - Membro_ID (chave estrangeira para Membros)

### Relacionamentos:
- Cada membro pode se inscrever em muitos planos, mas cada plano pode ter muitos membros (N:N).
- Cada funcionário pode ser responsável por várias aulas, mas cada aula tem um único professor (1:N).
- Cada aula pode ter vários membros inscritos, e cada membro pode se inscrever em várias aulas (N:N).
- Equipamentos são utilizados nas aulas, mas cada aula pode usar vários equipamentos (N:N).

## 2. Modelagem Conceitual

![Modelagem Conceitual](imagens/der_prime_gym.png)

## 3. Modelagem Lógica

Aqui está a tradução do DER para o modelo lógico, especificando os tipos de dados esperados para cada atributo.

```sql
CREATE TABLE Membros (
    ID_Membro INT PRIMARY KEY AUTO_INCREMENT,
    PrimeiroNome VARCHAR(50),
    Sobrenome VARCHAR(50),
    DataNascimento DATE,
    Email VARCHAR(100),
    Telefone VARCHAR(20),
    Rua VARCHAR(100),
    Cidade VARCHAR(50),
    Estado VARCHAR(50),
    CEP VARCHAR(10),
    DataInscricao DATE DEFAULT CURRENT_DATE,
    Plano_ID INT,
    FOREIGN KEY (Plano_ID) REFERENCES Planos(ID_Plano)
);

CREATE TABLE Funcionários (
    ID_Funcionario INT PRIMARY KEY AUTO_INCREMENT,
    PrimeiroNome VARCHAR(50),
    Sobrenome VARCHAR(50),
    CPF VARCHAR(11) UNIQUE,
    Cargo VARCHAR(50),
    Salario DECIMAL(10, 2),
    Telefone VARCHAR(20),
    Email VARCHAR(100),
    DataContratacao DATE
);

CREATE TABLE Planos (
    ID_Plano INT PRIMARY KEY AUTO_INCREMENT,
    TipoPlano VARCHAR(20),
    Preco DECIMAL(10, 2),
    Descricao TEXT
);

CREATE TABLE Equipamentos (
    ID_Equipamento INT PRIMARY KEY AUTO_INCREMENT,
    NomeEquipamento VARCHAR(100),
    Quantidade INT,
    DataAquisicao DATE
);

CREATE TABLE Aulas (
    ID_Aula INT PRIMARY KEY AUTO_INCREMENT,
    NomeAula VARCHAR(100),
    DiaSemana VARCHAR(20),
    Horario TIME,
    ID_Professor INT,
    FOREIGN KEY (ID_Professor) REFERENCES Funcionários(ID_Funcionario)
);

CREATE TABLE Membro_Aula (
    ID_Membro INT,
    ID_Aula INT,
    PRIMARY KEY (ID_Membro, ID_Aula),
    FOREIGN KEY (ID_Membro) REFERENCES Membros(ID_Membro),
    FOREIGN KEY (ID_Aula) REFERENCES Aulas(ID_Aula)
);
```

## 4. Modelagem Física

A implementação das tabelas usando MySQL já foi mostrada na seção de Modelagem Lógica.

## 5. Inserção de Dados

Vamos inserir dados em todas as tabelas.

```sql
INSERT INTO Planos (TipoPlano, Preco, Descricao) VALUES 
('Mensal', 100.00, 'Plano mensal com acesso a todas as áreas'),
('Trimestral', 270.00, 'Plano trimestral com desconto'),
('Anual', 1000.00, 'Plano anual com maior desconto');

INSERT INTO Funcionários (PrimeiroNome, Sobrenome, CPF, Cargo, Salario, Telefone, Email, DataContratacao) VALUES
('João', 'Silva', '12345678900', 'Professor', 3000.00, '123456789', 'joao@primegym.com', '2023-01-01'),
('Maria', 'Oliveira', '09876543210', 'Recepcionista', 1500.00, '987654321', 'maria@primegym.com', '2023-02-01');

INSERT INTO Membros (PrimeiroNome, Sobrenome, DataNascimento, Email, Telefone, Rua, Cidade, Estado, CEP, Plano_ID) VALUES
('Carlos', 'Santos', '1990-05-20', 'carlos@gmail.com', '123123123', 'Rua A', 'Cidade A', 'Estado A', '12345-678', 1),
('Ana', 'Costa', '1985-10-15', 'ana@gmail.com', '321321321', 'Rua B', 'Cidade B', 'Estado B', '98765-432', 2);

INSERT INTO Equipamentos (NomeEquipamento, Quantidade, DataAquisicao) VALUES
('Esteira', 10, '2022-01-01'),
('Bicicleta', 5, '2022-02-01');

INSERT INTO Aulas (NomeAula, DiaSemana, Horario, ID_Professor) VALUES
('Yoga', 'Segunda-feira', '08:00:00', 1),
('Pilates', 'Quarta-feira', '10:00:00', 1);

INSERT INTO Membro_Aula (ID_Membro, ID_Aula) VALUES
(1, 1),
(2, 2);
```

## 6. CRUD

### Inserção de dados

```sql
INSERT INTO Membros (PrimeiroNome, Sobrenome, DataNascimento, Email, Telefone, Rua, Cidade, Estado, CEP, Plano_ID) VALUES
('Pedro', 'Almeida', '1992-07-22', 'pedro@gmail.com', '555555555', 'Rua C', 'Cidade C', 'Estado C', '11223-456', 3);
```
![Inserção de Dados](imagens/insercao_dados.png)

### Leitura de dados

```sql
SELECT * FROM Membros WHERE ID_Membro = 1;
```
![Leitura de Dados](imagens/leitura_dados.png)

### Atualização de dados

```sql
UPDATE Membros SET Email = 'novoemail@gmail.com' WHERE ID_Membro = 1;
```
![Atualização de Dados](imagens/atualizacao_dados.png)

### Deleção de dados

```sql
DELETE FROM Membros WHERE ID_Membro = 1;
```
![Deleção de Dados](imagens/delecao_dados.png)

## 7. Relatórios

### Relatório 1: Todos os membros e seus planos

```sql
SELECT Membros.PrimeiroNome, Membros.Sobrenome, Planos.TipoPlano 
FROM Membros 
JOIN Planos ON Membros.Plano_ID = Planos.ID_Plano;
```
![Relatório 1](imagens/relatorio_1.png)

### Relatório 2: Membros em uma determinada aula

```sql
SELECT Membros.PrimeiroNome, Membros.Sobrenome, Aulas.NomeAula 
FROM Membros 
JOIN Membro_Aula ON Membros.ID_Membro = Membro_Aula.ID_Membro 
JOIN Aulas ON Membro_Aula.ID_Aula = Aulas.ID_Aula 
WHERE Aulas.ID_Aula = 1;
```
![Relatório 2](imagens/relatorio_2.png)

### Relatório 3: Equipamentos adquiridos após uma data específica

```sql
SELECT * FROM Equipamentos WHERE DataAquisicao > '2022-01-01';
```
![Relatório 3](imagens/relatorio_3.png)

### Relatório 4: Funcionários e suas respectivas aulas

```sql
SELECT Funcionários.PrimeiroNome, Funcionários.Sobrenome, Aulas.NomeAula 
FROM Funcionários 
JOIN Aulas ON Funcionários.ID_Funcionario = Aulas.ID_Professor;
```
![Relatório 4](imagens/relatorio_4.png)

### Relatório 5: Planos mais caros que um determinado valor

```sql
SELECT * FROM Planos WHERE Preco > 200;
```
![Relatório 5

](imagens/relatorio_5.png)

### Relatório 6: Membros ordenados por data de inscrição

```sql
SELECT * FROM Membros ORDER BY DataInscricao DESC;
```
![Relatório 6](imagens/relatorio_6.png)

### Relatório 7: Aulas e a quantidade de membros inscritos

```sql
SELECT Aulas.NomeAula, COUNT(Membro_Aula.ID_Membro) AS QuantidadeMembros 
FROM Aulas 
JOIN Membro_Aula ON Aulas.ID_Aula = Membro_Aula.ID_Aula 
GROUP BY Aulas.ID_Aula;
```
![Relatório 7](imagens/relatorio_7.png)

### Relatório 8: Membros que têm aniversário em um determinado mês

```sql
SELECT PrimeiroNome, Sobrenome 
FROM Membros 
WHERE MONTH(DataNascimento) = 5;
```
![Relatório 8](imagens/relatorio_8.png)

### Relatório 9: Média salarial dos funcionários por cargo

```sql
SELECT Cargo, AVG(Salario) AS MediaSalarial 
FROM Funcionários 
GROUP BY Cargo;
```
![Relatório 9](imagens/relatorio_9.png)

### Relatório 10: Total de equipamentos disponíveis

```sql
SELECT NomeEquipamento, SUM(Quantidade) AS TotalQuantidade 
FROM Equipamentos 
GROUP BY NomeEquipamento;
```
![Relatório 10](imagens/relatorio_10.png)

---

### O que deverá ser entregue?

Este repositório contém:
- `prova.sql` com todo o código SQL desenvolvido.
- Pasta `imagens` contendo todos os prints e imagens utilizadas.
- `README.md` detalhado conforme as seções acima.

---

**Nota:** Os prints de todas as execuções SQL foram salvos na pasta `imagens` e referenciados no README para facilitar a visualização dos resultados.
```

Certifique-se de adicionar os prints mencionados no README na pasta `imagens` do seu repositório. Esse README fornece uma estrutura clara e detalhada, demonstrando o domínio dos conceitos de modelagem e SQL.
