# uvv_bd_1_cc1m
#                  Pset 1

## Nome: Guilherme Souza Oliveira
## Professor: Abrantes
## Turma: CC1M


### Infelizmente, ontem durante a aula quando eu estava tirando uma dúvida, minha máquina virtual acabou dando problema e apagando todos os meus arquivos e meu progresso no pset. O professor tentou resolver, mas não tinha mais jeito e eu não tinha salvado nada no github ainda. Aqui está a única parte que consegui salvar, pois tinha salvado esse código antes. Peço que compreenda minha situação




### Aqui está o código gerado a partir do esquema "hr" que fiz no SQL Power Architect:


CREATE TABLE public.cargos (
                id_cargo VARCHAR(10) NOT NULL,
                cargo VARCHAR(35) NOT NULL,
                salario_minimo NUMERIC(8,2),
                salario_maximo NUMERIC(8,2),
                CONSTRAINT cargos__pk_ PRIMARY KEY (id_cargo)
);
COMMENT ON TABLE public.cargos IS 'Tabela que armazena os cargos existentes e a faixa salarial(máximo e mínimo que um empregado pode receber) para cada cargo';
COMMENT ON COLUMN public.cargos.id_cargo IS 'Chave primária da tabela cargos';
COMMENT ON COLUMN public.cargos.cargo IS 'Nome do cargo';
COMMENT ON COLUMN public.cargos.salario_minimo IS 'O menor salário oferecido por um cargo';
COMMENT ON COLUMN public.cargos.salario_maximo IS 'Maior salario oferecido por um cargo';


CREATE UNIQUE INDEX cargos_idx
 ON public.cargos
 ( cargo );

CREATE TABLE public.regioes (
                id_regiao INTEGER NOT NULL,
                nome VARCHAR(25) NOT NULL,
                CONSTRAINT regioes_pk PRIMARY KEY (id_regiao)
);
COMMENT ON TABLE public.regioes IS 'Tabela que contém os códigos e os nomes das regiões';
COMMENT ON COLUMN public.regioes.id_regiao IS 'Esta é a chave primária da tabela regiões';
COMMENT ON COLUMN public.regioes.nome IS 'Os nomes das regiões';


CREATE UNIQUE INDEX regioes_idx
 ON public.regioes
 ( nome );

CREATE TABLE public.paises (
                id_pais CHAR(2) NOT NULL,
                Nome VARCHAR(50) NOT NULL,
                id_regiao INTEGER NOT NULL,
                CONSTRAINT paises__pk_ PRIMARY KEY (id_pais)
);
COMMENT ON TABLE public.paises IS 'Tabela que contém as informações dos paises';
COMMENT ON COLUMN public.paises.id_pais IS 'É a chave primária da tabela paises';
COMMENT ON COLUMN public.paises.Nome IS 'Nome do país';
COMMENT ON COLUMN public.paises.id_regiao IS 'Esta é a chave estrangeira para a tabela regiões';


CREATE UNIQUE INDEX paises_idx
 ON public.paises
 ( Nome );

CREATE TABLE public.localizacoes (
                id_localizacao INTEGER NOT NULL,
                endereco VARCHAR(50),
                cep VARCHAR(12),
                cidade VARCHAR(50),
                uf VARCHAR(25),
                id_pais CHAR(2),
                CONSTRAINT localizacoes__pk_ PRIMARY KEY (id_localizacao)
);
COMMENT ON TABLE public.localizacoes IS 'Tabela que contém os endereços de várias facilidades e também escritórios da empresa, mas não armazena endereços de clientes';
COMMENT ON COLUMN public.localizacoes.id_localizacao IS 'É a chave primária da tabela localizações';
COMMENT ON COLUMN public.localizacoes.endereco IS 'Endereço (número, logradouro) de uma facilidade ou escritório da empresa';
COMMENT ON COLUMN public.localizacoes.cep IS 'É o CEP de uma localização ou facilidade da empresa';
COMMENT ON COLUMN public.localizacoes.cidade IS 'Cidade em que está localizado um escritório ou facilidade da empresa';
COMMENT ON COLUMN public.localizacoes.uf IS 'Estado onde está localizado um escritório ou uma facilidade da empresa. Pode ser abreviado ou por extenso';
COMMENT ON COLUMN public.localizacoes.id_pais IS 'É a chave estrangeira para a tabela paises';


CREATE TABLE public.departamentos (
                id_departamento INTEGER NOT NULL,
                nome VARCHAR(50),
                id_localizacao INTEGER,
                id_gerente INTEGER,
                CONSTRAINT departamentos__pk_ PRIMARY KEY (id_departamento)
);
COMMENT ON TABLE public.departamentos IS 'Tabela que contém informações sobre os departamentos da empresa';
COMMENT ON COLUMN public.departamentos.id_departamento IS 'Chave primária da tabela departamentos';
COMMENT ON COLUMN public.departamentos.nome IS 'Nome do departamento da empresa';
COMMENT ON COLUMN public.departamentos.id_localizacao IS 'É a chave estrangeira para a tabela localizações';
COMMENT ON COLUMN public.departamentos.id_gerente IS 'Chave estrangeira para a tabela empregados, que representa qual empregado, se houver, é o gerente deste departamento';


