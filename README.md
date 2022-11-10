# uvv_bd_1_cc1m
#Pset 1
## Nome: Guilherme Souza Oliveira
## Professor: Abrantes
## Turma: CC1M


###Infelizmente, ontem durante a aula quando eu estava tirando uma dúvida, minha máquina virtual acabou dando problema e apagando todos os meus arquivos e meu progresso no pset. O professor tentou resolver, mas não tinha mais jeito e eu não tinha salvado nada no github ainda. Aqui está a única parte que consegui salvar, pois tinha salvado esse código antes.

### Aqui está o código gerado a partir do esquema "hr" que fiz no SQL Power Architect:

CREATE TABLE cargos (
                id_cargo VARCHAR(10) NOT NULL,
                cargo VARCHAR(35) NOT NULL,
                salario_minimo NUMERIC(8,2),
                salario_maximo NUMERIC(8,2),
                CONSTRAINT cargos_pk PRIMARY KEY (id_cargo)
);
COMMENT ON TABLE cargos IS 'Tabela que contém a faixa salarial (salário máximo e mínimo)de cada cargo e os cargos que existem.';
COMMENT ON COLUMN cargos.id_cargo IS 'Chave primária dessa tabela.';
COMMENT ON COLUMN cargos.cargo IS 'Nome do cargo.';
COMMENT ON COLUMN cargos.salario_minimo IS 'Menor salário possível para um cargo.';
COMMENT ON COLUMN cargos.salario_maximo IS 'Maior salário possível para um cargo.';


CREATE UNIQUE INDEX cargos_idx
 ON cargos
 ( cargo );

CREATE TABLE regioes (
                id_regiao INTEGER NOT NULL,
                nome VARCHAR(25) NOT NULL,
                CONSTRAINT regioes_pk_ PRIMARY KEY (id_regiao)
);
COMMENT ON TABLE regioes IS 'Nesta tabela estão descritos os nomes das regiões e seus respectivos ids.';
COMMENT ON COLUMN regioes.id_regiao IS 'código das regiões para especificar cada uma delas. É a chave primária da tabela regioes.';
COMMENT ON COLUMN regioes.nome IS 'Aqui estão os nomes das regiões.';


CREATE UNIQUE INDEX regioes_idx
 ON regioes
 ( nome );

CREATE TABLE paises (
                id_pais CHAR(2) NOT NULL,
                nome VARCHAR(50) NOT NULL,
                id_regiao INTEGER NOT NULL,
                CONSTRAINT paises_pk_ PRIMARY KEY (id_pais)
);
COMMENT ON TABLE paises IS 'Nesta tabela estão as informações dos países';
COMMENT ON COLUMN paises.id_pais IS 'aqui estão os códigos dos paises. É a chave primária desta tabela.';
COMMENT ON COLUMN paises.nome IS 'aqui estão os nomes dos paises.';
COMMENT ON COLUMN paises.id_regiao IS 'Aqui estão os códigos das regiões. Chave estrangeira para a tabela regioes.';


CREATE UNIQUE INDEX paises_idx
 ON paises
 ( nome );

CREATE TABLE localizacoes (
                id_localizacao INTEGER NOT NULL,
                endereco VARCHAR(50),
                cep VARCHAR(12),
                cidade VARCHAR(50),
                uf VARCHAR(25),
                id_pais CHAR(2) NOT NULL,
                CONSTRAINT localizacoes_pk PRIMARY KEY (id_localizacao)
);
COMMENT ON TABLE localizacoes IS 'Nesta tabela estão as localizações de vários escritórios da empresa';
COMMENT ON COLUMN localizacoes.id_localizacao IS 'Aqui estão os códigos da localizações.';
COMMENT ON COLUMN localizacoes.endereco IS 'Aqui estão os endereços.';
COMMENT ON COLUMN localizacoes.cep IS 'Aqui estão os CEP (código postal).';
COMMENT ON COLUMN localizacoes.cidade IS 'Aqui estão as cidades onde estão localizadas os escritórios ou facilidades da empresa.';
COMMENT ON COLUMN localizacoes.uf IS 'Estados onde estão localizados os escritórios ou facilidades da empresa.';
COMMENT ON COLUMN localizacoes.id_pais IS 'Chave estrangeira da tabela paises.';


CREATE TABLE departamentos (
                id_departamento INTEGER NOT NULL,
                nome VARCHAR(50),
                id_localizacao INTEGER NOT NULL,
                id_gerente INTEGER NOT NULL,
                CONSTRAINT departamentos_pk PRIMARY KEY (id_departamento)
);
COMMENT ON TABLE departamentos IS 'Nesta tabela esão dispostas informações sobre os departamentos da empresa';
COMMENT ON COLUMN departamentos.id_departamento IS 'códigos dos departamentos. É a chave primária desta tabela.';
COMMENT ON COLUMN departamentos.nome IS 'Nomes dos departamentos da empresa.';
COMMENT ON COLUMN departamentos.id_localizacao IS 'Esta é a chave estrangeira para a tabela localizacoes.';
COMMENT ON COLUMN departamentos.id_gerente IS 'Chave estrangeira para a tabela empregados, e irá representar o gerente, se houver algum.';


CREATE UNIQUE INDEX departamentos_idx
 ON departamentos
 ( nome );

