1. INTRODUÇÃO
Atualmente, as lan houses gamer continuam sendo ambientes bastante procurados por pessoas que desejam ter acesso a computadores de alto desempenho, jogos online e espaços voltados ao entretenimento digital. Além disso, muitas dessas empresas também realizam campeonatos, vendas de produtos e oferecem serviços para diferentes públicos.
Pensando nisso, foi desenvolvido um banco de dados para a CASA LAN MWC, com o objetivo de melhorar a organização das informações e facilitar o gerenciamento das atividades realizadas diariamente na empresa.
O sistema foi elaborado utilizando conceitos estudados na disciplina de Banco de Dados, envolvendo modelagem conceitual, modelagem lógica e implementação física em MySQL.
A proposta do projeto é centralizar informações importantes, como cadastro de clientes, controle dos computadores disponíveis, sessões de uso, produtos consumidos e torneios realizados pela lan house.
 
2. OBJETIVO DO PROJETO
O principal objetivo deste projeto é desenvolver um banco de dados funcional para auxiliar no gerenciamento da CASA LAN MWC.
Com a criação do sistema, torna-se possível organizar melhor as informações da empresa, evitando perda de dados e facilitando processos internos.
Além disso, o projeto busca aplicar na prática os conhecimentos adquiridos em sala de aula sobre estruturação de tabelas, relacionamentos entre entidades, chaves primárias, chaves estrangeiras e comandos SQL.
 
3. REGRAS DE NEGÓCIO
* Um cliente pode possuir várias sessões.
* Cada sessão pertence a apenas um cliente.
* Um computador pode ser utilizado em várias sessões.
* Produtos podem ser consumidos durante uma sessão.
* Um cliente pode participar de vários torneios.
* O sistema deve armazenar registros de auditoria.
* O status dos computadores deve ser controlado entre “livre” e “ocupado”.
* O status das sessões deve ser controlado entre “aberta” e “encerrada”.
 
4. MODELO CONCEITUAL

 
 
 
5. MODELO LÓGICO

 
6. DICIONÁRIO DE DADOS
Tabela
 
Descrição
Clientes
 
Armazena os dados dos clientes
Computadores
 
Controla os computadores disponíveis
Sessões
 
Registra as sessões de utilização
Produtos
 
Armazena os produtos vendidos
Consumo
 
Registra o consumo realizado durante as sessões
Torneios
 
Armazena os torneios cadastrados
Inscrições
 
Registra clientes inscritos nos torneios
Audit_log
 
Armazena registros de auditoria e histórico de ações realizadas no sistema
 
7. DDL – CRIAÇÃO DAS TABELAS
CREATE TABLE clientes (
   id_cliente INT AUTO_INCREMENT PRIMARY KEY,
   nome VARCHAR(100) NOT NULL,
   cpf VARCHAR(14) NOT NULL UNIQUE,
   email VARCHAR(100) NOT NULL UNIQUE,
   telefone VARCHAR(20),
   data_cadastro DATETIME DEFAULT CURRENT_TIMESTAMP
);
 
CREATE TABLE computadores (
   id_computador INT AUTO_INCREMENT PRIMARY KEY,
   nome VARCHAR(50) NOT NULL,
   especificacoes VARCHAR(255),
   status VARCHAR(10) DEFAULT 'livre',
   preco_hora DECIMAL(6,2),
   CHECK (status IN ('livre', 'ocupado'))
);
 
CREATE TABLE sessoes (
   id_sessao INT AUTO_INCREMENT PRIMARY KEY,
   id_cliente INT NOT NULL,
   id_computador INT NOT NULL,
   inicio DATETIME DEFAULT CURRENT_TIMESTAMP,
   fim DATETIME,
   valor_total DECIMAL(8,2),
   status VARCHAR(10) DEFAULT 'aberta',
 
   CHECK (status IN ('aberta', 'encerrada')),
 
   FOREIGN KEY (id_cliente)
   REFERENCES clientes(id_cliente),
 
   FOREIGN KEY (id_computador)
   REFERENCES computadores(id_computador)
);
 
