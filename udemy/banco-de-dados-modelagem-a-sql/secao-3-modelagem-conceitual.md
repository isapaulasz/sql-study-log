# Seção 3 - Modelagem Conceitual

## Aula 14 - Introdução ao Modelo Conceitual - Parte 1

- Um projeto de banco de dados deve avaliar a necessidade das informações que serão armazenadas e conceber um banco de dados eficaz e eficiente
- Um projeto tem 4 etapas:
  - Modelo conceitual
  - Normalização
  - Modelo lógico
  - Modelo físico
- O modelo conceitual deve ser de fácil entendimento para o usuário, pois deve ser construído com a ajuda dele
- Esta etapa busca uma definição certeira dos dados e precisa especificar os objetos presentes no projeto e seus relacionamentos

## Aula 15 - Introdução ao Modelo Conceitual - Parte 2

- O modelo não pode ser ambíguo, pois servirá de meio de comunicação entre usuários e desenvolvedores
- Elementos presentes no modelo:
  - Entidade: representa um objeto do mundo real ou conceitual
  - Atributos: propriedades das entidades
  - Relacionamentos: relação entre entidades que se conectam, apresentando uma associação

## Aula 16 - O "Levantamento de requisitos" - Parte 1

- O levantamento de requisitos reúne as descrições dos serviços fornecidos pelo sistema e suas restrições operacionais
- Com o levantamento em mãos, a equipe deverá aplicar técnicas para transformar tais requisitos em projetos apropriados às necessidades do sistema

## Aula 17 - O "Levantamento de requisitos" - Parte 2

- Estudo de caso: projeto de automatização de um mercantil de pequeno porte, destinado a melhorar a eficiência e o controle de estoque
- Os requisitos destacados incluem o registro de vendas com identificação do cliente e a possibilidade de cadastrar novos clientes
- O sistema também deve gerar documentos vinculados às vendas, contendo informações como data de vencimento e valor
- Além disso, o controle de estoque é enfatizado, sendo necessárias a atualização das quantidades disponíveis a cada venda e o registro adequado das entradas e saídas de produtos

## Aula 18 - Detectar as possíveis entidades - Parte 1

- Entidade: objeto do mundo real ou conceitual

## Aula 19 - Detectar as possíveis entidades - Parte 2

- Detectar: ler o levantamento de requisitos, extraindo dele todas as possíveis entidades
- Nesta primeira etapa, todas as possíveis entidades são listadas

## Aula 20 - Eliminar entidades que descrevem o domínio

- Entre as possíveis entidades listadas, alguns elementos provavelmente só estão na lista para descrever o domínio do negócio
- Esses elementos devem ser eliminados (ex: o próprio mercantil)

## Aula 21 - Eliminar referências ao aplicativo

- Todas as referências a funcionalidades, configurações e design do futuro software devem ser eliminadas (ex: sistema, tela, relatório)

## Aula 22 - Eliminar entidades com instância única

- É possível existir elementos que foram detectados como entidades mas que, após uma análise mais detalhada, se mostram um conjunto unitário, ou seja, será armazenada uma única instância da entidade
- Entidades com uma única instância devem ser eliminadas

## Aula 23 - Eliminar entidades com atributo único

- É necessário revisar as entidades restantes para verificar se elas possuem mais de um atributo
- Caso alguma entidade possua apenas um atributo, é provável que ela seja um atributo de outra entidade

## Aula 24 - Detectar os possíveis relacionamentos

- A associação entre as entidades se dá através de conexões denominadas relacionamentos
- Essas associações são identificadas através de verbos ou preposições que conectam duas ou mais entidades (ex: o cliente "realiza" uma venda)

## Aula 25 - Determinar cardinalidade

- Define o grau de relacionamento existente entre entidades
- Dependendo das regras de negócio, a cardinalidade poderá ser do tipo:
  - 1:1: um para um
  - 1:N: um para muitos (ex: um cliente realiza várias vendas)
  - N:N: muitos para muitos (ex: uma venda possui vários produtos, e um produto aparece em várias vendas)
- Para identificar a cardinalidade, é necessário se colocar no lugar de uma entidade e tentar identificar com quantas entidades do outro lado do relacionamento ela pode se relacionar

## Aula 26 - Detectar os atributos

- Os atributos são propriedades ou características de uma entidade
- É possível detectá-los através de adjetivos conectados de forma direta ou indireta às entidades
- Atributos também podem ser vistos como substantivos

## Aula 27 - Tipos de atributos

- Quanto à quantidade de informação:
  - monovalorado: formado por um único valor
  - multivalorado: formado por um conjunto de valores
    - ex: é possível um cliente ter diversos números de telefone
- Quanto à atomicidade:
  - simples ou atômico: atributo indivisível
  - composto: pode ser dividido em partes menores
    - ex: endereço contém diversas informações que podem ser divididas (rua, número, cidade)
- Quanto à unicidade:
  - chave simples: atributo que garante a unicidade das entidades
    - ex: o CPF é único
  - chave composta: conjunto de atributos que garantem a unicidade das entidades
    - ex: código da venda + código do produto
- Quanto ao armazenamento:
  - armazenado: atributo que tem seu valor armazenado no banco de dados
  - derivado: atributo que tem seu valor calculado a partir de outros valores armazenados no banco de dados
    - ex: a idade, calculada a partir da data de nascimento
