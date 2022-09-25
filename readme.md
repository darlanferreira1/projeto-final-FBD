> Projeto da disciplina de fundamentos de banco de dados 
# Requisitos do sistema

Alunos: Darlan Oliveira Ferreira e Guilherme dos Santos Cavalcante

## Sistema para gestão hospitalar.


### DESCRIÇÃO: 
Hospitais são uma parte muito importante na vida de um cidadão, tentando fornecer as melhores instalações médicas para as diversas pessoas que sofrem de vários tipos de doenças e sendo o ambiente de trabalho de muitos profissionais importantíssimos para a sociedade, assim, fica difícil para o hospital manter suas atividades e registros do dia-a-dia manualmente. É por isso que um banco de dados é necessário para manter registros de todos os tipos de atividades de um ambiente como esse.
O sistema em questão possui funcionalidades de controle e gerenciamento de funcionários (médicos e enfermeiras), pacientes, logística de quartos, departamentos, remédios, atendimentos e procedimentos do hospital, sendo realizadas por meio de operações de criação, leitura, atualização e remoção de dados.


### ENTIDADES E RELACIONAMENTOS:

###### MEDICO: 
- Atributos:
  - medicoID (int) (PK)
  - nome (text)
  - cargo (text)
  - CRM (int)

- Descrição: entidade responsável por atender os pacientes. 
  - Cardinalidade: 
    - MEDICO (1-N) DEPARTAMENTO 
    - MEDICO (1-N) AFILIADO_COM
    - MEDICO (1-N) ATENDIMENTO 
    - MEDICO (1-N) PRESCRICAO
    - MEDICO (1-N) PACIENTE


###### DEPARTAMENTO:
- Atributos:
  - departamentoID (int) (PK)
  - nome (text)
  - medico_chefe (int) (FK - MEDICO, medicoID)

- Descrição: um hospital tem vários departamentos.
  - Cardinalidade: 
    - DEPARTAMENTO (N-1) MEDICO
    - DEPARTAMENTO (1-N) AFILIADO_COM

###### AFILIADO_COM:
- Atributos:
  - medico (int) (PK) (FK - MEDICO, medicoID)
  - departamento (int) (PK) (FK - DEPARTAMENTO, departamentoID)
  - primeira_afiliacao (boolean)

- Descrição: um médico pode estar filiado ou não
  - Cardinalidade: 
    - AFILIADO_COM (N-1) MEDICO
    - AFILIADO_COM (N-1) DEPARTAMENTO

###### PROCEDIMENTO:
- Atributos:
  - codigo (int) (PK)
  - nome (text)

###### PACIENTE:
- Atributos:
  - pacienteID (int) (PK)
  - nome (text)
  - idade (int)
  - endereco (text)
  - telefone (text)
  - principal_medico (int) (FK - MEDICO, medicoID)

- Descrição: armazena dados dos pacientes
  - Cardinalidade: 
    - PACIENTE (N-1) MEDICO
    - PACIENTE (1-N) ATENDIMENTO
    - PACIENTE (1-N) PRESCRICAO
    - PACIENTE (1-N) SOB_ATENDIMENTO


###### ENFERMEIRA:
- Atributos:
  - enfermeiraID (int) (PK)
  - nome (text)
  - cargo (text)
  - registrado (boolean)
  - COREN (int)

- Descrição: responsável por  cuidar dos pacientes
  - Cardinalidade:	
    - ENFERMEIRA (1-N) ATENDIMENTO
    - ENFERMEIRA (1-N) PLANTAO


######  ATENDIMENTO:
- Atributos:
  - atendimentoID(int) (PK)
  - paciente (int) (FK - PACIENTE, pacienteID)
  - enfermeira (int) (FK - ENFERMEIRA, enfermeiraID)
  - medico (int) (FK - MEDICO, medicoID)
  - comeco_atendimento (data/time)
  - fim_atendimento (data/time)
  - sala_exame  (text)

- Descrição: responsável por  guardar os dados do atendimento.
  - Cardinalidade:	
    - ATENDIMENTO (N-1) PACIENTE
    - ATENDIMENTO (N-1) MEDICO
    - ATENDIMENTO (N-1) PRESCRICAO 


###### MEDICAMENTO:
- Atributos:
  - codigo (int) (PK)
  - nome (text)
  - marca (text)
  - descrição (text)

###### PRESCRICAO:
- Atributos:
  - medico (int) (FK - MEDICO, medicoID)
  - paciente (int) (FK - PACIENTE, pacienteID)
  - medicamento (int) (FK - MEDICAMENTO, codigo)
  - data (data/time)
  - atendimento (int) (FK - ATENDIMENTO, atendimentoID)
  - dose (text)
	
- Descrição: armazena os dados da prescrição passada pelo médico.
  - Cardinalidade:	
    - PRESCRICAO (N-1) MEDICO
    - PRESCRICAO (N-1) PACIENTE
    - PRESCRICAO (1-N) ATENDIMENTO 


###### BLOCO:
- Atributos:
  - andar: (int) (PK)
  - código_do_bloco: (int) (PK)


###### QUARTO:
- Atributos:
  - número_quarto: (int) (PK)
  - tipo_quarto: (text)
  - andar: (int)
  - codigo_do_bloco: (int) (FK - BLOCO, código_do_bloco) 
  - indisponível: (bool)


###### PLANTAO:
- Atributos:
  - enfermeira: (int) (PK) (FK - ENFERMEIRA, enfermeiraID)
  - andar: (int) (PK)
  - codigo_do_bloco: (int) (PK)
  - inicio: (time) (PK)
  - fim: (time) (PK)

- Descrição: informações sobre o plantão de uma infermeira.
  - Cardinalidade:	
    - PLANTAO (N-1) ENFERMEIRA


###### SOB_ATENDIMENTO:
- Atributos:
  - sob_atendimentoID: (int) (PK)
  - paciente: (text) (FK - PACIENTE, pacienteID)
  - quarto: (int) (FK - QUARTO, numero_do_quarto)
  - inicio: (time)
  - fim:  (time)

- Descrição: pacientes que estão sob atendimento.
  - Cardinalidade:	
    - SOB_ATENDIMENTO (N-1) PACIENTE

### PERGUNTAS QUE O SISTEMA RESPONDE:

1.  Quantidade de médicos e seus respectivos cargos.
1.  Quantidade de enfermeiras e seus respectivos cargos.
1.  Observando os dados, é possível determinar o horário de pico dos pacientes, e então alocar mais médicos ou enfermeiras para agilizar o atendimento. 
1.  Quantidade de pacientes irão ser atendidos, que estão em atendimento ou que já foram atendidos.
1.  Controle das prescrições emitidas por um médico, e a quantidade de prescrições que um paciente possui.
1.  Tipos de procedimentos realizados pelo hospital. 
1.  Gerenciamento de quartos onde são realizados os tratamentos do hospital.
1.  Controle e gerenciamento de plantões dos funcionários.
1.  Com base nos dados, é possível determinar uma patologia muito frequente na população e a partir disso tomar medidas de controle.
1.  Que a tecnologia e a ciência de dados são essenciais na área da saúde e na sociedade como um todo.