CREATE TABLE produtos (
   id_produto INT AUTO_INCREMENT PRIMARY KEY,
   nome VARCHAR(100),
   categoria VARCHAR(50),
   preco DECIMAL(6,2),
   estoque INT,
   estoque_minimo INT
);
 
CREATE TABLE consumo (
   id_consumo INT AUTO_INCREMENT PRIMARY KEY,
   id_sessao INT,
   id_produto INT,
   quantidade INT,
   subtotal DECIMAL(8,2),
 
   FOREIGN KEY (id_sessao)
   REFERENCES sessoes(id_sessao),
 
   FOREIGN KEY (id_produto)
   REFERENCES produtos(id_produto)
);
 
CREATE TABLE torneios (
   id_torneio INT AUTO_INCREMENT PRIMARY KEY,
   nome VARCHAR(100),
   jogo VARCHAR(100),
   data_torneio DATE,
   premio DECIMAL(10,2)
);
CREATE TABLE inscricoes (
   id_inscricao INT AUTO_INCREMENT PRIMARY KEY,
   id_cliente INT,
   id_torneio INT,
   data_inscricao DATETIME DEFAULT CURRENT_TIMESTAMP,
 
   FOREIGN KEY (id_cliente)
   REFERENCES clientes(id_cliente),
 
   FOREIGN KEY (id_torneio)
   REFERENCES torneios(id_torneio)
);
 
CREATE TABLE audit_log (
   id_log INT AUTO_INCREMENT PRIMARY KEY,
   tabela_afetada VARCHAR(50),
   acao VARCHAR(20),
   data_evento DATETIME DEFAULT CURRENT_TIMESTAMP
);
 
A tabela audit_log foi criada com a finalidade de registrar alterações realizadas no sistema, permitindo maior controle e acompanhamento das operações executadas no banco de dados.
 
8. DML – INSERÇÃO DE DADOS
INSERT INTO clientes (nome, cpf, email, telefone) VALUES
('Ana Beatriz Lima','101.111.111-01','ana@gmail.com','81990000001'),
('Carlos Eduardo Silva','101.111.111-02','carlos@gmail.com','81990000002'),
('Fernanda Rocha','101.111.111-03','fernanda@gmail.com','81990000003'),
('Gabriel Souza','101.111.111-04','gabriel@gmail.com','81990000004'),
('Helena Costa','101.111.111-05','helena@gmail.com','81990000005'),
('Igor Mendes','101.111.111-06','igor@gmail.com','81990000006'),
('Julia Ferreira','101.111.111-07','julia@gmail.com','81990000007'),
('Kaique Rodrigues','101.111.111-08','kaique@gmail.com','81990000008'),
('Larissa Monteiro','101.111.111-09','larissa@gmail.com','81990000009'),
('Marcos Vinicius','101.111.111-10','marcos@gmail.com','81990000010'),
('Natalia Costa','101.111.111-11','natalia@gmail.com','81990000011'),
('Otavio Santana','101.111.111-12','otavio@gmail.com','81990000012'),
('Pedro Henrique','101.111.111-13','pedro@gmail.com','81990000013'),
('Rafaela Gomes','101.111.111-14','rafaela@gmail.com','81990000014'),
('Thiago Almeida','101.111.111-15','thiago@gmail.com','81990000015');
 
INSERT INTO computadores (nome, especificacoes, status, preco_hora) VALUES
('MWC-ALPHA','i5 RTX 3060 16GB','livre',5.00),
('MWC-BLACK','i5 RTX 3060 16GB','livre',5.00),
('MWC-GHOST','Ryzen 7 RTX 4070 32GB','livre',7.50),
('MWC-DRAGON','Ryzen 7 RTX 4070 32GB','ocupado',7.50),
('MWC-TITAN','i7 RTX 4080 32GB','livre',10.00),
('MWC-VORTEX','i7 RTX 4080 32GB','livre',10.00),
('MWC-FLASH','GTX 1660 16GB','livre',4.00),
('MWC-STORM','GTX 1660 16GB','ocupado',4.00),
('MWC-LEGEND','RX 7600 16GB','livre',5.50),
('MWC-PHOENIX','RX 7600 16GB','livre',5.50),
('MWC-IMMORTAL','RTX 4090 64GB','livre',15.00),
('MWC-NEXUS','RTX 4090 64GB','livre',15.00),
('MWC-SHADOW','RTX 4080 32GB','ocupado',12.00),
('MWC-ELITE','RTX 4080 32GB','livre',12.00),
('MWC-STREAM','Streamer Setup RTX 4070','livre',9.00);
 