CREATE UNIQUE INDEX departamentos_idx
 ON public.departamentos
 ( nome );

CREATE TABLE public.empregados (
                id_empregado INTEGER NOT NULL,
                nome VARCHAR(75) NOT NULL,
                email VARCHAR(35) NOT NULL,
                telefone VARCHAR(20),
                data_contratacao DATE NOT NULL,
                id_cargo VARCHAR(10) NOT NULL,
                salario NUMERIC(8,2),
                comissao NUMERIC(4,2),
                id_departamento INTEGER,
                id_supervisor INTEGER,
                CONSTRAINT empregados__pk_ PRIMARY KEY (id_empregado)
);
COMMENT ON TABLE public.empregados IS 'Tabela com as informações dos empregados da empresa';
COMMENT ON COLUMN public.empregados.id_empregado IS 'Chave primária da tabela empregados';
COMMENT ON COLUMN public.empregados.nome IS 'Nome completo do funcionário';
COMMENT ON COLUMN public.empregados.email IS 'Parte inicial do email do funcionário (antes do @)';
COMMENT ON COLUMN public.empregados.telefone IS 'Número de telefone do empregado';
COMMENT ON COLUMN public.empregados.data_contratacao IS 'Data em que o empregado iniciou no seu cargo atual';
COMMENT ON COLUMN public.empregados.id_cargo IS 'Chave estrangeira para a tabela cargos, indica o cargo atual do empregado';
COMMENT ON COLUMN public.empregados.salario IS 'Salário que o empregado recebe por mês';
COMMENT ON COLUMN public.empregados.comissao IS 'Porcentagem de comissão de um empregado (apenas para os empregados do departamento de vendas)';
COMMENT ON COLUMN public.empregados.id_departamento IS 'Chave estrangeira para a tabela departamentos, e indica o departamento atual de um funcionário';
COMMENT ON COLUMN public.empregados.id_supervisor IS 'Chave estrangeira para a própria tabela empregados (auto-relacionamento). Indica o supervisor direto de um empregado';


CREATE UNIQUE INDEX empregados_idx
 ON public.empregados
 ( email );

CREATE TABLE public.historico_cargos (
                id_empregado INTEGER NOT NULL,
                data_inicial DATE NOT NULL,
                data_final DATE NOT NULL,
                id_cargo VARCHAR(10) NOT NULL,
                id_departamento INTEGER,
                CONSTRAINT historico_cargos__pk_ PRIMARY KEY (id_empregado, data_inicial)
);
COMMENT ON TABLE public.historico_cargos IS 'Tabela que contém o hitórico de um empregado. Quando um funcionário muda de cargo dentro de um departamento ou de um departamento dentro de um cargo novas linhas tem que ser inseridas com a informação antiga do empregado';
COMMENT ON COLUMN public.historico_cargos.id_empregado IS 'Parte da chave primária composta da tabela. Também é uma chave estrangeira para a tabela empregados';
COMMENT ON COLUMN public.historico_cargos.data_inicial IS 'Parte da chave primária composta da tabela. Ela indica a data inicial de um funcionário em um novo cargo e deve ser menor que a data_final';
COMMENT ON COLUMN public.historico_cargos.data_final IS 'Corresponde ao último dia de um funcionário neste cargo e deve ser maior que a data_inicial';
COMMENT ON COLUMN public.historico_cargos.id_cargo IS 'Chave estrangeira para a tabela cargos';
COMMENT ON COLUMN public.historico_cargos.id_departamento IS 'Chave estrangeira para a tabela departamentos';


ALTER TABLE public.empregados ADD CONSTRAINT cargos_empregados_fk
FOREIGN KEY (id_cargo)
REFERENCES public.cargos (id_cargo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.historico_cargos ADD CONSTRAINT cargos_historico_cargos_fk
FOREIGN KEY (id_cargo)
REFERENCES public.cargos (id_cargo)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.paises ADD CONSTRAINT regioes_paises_fk
FOREIGN KEY (id_regiao)
REFERENCES public.regioes (id_regiao)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.localizacoes ADD CONSTRAINT paises_localizacoes_fk
FOREIGN KEY (id_pais)
REFERENCES public.paises (id_pais)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.departamentos ADD CONSTRAINT localizacoes_departamentos_fk
FOREIGN KEY (id_localizacao)
REFERENCES public.localizacoes (id_localizacao)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.empregados ADD CONSTRAINT departamentos_empregados_fk
FOREIGN KEY (id_departamento)
REFERENCES public.departamentos (id_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.historico_cargos ADD CONSTRAINT departamentos_historico_cargo_fk
FOREIGN KEY (id_departamento)
REFERENCES public.departamentos (id_departamento)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.departamentos ADD CONSTRAINT empregados_departamentos_fk
FOREIGN KEY (id_gerente)
REFERENCES public.empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.empregados ADD CONSTRAINT empregados_empregados_fk
FOREIGN KEY (id_supervisor)
REFERENCES public.empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;

ALTER TABLE public.historico_cargos ADD CONSTRAINT empregados_historico_cargo_fk
FOREIGN KEY (id_empregado)
REFERENCES public.empregados (id_empregado)
ON DELETE NO ACTION
ON UPDATE NO ACTION
NOT DEFERRABLE;
