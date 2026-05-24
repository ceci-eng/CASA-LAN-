# MODELO CONCEITUAL - CASA LAN MWC

![Modelo Conceitual CASA LAN MWC](modelo-conceitual.png)

## Descrição do Modelo

Este é um diagrama Entidade-Relacionamento (ER) para o sistema de gerenciamento de LAN "CASA LAN MWC".

### Entidades Principais:

1. **CLIENTES** (Clients)
   - Atributos: id_cliente, nome, cpf, email, telefone, data_cadastro
   - Relacionamento: "realiza" (conduz) sessões

2. **SESSÕES** (Sessions)
   - Atributos: id_sessao, inicio, fim, valor_total, status
   - Relacionamentos: 
     - "realiza" (1:N) com CLIENTES
     - "utilizados_em" (N:1) com COMPUTADORES
     - "possuem" (1:N) com CONSUMO

3. **COMPUTADORES** (Computers)
   - Atributos: id_computador, nome, especificacoes, status, preco_hora
   - Relacionamento: "utilizados_em" com SESSÕES

4. **CONSUMO** (Consumption/Items)
   - Atributos: id_consumo, quantidade, subtotal
   - Relacionamentos:
     - "possuem" (N:1) com SESSÕES
     - "participam_de" (N:1) com PRODUTOS

5. **PRODUTOS** (Products)
   - Atributos: id_produto, nome, categoria, preco, estoque, estoque_minimo
   - Relacionamento: "participam_de" com CONSUMO

6. **TORNEIOS** (Tournaments)
   - Atributos: id_torneio, nome, jogo, data_torneio, premio
   - Relacionamento: "possuem" (1:N) com INSCRIÇÕES

7. **INSCRIÇÕES** (Registrations)
   - Atributos: id_inscricao, data_inscricao
   - Relacionamentos:
     - "possuem" (N:1) com TORNEIOS
     - "realizam" (N:1) com CLIENTES

8. **AUDIT_LOG** (Audit Trail)
   - Atributos: id_log, tabela_afetada, acao, data_evento
   - Rastreia mudanças no sistema

### Relacionamentos:
- **1:N** - Um para muitos
- **N:1** - Muitos para um
- **N** - Muitos para muitos

O modelo suporta um **sistema de gerenciamento de LAN café/gaming center** com gerenciamento de sessões de clientes, aluguel de computadores, consumo de produtos (alimentos/bebidas) e gerenciamento de torneios.
