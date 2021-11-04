## MongoDB - Modelagem


As aplicações do atual momento necessitam de caractrísticas diferentes para se tornarem mais velozes, escalaveis e tolerantes a falha e isso gerou uma necessidade também da adaptação dos banco de dados para atender a demanda. Os bancos de dados relacionais necessitam de um modelo de dados definido para que seu uso seja feito, o que implica em diversos fatores negativos como inflexibilidade, escalabilabidade vertical e alto custo de processamento. Os bancos de dados não-relacionais sugiram para atender demandas específicas e hoje vamos falar sobre algumas dicas para modelar corretamente sua implementação em um banco de dados NoSQL de documentos. 

As dicas aqui são voltadas para o MongoDB, que é um banco de dados de documentos com caracterísitcas de escalabilidade e flexibilidade, classificado como programa de banco de dados NoSQL(Not Only SQL).

- Evite o uso de junção(JOIN) entre os documentos.
- Evite a criação de documentos muito grandes.
- Evite campos multivalorados com muitos elementos.
- Utilize nomemclatura objetiva para nomear as chaves/campos.
- Armazene no documento somente os dados necessários.
- Sempre que possível agrege informações no documento utilizando um subconjunto de dados.

Modelo relacional

![image](https://github.com/ralfsilvadba/NoSQL/blob/main/img/EXEMPLO_MODELO_RELACIONAL.png)

Modelo desnormalizado

![image](https://github.com/ralfsilvadba/NoSQL/blob/main/img/EXEMPLO_DESNORMALIZADO.png)

Modelo relacional com tipo de dados

![image](https://github.com/ralfsilvadba/NoSQL/blob/main/img/EXEMPLO_METADADOS.png)

Modelo desnormalizado com tipo de dados

![image](https://github.com/ralfsilvadba/NoSQL/blob/main/img/EXEMPLO_METADADOS_DOCUMENTO.png)

A MongoDB também disponibiliza uma série de padrões de modelagem que podemos utilizar.

- Catalog: Como catálogo de produtos e serviços;
- Content Management: Como em ECMs (gerenciadores de conteúdo corporativo);
- Internet of Things: Como em sistemas com sensores real-time, como na indústria 4.0;
- Mobile: Como em aplicações móveis de baixa latência e alta escala;
- Personalization: Como em sistemas com schema de dados personalizável para entregar conteúdo relevante aos usuários, muito usados no marketing digital;
- Real-Time Analytics: Como em sistemas de estatísticas real-time;
- Single View: Como em sistemas de dashboards, gerenciais e outros que agregam informações de diferentes fontes em uma base só;
