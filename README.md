# Atividade-Cart-o
(UNINOVE)

-- 
-- =========================================================
--  TABELA CLIENTE 
--
-- ========================================================
CREATE TABLE Cliente (                      -- Cria a tabela chamada "Cliente"
    id_cliente INT PRIMARY KEY AUTO_INCREMENT, -- Identificador único; cresce automaticamente a cada novo cliente
    nome VARCHAR(100) NOT NULL,               -- Nome completo do cliente (campo obrigatório)
    email VARCHAR(100) UNIQUE NOT NULL,       -- E-mail único; não pode repetir
    telefone VARCHAR(20),                     -- Telefone do cl
-- iente (campo opcional)
    data_cadastro DATE DEFAULT CURRENT_DATE   -- Data em que o cliente foi cadastrado; por padrão, a data de hoje
);

-- ========================================================

--     TABELA CARTAO 

--- =======================================================

CREATE TABLE Cartao(         ---> Cria a tabela "cartão"
id_cartao INT PRIMARY KEY AUTO_INCREMENT,    --> Identificador único do cartão
id_cliente INT NOT NULL,   -- Chave estrangeira que liga o cartão a um cliente
 numero_cartao VARCHAR(20) UNIQUE NOT NULL, -- Número do cartão (único e obrigatório)
    validade DATE NOT NULL,                  -- Data de validade do cartão
    bandeira VARCHAR(50),                    -- Bandeira do cartão (Visa, MasterCard, etc.)
    limite_credito DECIMAL(10,2),            -- Limite de crédito disponível no cartão
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente) -- Liga o cartão a um cliente existente
);

-- =====================================================
-- TABELA FATURA
-- =====================================================
CREATE TABLE Fatura (                        -- Cria a tabela "Fatura"
    id_fatura INT PRIMARY KEY AUTO_INCREMENT, -- Identificador único da fatura
    id_cliente INT NOT NULL,                 -- Chave estrangeira que liga a fatura ao cliente
    data_inicio DATE NOT NULL,               -- Data inicial do período da fatura
    data_fim DATE NOT NULL,                  -- Data final do período da fatura
    valor_total DECIMAL(10,2) DEFAULT 0.00,  -- Valor total das compras da fatura; começa com 0
    situacao VARCHAR(20) DEFAULT 'Aberta',   -- Situação atual da fatura (Aberta, Paga, etc.)
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente) -- Liga a fatura ao cliente dono dela
);

-- =====================================================
-- TABELA METODO_PAGAMENTO
-- =====================================================
CREATE TABLE Metodo_Pagamento (              -- Cria a tabela "Metodo_Pagamento"
    id_metodo INT PRIMARY KEY AUTO_INCREMENT, -- Identificador único do método de pagamento
    descricao VARCHAR(50) NOT NULL           -- Nome do método (ex: Crédito, Débito, PIX)
);

-- =====================================================
-- TABELA TRANSACAO
-- =====================================================
CREATE TABLE Transacao (                     -- Cria a tabela "Transacao"
    id_transacao INT PRIMARY KEY AUTO_INCREMENT, -- Identificador único da transação
    id_cliente INT NOT NULL,                 -- FK: Cliente que realizou a transação
    id_cartao INT NOT NULL,                  -- FK: Cartão usado na transação
    id_metodo INT NOT NULL,                  -- FK: Método de pagamento (Crédito, Débito etc.)
    id_fatura INT,                           -- FK: Fatura onde a transação será registrada
    valor DECIMAL(10,2) NOT NULL,            -- Valor monetário da transação
    data_transacao DATETIME DEFAULT CURRENT_TIMESTAMP, -- Data e hora em que ocorreu
    situacao VARCHAR(20) DEFAULT 'Concluída', -- Estado atual da transação
    FOREIGN KEY (id_cliente) REFERENCES Cliente(id_cliente), -- Conecta com o cliente
    FOREIGN KEY (id_cartao) REFERENCES Cartao(id_cartao),    -- Conecta com o cartão
    FOREIGN KEY (id_metodo) REFERENCES Metodo_Pagamento(id_metodo), -- Conecta com o método de pagamento
    FOREIGN KEY (id_fatura) REFERENCES Fatura(id_fatura)     -- Conecta com a fatura
);