INSERT INTO produtos (nome, categoria, preco, estoque, estoque_minimo) VALUES
('Combo MWC Gamer','Lanche',25.00,20,5),
('Energetico Nitro MWC','Energetico',15.00,25,5),
('Pizza XP Boost','Lanche',30.00,10,2),
('Hamburguer Boss Level','Lanche',22.00,15,3),
('Coca-Cola 350ml','Bebida',5.00,50,10),
('Agua Mineral','Bebida',3.00,80,20),
('Monster Energy','Energetico',12.00,25,5),
('Pringles Original','Snack',9.50,30,5),
('Doritos Queijo','Snack',7.00,35,5),
('Chocolate Lacta','Doce',6.00,30,5),
('Mouse Gamer RGB','Acessorio',120.00,10,2),
('Headset Pro MWC','Acessorio',250.00,8,2),
('Mousepad XL','Acessorio',60.00,12,2),
('Batata Frita','Lanche',14.00,20,5),
('Cafe Expresso','Bebida',4.00,40,10);
 
INSERT INTO torneios
(nome, jogo, data_torneio, premio) VALUES
('Campeonato CS2', 'Counter Strike 2', '2026-06-10', 1000.00),
('Liga Valorant', 'Valorant', '2026-07-15', 1500.00),
('MWC League of Legends', 'League of Legends', '2026-08-20', 2000.00),
('FIFA Ultimate Cup', 'FIFA 26', '2026-09-05', 1200.00),
('Free Fire Arena', 'Free Fire', '2026-10-12', 800.00);
 
INSERT INTO sessoes
(id_cliente, id_computador, inicio, fim, valor_total, status) VALUES
 
(1, 1, '2026-05-10 13:00:00', '2026-05-10 15:00:00', 10.00, 'encerrada'),
(2, 3, '2026-05-10 14:00:00', '2026-05-10 17:00:00', 22.50, 'encerrada'),
(3, 5, '2026-05-10 15:30:00', '2026-05-10 18:30:00', 30.00, 'encerrada'),
(4, 2, '2026-05-11 09:00:00', NULL, NULL, 'aberta'),
(5, 7, '2026-05-11 10:00:00', '2026-05-11 12:00:00', 8.00, 'encerrada');
 
INSERT INTO audit_log
(tabela_afetada, acao, data_evento) VALUES
 
('clientes', 'INSERT', '2026-05-10 09:00:00'),
('clientes', 'UPDATE', '2026-05-10 09:10:00'),
('computadores', 'INSERT', '2026-05-10 09:20:00'),
('sessoes', 'INSERT', '2026-05-10 10:00:00'),
('sessoes', 'UPDATE', '2026-05-10 12:00:00'),
('produtos', 'INSERT', '2026-05-10 13:00:00'),
('consumo', 'INSERT', '2026-05-10 13:20:00'),
('torneios', 'INSERT', '2026-05-10 14:00:00'),
('inscricoes', 'INSERT', '2026-05-10 15:00:00'),
('clientes', 'DELETE', '2026-05-10 16:00:00'),
('produtos', 'UPDATE', '2026-05-10 17:00:00'),
('computadores', 'UPDATE', '2026-05-10 18:00:00'),
('sessoes', 'DELETE', '2026-05-10 19:00:00'),
('audit_log', 'INSERT', '2026-05-10 20:00:00'),
('inscricoes', 'DELETE', '2026-05-10 21:00:00');
 
Os registros acima representam exemplos de auditoria do sistema, permitindo acompanhar ações realizadas dentro do banco de dados.
