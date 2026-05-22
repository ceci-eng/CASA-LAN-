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
 

