# Seção 4 - Normalização

## Aula 28 - Introdução à normalização

- Normalização é um conjunto de passos que permite um armazenamento eficaz e eficiente dos dados
- Objetivos:
  - simplificar o armazenamento no banco de dados, buscando manter sempre atributos atômicos
  - garantir a integridade, minimizando a redundância

## Aula 29 - Chave candidata

- Chave candidata: atributo ou conjunto de atributos com potencial de garantir a unicidade
  - chave primária: chave candidata eleita
  - chave alternativa: chave candidata não eleita
  - chave superposta: chave candidata composta que tem pelo menos um atributo em comum com outra chave candidata
  - chave disjunta: chave candidata que não tem superposição

## Aula 30 - Dependência Funcional

- É o relacionamento entre dois ou mais atributos, em que um ou mais atributos determinam o valor de outros atributos
- Tipos de dependência funcional: parcial, transitiva, multivalorada e cíclica
  - cada uma é tratada em uma forma normal, apresentada nas próximas aulas
- No curso, a dependência cíclica é a que a literatura costuma chamar de dependência de junção

## Aula 31 - Primeira forma normal (1FN)

- Uma tabela está na 1FN se não possuir atributo multivalorado ou atributo composto
- Para atingir a 1FN, os atributos compostos são divididos em partes e os multivalorados passam para uma tabela própria

## Aula 32 - Segunda forma normal (2FN)

- Uma tabela está na 2FN se estiver na 1FN e não possuir dependência funcional parcial
- Dependência funcional parcial ocorre quando um ou mais atributos não chave têm dependência funcional de apenas parte da chave primária composta
- A 2FN é aplicada quando existe chave primária composta

## Aula 33 - Terceira forma normal (3FN)

- Uma tabela está na 3FN se estiver na 2FN e não possuir dependência funcional transitiva
- Dependência funcional transitiva ocorre quando um ou mais atributos não chave têm dependência funcional de um ou mais atributos que não fazem parte da chave primária

## Aula 34 - Forma normal de Boyce-Codd (FNBC)

- Deve-se aplicar a FNBC em tabelas que estejam na 3FN, que possuam mais de uma chave candidata e tenham chaves superpostas
- Esta forma normal é mais restritiva que a 3FN: toda tabela na FNBC já está na 3FN, por isso ela não depende da forma normal anterior para ser aplicada

## Aula 35 - Quarta forma normal (4FN)

- Uma tabela está na 4FN se estiver na 3FN/FNBC e não possuir dependência funcional multivalorada
- Dependência funcional multivalorada ocorre quando dois ou mais atributos multivalorados dependem de um terceiro atributo, mas são independentes entre si
  - ex: um funcionário que possui vários idiomas e vários cursos, sem relação entre eles

## Aula 36 - Quinta forma normal (5FN)

- Uma tabela está na 5FN se estiver na 4FN e não possuir dependência funcional cíclica
- Dependência funcional cíclica ocorre quando existem dependências entre atributos distintos de tal forma que o primeiro depende do segundo, o segundo do terceiro, e o enésimo depende do primeiro
