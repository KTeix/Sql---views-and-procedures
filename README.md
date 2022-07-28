# Sql views and procedures
Usando views e procedures


CREATE DATABASE exemplojoin;
USE exemplojoin;
CREATE TABLE tab_funcionarios(
	nome VARCHAR(50) NOT NULL,
	codigo INT NOT NULL
);
CREATE TABLE tab_profissoes(
nome VARCHAR(50) NOT NULL,
	codigo INT NOT NULL
);

INSERT INTO tab_funcionarios(nome, codigo)
VALUES
('João', 1), ('Maria', 2),
('Kátia', 3), ('Sérgio', 4);

INSERT INTO tab_profissoes(nome, codigo)
VALUES
('Programador', 1), ('Designer', 2),
('Analista', 3), ('DBA', 5);

SELECT * FROM tab_funcionarios;
SELECT * FROM tab_profissoes;

-- INNER JOIN
-- Retorna dados apenas quando as duas tabelas tem 
-- chaves correspondentes na cláusula ON do join
SELECT tab_funcionarios.*,
	   tab_profissoes.*
FROM tab_funcionarios
INNER JOIN tab_profissoes
ON tab_funcionarios.codigo = tab_profissoes.codigo;



-- LEFT JOIN
-- Retorna a TabelaA inteira e apenas os registros 
-- que coincidirem com a igualdade do join na TabelaB 
-- (ou campos nulos para os campos sem 
-- correspondência).
SELECT tab_funcionarios.*,
	   tab_profissoes.*
FROM tab_funcionarios
LEFT JOIN tab_profissoes
ON tab_funcionarios.codigo = tab_profissoes.codigo;

-- RIGHT JOIN
-- Segue o mesmo raciocínio do Left Join, mas se 
-- aplicando à TabelaB em vez da TabelaA.
SELECT tab_funcionarios.*,
	   tab_profissoes.*
FROM tab_funcionarios
RIGHT JOIN tab_profissoes
ON tab_funcionarios.codigo = tab_profissoes.codigo;

-- FULL OUTER JOIN (SQL Server) 
-- Retorna todos os registros de ambas as tabelas.
SELECT tab_funcionarios.*, 
	   tab_profissoes.* 
FROM tab_funcionarios
FULL OUTER JOIN tab_profissoes
ON tab_funcionarios.codigo = tab_profissoes.codigo;


-- CROSS JOIN
-- Basicamente é o produto cartesiano entre as duas 
-- tabelas. 
-- Para cada linha de TabelaA, são retornadas todas as 
-- linhas da TabelaB.
SELECT tab_funcionarios.*, tab_profissoes.* 
FROM tab_funcionarios
CROSS JOIN tab_profissoes;





--**********************************




-- CRIANDO VIEWS
-- A view pode ser definida como uma tabela virtual 
-- composta por linhas e colunas de dados vindos de 
-- tabelas relacionadas em uma query (um agrupamento 
-- de SELECT’s, por exemplo). As linhas e colunas da 
-- view são geradas dinamicamente no momento em que 
-- é feita uma referência a ela.
-- As views ficam armazenads em:

-- Seu Banco de Dados
-- Exibições

USE escola;
-- Criando uma view para mostrar o id, nome, 
-- email e fone dos alunos
CREATE VIEW vwAlunos
AS
SELECT id_aluno AS 'Código',
	   nome AS Nome,
	   email AS [E-mail],
	   fone AS Celular
FROM tab_alunos;

-- Executando a view
SELECT * FROM vwAlunos;
SELECT Nome, Celular FROM vwAlunos;

-- Removendo uma view
DROP VIEW vwAlunos;

-- Alterando uma view
ALTER VIEW vwAlunos
AS
SELECT id_aluno AS 'Código',
	   nome AS Nome,
	   email AS [E-mail],
	   fone AS Celular
FROM tab_alunos
WHERE email LIKE '%gmail.com';


