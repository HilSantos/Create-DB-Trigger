# Create-DB-Trigger

CREATE DATABASE School(

use School;

CREATE TABLE Disciplinas(

id_Curso int AUTO_INCREMENT PRIMARY KEY,

name_Curso VARCHAR(50) not null,

name_teacher VARCHAR(50) not null,

);

select * from Cursos;

insert into Disciplinas(materia) values(001, Biotecnologia), (002, Meio Ambiente e Sustentabilidade), (003, Energias Renovaveis);

CREATE TABLE Vendas(

Venda INT,
  
Produto VARCHAR(20),
  
Quantidade INT
  
);

DELIMITER $

CREATE TRIGGER Tgr_Vendas_Insert AFTER INSERT

ON Vendas

FOR EACH ROW

BEGIN

UPDATE Disciplinas SET Portfolio = Portfolio - NEW.Quantidade

WHERE Referencia = NEW.Curso;
END$

CREATE TRIGGER Tgr_Vendas_Delete AFTER DELETE

ON Vendas

FOR EACH ROW

BEGIN

UPDATE Disciplinas SET Portfolio = Portfolio + OLD.Quantidade

WHERE Referencia = OLD.Curso;
END$

DELIMITER ;

insert into Venda VALUES (1,'001', 8);
insert into Venda VALUES (1,'002', 11);
insert into Venda VALUES (1,'003', 20);

select * from Vendas;

SHOW TRIGGERS;

DROP TRIGGER Tgr_Vendas_Insert;
