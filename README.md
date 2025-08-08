# Sistema-de-Clientes-Contas-Pagamentos-e-Entregas
## Descrição do Projeto
Este projeto apresenta um modelo conceitual para gerenciamento de clientes, contas, pagamentos e entregas, visando atender tanto clientes Pessoa Jurídica (PJ) quanto Pessoa Física (PF).
### Contexto
- **Cliente**: Pode ser Pessoa Jurídica (PJ) ou Pessoa Física (PF), mas uma conta não pode estar associada a ambos simultaneamente, garantindo exclusividade.
- **Conta**: Relacionada a um único cliente (PJ ou PF).
- **Pagamento**: Cada conta pode possuir múltiplas formas de pagamento cadastradas.
- **Entrega**: Cada entrega possui um status e código de rastreio para acompanhamento.

### Objetivos

- Representar com clareza a distinção entre clientes PJ e PF.
- Permitir múltiplas formas de pagamento por conta.
- Controlar o status e rastreamento das entregas.
- Garantir integridade e exclusividade na modelagem.

## Modelo Conceitual

O modelo foi implementado considerando as seguintes entidades e relacionamentos:

- **Cliente** (superclasse abstrata)
  - **ClientePF** (subclasse)
    - Atributos: CPF, Nome, Data de Nascimento, etc.
  - **ClientePJ** (subclasse)
    - Atributos: CNPJ, Razão Social, Nome Fantasia, etc.
- **Conta**
  - Atributos: Número da Conta, Data de Abertura, Saldo, etc.
  - Relacionamento 1:1 com Cliente (apenas PF ou PJ)
- **Pagamento**
  - Atributos: Tipo de Pagamento (cartão, boleto, etc.), dados específicos do pagamento.
  - Relacionamento N:1 com Conta (uma conta pode ter várias formas de pagamento)
- **Entrega**
  - Atributos: Status (pendente, enviado, entregue), Código de Rastreamento.
  - Relacionamento N:1 com Conta (várias entregas por conta)


## Script SQL (Modelo Lógico simplificado)
CREATE TABLE ClientePF (
    CPF VARCHAR(11) PRIMARY KEY,
    Nome VARCHAR(100) NOT NULL,
    DataNascimento DATE NOT NULL
);

CREATE TABLE ClientePJ (
    CNPJ VARCHAR(14) PRIMARY KEY,
    RazaoSocial VARCHAR(150) NOT NULL,
    NomeFantasia VARCHAR(150)
);

CREATE TABLE Conta (
    NumConta INT PRIMARY KEY,
    ClientePF_CPF VARCHAR(11),
    ClientePJ_CNPJ VARCHAR(14),
    DataAbertura DATE NOT NULL,
    Saldo DECIMAL(15, 2) DEFAULT 0.00,
    CONSTRAINT fk_clientePF FOREIGN KEY (ClientePF_CPF) REFERENCES ClientePF(CPF),
    CONSTRAINT fk_clientePJ FOREIGN KEY (ClientePJ_CNPJ) REFERENCES ClientePJ(CNPJ),
    CONSTRAINT chk_cliente CHECK (
      (ClientePF_CPF IS NOT NULL AND ClientePJ_CNPJ IS NULL) OR
      (ClientePF_CPF IS NULL AND ClientePJ_CNPJ IS NOT NULL)
    )
);

CREATE TABLE Pagamento (
    IdPagamento INT PRIMARY KEY AUTO_INCREMENT,
    NumConta INT,
    TipoPagamento VARCHAR(50),
    DadosPagamento TEXT,
    CONSTRAINT fk_conta_pagamento FOREIGN KEY (NumConta) REFERENCES Conta(NumConta)
);

CREATE TABLE Entrega (
    IdEntrega INT PRIMARY KEY AUTO_INCREMENT,
    NumConta INT,
    Status VARCHAR(20),
    CodigoRastreamento VARCHAR(50),
    CONSTRAINT fk_conta_entrega FOREIGN KEY (NumConta) REFERENCES Conta(NumConta)
);

---

## Contato

Lucas Rodrigues - https://www.linkedin.com/in/lucas-rodrigues-43b219368/

---

