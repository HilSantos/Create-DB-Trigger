-- Criação do banco de dados 'School' --
CREATE DATABASE IF NOT EXISTS School;
USE School;

-- Criação da tabela 'Disciplinas' (refatorada) --
CREATE TABLE Disciplinas (
    id_Disciplina INT AUTO_INCREMENT PRIMARY KEY, -- ID da disciplina
    nome_Disciplina VARCHAR(50) NOT NULL,          -- Nome da disciplina
    nome_Professor VARCHAR(50) NOT NULL             -- Nome do professor responsável
);

-- Inserção de disciplinas --
INSERT INTO Disciplinas (nome_Disciplina, nome_Professor) VALUES
('Biotecnologia', 'Prof. Silva'),
('Meio Ambiente e Sustentabilidade', 'Prof. Costa'),
('Energias Renováveis', 'Prof. Lima');

-- Criação da tabela 'Vendas' (relacionando com disciplina via 'id_Disciplina') --
CREATE TABLE Vendas (
    id_Venda INT AUTO_INCREMENT PRIMARY KEY,
    id_Disciplina INT,          -- Referência à disciplina vendida
    Produto VARCHAR(20),        -- Nome do produto
    Quantidade INT,             -- Quantidade vendida
    FOREIGN KEY (id_Disciplina) REFERENCES Disciplinas(id_Disciplina)
);

-- Inserção de vendas --
INSERT INTO Vendas (id_Disciplina, Produto, Quantidade) VALUES
(1, 'Kit Biotecnologia', 8),
(2, 'Eco Mochila', 11),
(3, 'Painel Solar', 20);

-- Visualizar vendas --
SELECT * FROM Vendas;

-- Mostrar triggers existentes --
SHOW TRIGGERS;

-- Caso queira criar triggers para atualizar alguma coluna na tabela 'Disciplinas', --
-- você precisa definir uma coluna, por exemplo, 'TotalVendas'. --
-- Exemplo de adição de coluna e trigger: --

ALTER TABLE Disciplinas ADD COLUMN TotalVendas INT DEFAULT 0;

-- Trigger após inserção em 'Vendas' --
DELIMITER $$
CREATE TRIGGER Tgr_Vendas_Insert
AFTER INSERT ON Vendas
FOR EACH ROW
BEGIN
    UPDATE Disciplinas
    SET TotalVendas = TotalVendas + NEW.Quantidade
    WHERE id_Disciplina = NEW.id_Disciplina;
END$$

-- Trigger após deletar de 'Vendas' --
CREATE TRIGGER Tgr_Vendas_Delete
AFTER DELETE ON Vendas
FOR EACH ROW
BEGIN
    UPDATE Disciplinas
    SET TotalVendas = TotalVendas - OLD.Quantidade
    WHERE id_Disciplina = OLD.id_Disciplina;
END$$
DELIMITER ;

-- Remover triggers quando necessário --
DROP TRIGGER Tgr_Vendas_Insert;
DROP TRIGGER Tgr_Vendas_Delete;