CREATE TABLE empregados (
                id_empregado INTEGER NOT NULL,
                nome VARCHAR(75) NOT NULL,
                email VARCHAR(35) NOT NULL,
                telefone VARCHAR(20),
                data_contratacao DATE NOT NULL,
                id_cargo VARCHAR(10) NOT NULL,
                comissao NUMERIC(4,2),
                salario NUMERIC(8,2),
                id_departamento INTEGER NOT NULL,
                id_supervisor INTEGER NOT NULL,
                CONSTRAINT empregados_pk PRIMARY KEY (id_empregado)
);
COMMENT ON TABLE empregados IS 'Nesta tabela estão dispostas as informações dos empregados dessa empresa.';
COMMENT ON COLUMN empregados.id_empregado IS 'Código dos empregados. Chave primária desta tabela.';
COMMENT ON COLUMN empregados.nome IS 'Aqui estão os nomes dos empregados.';
COMMENT ON COLUMN empregados.email IS 'Aqui estão as partes iniciais dos emails (antes do @) dos empregados.';
COMMENT ON COLUMN empregados.telefone IS 'Aqui estão os números de telefone dos empregados.';
COMMENT ON COLUMN empregados.data_contratacao IS 'Aqui está a data que o empregado iniciou no seu cargo atual.';
COMMENT ON COLUMN empregados.id_cargo IS 'Está é uma chave estrangeira para a tabela cargos. indica o código do cargo atual do empregado.';
COMMENT ON COLUMN empregados.comissao IS 'Aqui está a porcentagem de comissão de um empregado, caso ele trabalhe no setor de vendas.';
COMMENT ON COLUMN empregados.salario IS 'Atual salário que o empregado recebe por mês.';
COMMENT ON COLUMN empregados.id_departamento IS 'Indica o atual departamento de um empregado. É uma chave estrangeira para a tabela departamentos.';
COMMENT ON COLUMN empregados.id_supervisor IS 'É um auto-relacionamento, ou seja, é uma chave estrangeira para a própria tabela empregados.  indica o supervisor direto do empregado.';


CREATE UNIQUE INDEX empregados_idx
 ON empregados
 ( email );

CREATE TABLE historico_cargos (
                id_empregado INTEGER NOT NULL,
                data_inicial DATE NOT NULL,
                data_final DATE NOT NULL,
                id_cargo VARCHAR(10) NOT NULL,
                id_departamento INTEGER NOT NULL,
                CONSTRAINT historico_cargos_pk PRIMARY KEY (id_empregado, data_inicial)
);
COMMENT ON TABLE historico_cargos IS 'Tabela que contém as informações do histórico de um empregado. Ela deve ser atualizada a medida que um empregado muda de departamento dentro de um cargo ou  de um cargo dentro de um departamento, adicionando novas linhas com a informação antiga do empregado';
COMMENT ON COLUMN historico_cargos.id_empregado IS 'É uma parte da chave primária composta da tabela. Também é uma chave estrangeira para a tabela empregados. indica o código do empregado.';
COMMENT ON COLUMN historico_cargos.data_inicial IS 'É uma parte da chave primária composta da tabela. Indica a data inicial do empregado em um cargo novo e deve ser menor que a data _final.';
COMMENT ON COLUMN historico_cargos.data_final IS 'Último dia que o empregado ficou nesse cargo. Deve ser maior que a data_inicial.';
COMMENT ON COLUMN historico_cargos.id_cargo IS 'É uma chave estrangeira para a tabela cargos e corresponde ao cargo que o empregado estava trabalhando no passado.';
COMMENT ON COLUMN historico_cargos.id_departamento IS 'É uma chave estrangeira para a tabela departamentos e corresponde a qual departamento o empregado trabalhava no passado.';


ALTER TABLE historico_cargos ADD CONSTRAINT cargos_historico_cargos_fk
FOREIGN KEY (id_cargo)
REFERENCES cargos (id_cargo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE empregados ADD CONSTRAINT cargos_empregados_fk
FOREIGN KEY (id_cargo)
REFERENCES cargos (id_cargo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE paises ADD CONSTRAINT regioes_paises_fk
FOREIGN KEY (id_regiao)
REFERENCES regioes (id_regiao)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE localizacoes ADD CONSTRAINT paises_localizacoes_fk
FOREIGN KEY (id_pais)
REFERENCES paises (id_pais)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE departamentos ADD CONSTRAINT localizacoes_departamentos_fk
FOREIGN KEY (id_localizacao)
REFERENCES localizacoes (id_localizacao)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE empregados ADD CONSTRAINT departamentos_empregados_fk
FOREIGN KEY (id_departamento)
REFERENCES departamentos (id_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE historico_cargos ADD CONSTRAINT departamentos_historico_cargos_fk
FOREIGN KEY (id_departamento)
REFERENCES departamentos (id_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE empregados ADD CONSTRAINT empregados_empregados_fk
FOREIGN KEY (id_supervisor)
REFERENCES empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE historico_cargos ADD CONSTRAINT empregados_historico_cargos_fk
FOREIGN KEY (id_empregado)
REFERENCES empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE departamentos ADD CONSTRAINT empregados_departamentos_fk
FOREIGN KEY (id_gerente)
REFERENCES empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;