-- Criar uma view para mostrar o nome da turma, 
-- do aluno, número da sala e nome do professor.
-- O nome da view será vwTurmas.
CREATE VIEW vwTurmas
AS
SELECT tab_turmas.nome AS 'Turma',
	   tab_alunos.nome AS 'Aluno',
	   tab_salas.numero AS 'Sala',
	   tab_professores.nome AS 'Professor'
FROM tab_turmas
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
INNER JOIN tab_salas
ON tab_turmas.id_sala = tab_salas.id_sala
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor;

-- Executando a view
SELECT * FROM vwTurmas;
SELECT * FROM vwTurmas WHERE Sala = 2;
SELECT Turma, Professor FROM vwTurmas;

-- Exercício
-- Criar uma view para mostrar o nome do professor 
-- e dos alunos de cada turma
CREATE VIEW vwTurmaSQL AS
SELECT tab_professores.nome AS Professor,
	   tab_alunos.nome as Aluno,
	   tab_turmas.nome AS Turma
FROM tab_turmas
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
WHERE tab_turmas.nome = 'SQL';

CREATE VIEW vwTurmaCSHARP AS
SELECT tab_professores.nome AS Professor,
	   tab_alunos.nome as Aluno,
	   tab_turmas.nome AS Turma
FROM tab_turmas
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
WHERE tab_turmas.nome = 'C#';

CREATE VIEW vwTurmaEXCEL AS
SELECT tab_professores.nome AS Professor,
	   tab_alunos.nome as Aluno,
	   tab_turmas.nome AS Turma
FROM tab_turmas
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
WHERE tab_turmas.nome = 'EXCEL';

CREATE VIEW vwTurmaREDES AS
SELECT tab_professores.nome AS Professor,
	   tab_alunos.nome as Aluno,
	   tab_turmas.nome AS Turma
FROM tab_turmas
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
WHERE tab_turmas.nome = 'REDES';

SELECT * FROM vwTurmaSQL;
SELECT * FROM vwTurmaCSHARP;
SELECT * FROM vwTurmaEXCEL;
SELECT * FROM vwTurmaREDES;

-- Criar uma view para mostrar o nome do professor Fábio
-- e sua turma
CREATE VIEW vwMostraTurmaFabio AS
SELECT tab_alunos.nome AS Aluno,
	   tab_professores.nome AS Professor,
	   tab_turmas.nome AS Turma
FROM tab_turmas
INNER JOIN tab_alunos
ON tab_turmas.id_aluno = tab_alunos.id_aluno
INNER JOIN tab_professores
ON tab_turmas.id_professor = tab_professores.id_professor
WHERE tab_professores.nome = 'Fábio';

SELECT * FROM vwMostraTurmaFabio;



--**************************************

-- Criando Stored Procedure
-- Stored Procedure, que traduzido significa Procedimento 
-- Armazenado, é um conjunto de comandos em SQL que podem 
-- ser executados de uma só vez, como em uma função. 
-- Ele armazena tarefas repetitivas e aceita parâmetros 
-- de entrada para que a tarefa seja efetuada de acordo 
-- com a necessidade individual.

-- As Stored Procedure ficam armazenadas em:

-- Seu Banco de Dados
-- Programação
-- Procedimentos armazenados

-- Criando uma Stored Procedure para buscar um email de aluno
-- Declarando a procedure
CREATE PROCEDURE spBuscaEmailAluno
-- Declarando uma variável - use o @ antes do nome da variável
@busca VARCHAR(50)
AS
-- Consulta
SELECT * FROM tab_alunos
-- Utilizando a vaiável como filtro da pesquisa
WHERE email LIKE @busca;

-- Executando a SP
EXECUTE spBuscaEmailAluno '%gmail%';
EXECUTE spBuscaEmailAluno 'fatima@gmail.com';

-- Excluindo uma SP
DROP PROCEDURE spBuscaEmailAluno;

-- Criar uma SP para mostrar fone e email de todos os alunos
CREATE PROCEDURE spMostraFoneEmailAlunos
AS
SELECT fone, email FROM tab_alunos;

EXECUTE spMostraFoneEmailAlunos;
