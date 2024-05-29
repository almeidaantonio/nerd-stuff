# Como Identificar e Corrigir Problemas na Sua REST API

# O que é uma API?

Interface de programação de aplicações (Application programming interface) é um conjunto de definições e protocolos para construir e integrar software de aplicação.

Em outras palavras, se você quiser interagir com um computador ou sistema para recuperar informações ou realizar uma função, uma API ajuda você a comunicar o que deseja a esse sistema para que ele possa entender e atender à solicitação.

# Tipos de API

## Bibliotecas e frameworks

A interface para uma biblioteca de software é um tipo de API. A API descreve e prescreve o "comportamento esperado" (uma especificação) enquanto a biblioteca é uma "implementação real" desse conjunto de regras.

A seguir, alguns exemplos de frameworks e bibliotecas:

| Linguagem | Framework/ Biblioteca |
| --- | --- |
| JavaScript/Node.js | Express.js |
|  | NestJS |
| Python | Flask |
|  | Django Rest Framework (DRF) |
| Java | Spring Boot |
| Go (Golang) | Gin |
|  | Fiber |
| PHP | Laravel |
| C# | http://asp.net/ Core |
| Rust | Rocket |
|  | Actix Web |

## Sistemas operacionais

Uma API pode especificar a interface entre uma aplicação e o sistema operacional.

- **POSIX:** fornece um conjunto de especificações de API comuns que visam permitir que uma aplicação escrita para um sistema operacional compatível com POSIX seja compilada para outro sistema operacional compatível com POSIX.
- **Windows API (Win32):** API designada para que aplicativos mais antigos possam ser executados em versões mais novas do Windows usando uma configuração específica do executável chamada "Modo de Compatibilidade".

## **Web APIs**

APIs Web são um serviço acessado a partir de dispositivos clientes para um servidor web usando o Protocolo de Transferência de Hipertexto (HTTP). Dispositivos clientes enviam uma solicitação na forma de uma requisição HTTP e recebem uma mensagem de resposta.

### Arquitetura de Web APIs

| Estilo de API | Prós | Contras | Casos de Uso |
| --- | --- | --- | --- |
| REST | Simples, flexível, escalável, amigável para a web | Sem contrato claro, sem consultas complexas, tratamento de erros fraco | Aplicações orientadas a dados, serviços web, cache, sem estado |
| SOAP | Contrato claro, consultas complexas, bom tratamento de erros | Complexo, verboso, não escalável, não amigável para a web | Aplicações transacionais, segurança, autenticação, interoperabilidade |
| GraphQL | Independente de linguagem, único ponto de extremidade, fortemente tipado, eficiência de dados | Complexo, difícil de usar, sem cache, tratamento de erros fraco | Modelos de dados complexos e dinâmicos, requisitos impulsionados pelo cliente, largura de banda e desempenho |
| gRPC | Independente de linguagem, contrato claro, consultas complexas, rápido e eficiente | Complexo, difícil de usar, não amigável para a web, inflexível | Modelos de dados complexos e dinâmicos, comunicação entre microsserviços, velocidade e eficiência |
| WebSocket | Comunicação rápida e eficiente, bidirecional e multiplexada, comunicação em tempo real e orientada a eventos | Não suportado por alguns navegadores ou proxies mais antigos, não seguro por padrão, não mantém estado | Troca de dados rápida e interativa, como chat, jogos ou streaming |
| Webhooks | Simples, fácil de usar, amigável para a web, escalável | Sem contrato claro, sem consultas complexas, tratamento de erros fraco | Modelos de dados simples e estáveis, notificações orientadas a eventos, desempenho e escalabilidade |

## **REST**

REST é uma maneira de usar padrões da web e endereços para trabalhar com dados em um servidor. Eles são populares, fáceis de implementar e usam métodos HTTP. A maioria dos serviços da web com os quais você interage diariamente, como Twitter ou YouTube, são alimentados por APIs Restful. Por exemplo, um cliente pode usar GET para obter dados, POST para criar novos dados, PUT para alterar dados ou DELETE para remover dados. Os dados geralmente estão em formatos como JSON ou XML. Alguns dos principais princípios do REST são:

1. **Stateless (Sem Estado):** Cada requisição feita pelo cliente para o servidor deve conter toda a informação necessária para entender e processar a requisição. Isso significa que cada requisição deve ser independente das requisições anteriores, ou seja, o servidor não deve manter nenhum estado da sessão do cliente entre requisições.
2. **Client-Server (Cliente-Servidor):** O cliente e o servidor devem ser separados, permitindo que eles evoluam independentemente. Isso promove a escalabilidade ao permitir que o cliente e o servidor sejam desenvolvidos e melhorados separadamente.
3. **Cacheable (Cacheável):** As respostas do servidor devem ser explicitamente indicadas como cacheáveis ou não-cacheáveis. Isso permite que as respostas sejam armazenadas em cache pelo cliente ou por intermediários, melhorando a eficiência e a escalabilidade do sistema.
4. **Layered System (Sistema em Camadas):** O sistema deve ser composto por camadas, onde cada componente (como um proxy ou gateway) só conhece a camada imediatamente inferior. Isso promove a escalabilidade ao permitir que os sistemas sejam expandidos em várias camadas, sem que cada componente conheça toda a complexidade do sistema.
5. **Uniform Interface (Interface Uniforme):** O sistema deve ter uma interface uniforme entre os componentes, o que simplifica e desacopla a arquitetura geral. Esta interface deve incluir recursos identificáveis, manipulação de recursos através de representações, mensagens auto-descritivas e hiperlinks para navegar entre recursos relacionados.

## **Sintomas Comuns de Problemas em REST APIs**

### Erros de tempo de execução (500 Internal Server Error)

Geralmente, o mínimo que se espera de resposta de uma API é que ela detecte a ocorrência de um ou mais erros na entrada, ou seja, requeremos a detecção de erros. Porém, a forma menos informativa disso é: “a entrada contém erro(s) de sintaxe”. 

Então, o que acontece quando uma requisição contendo erros é oferecida para uma API com uma boa capacidade de detecção de erros? A API pode dizer: "Então, há um erro tal na entrada tal , então eu desisto".

O ideal é que realizemos todas as tratativas preventivas possíveis e viáveis, e que, em caso de erro, retornemos uma mensagem detalhada juntamente com o código de status apropriado.

### Exemplo de erros mais comuns

### 1xx: Informational

1. **100 Continue**
    - **Caso de uso:** O servidor recebeu os cabeçalhos da solicitação e o cliente pode prosseguir enviando o corpo da solicitação.

### 2xx: Success

1. **200 OK**
    - **Caso de uso:** A solicitação foi bem-sucedida e o servidor retornou os dados solicitados.
2. **201 Created**
    - **Caso de uso:** A solicitação foi bem-sucedida e um novo recurso foi criado como resultado.
3. **202 Accepted**
    - **Caso de uso:** A solicitação foi recebida, mas ainda não foi processada. Isso é útil para operações assíncronas.
4. **204 No Content**
    - **Caso de uso:** A solicitação foi bem-sucedida, mas não há conteúdo para retornar. Usado frequentemente em respostas de DELETE.

### 3xx: Redirection

1. **301 Moved Permanently**
    - **Caso de uso:** O recurso solicitado foi movido permanentemente para um novo URI.
2. **302 Found**
    - **Caso de uso:** O recurso solicitado foi encontrado em outro URI temporariamente.
3. **304 Not Modified**
    - **Caso de uso:** O recurso não foi modificado desde a última solicitação. Utilizado para caching.

### 4xx: Client Error

1. **400 Bad Request**
    - **Caso de uso:** A solicitação não pôde ser entendida ou estava incorreta.
2. **401 Unauthorized**
    - **Caso de uso:** A autenticação é necessária e falhou ou não foi fornecida.
3. **403 Forbidden**
    - **Caso de uso:** O servidor entendeu a solicitação, mas se recusa a autorizá-la.
4. **404 Not Found**
    - **Caso de uso:** O recurso solicitado não foi encontrado no servidor.
5. **405 Method Not Allowed**
    - **Caso de uso:** O método especificado na solicitação não é permitido para o recurso.
6. **409 Conflict**
    - **Caso de uso:** A solicitação não pôde ser concluída devido a um conflito com o estado atual do recurso.
7. **422 Unprocessable Entity**
    - **Caso de uso:** A solicitação está bem formatada, mas não pode ser processada devido a erros semânticos.

### 5xx: Server Error

1. **500 Internal Server Error**
    - **Caso de uso:** O servidor encontrou uma condição inesperada que o impediu de atender à solicitação.
2. **502 Bad Gateway**
    - **Caso de uso:** O servidor, ao atuar como um gateway ou proxy, recebeu uma resposta inválida do servidor upstream.
3. **503 Service Unavailable**
    - **Caso de uso:** O servidor está temporariamente indisponível, geralmente devido a manutenção ou sobrecarga.
4. **504 Gateway Timeout**
    - **Caso de uso:** O servidor, ao atuar como um gateway ou proxy, não recebeu uma resposta oportuna do servidor upstream.

## **Ausência de cache no Lado do Servidor**

Alcançar um alto nível de desempenho é desafiador. Há muitos fatores que impactam a velocidade de uma aplicação, desde o código que forma sua base e o servidor onde ele está hospedado.

Uma maneira melhorar o desempenho da aplicação, é o uso cache (server side cache).

### Características gerais

- Melhora a velocidade geral para o cliente ao processar a carga no servidor e reduzir os tempos de processamento.
- Crucial para a escalabilidade, reduzindo a demanda de recursos e lidando com mais solicitações e dados.
- Mais complexo, exigindo um mergulho profundo nas camadas e configurações de cache.

### Ferramentas de cache

Tecnologias como Batcache, Varnish e Redis são populares, cada uma com seus pontos fortes, adequados para diferentes tipos de necessidades de cache.

Além dessas ferramentas, é sempre possível implementar internamente na aplicação um gerenciamento de dados em memória, baseado no modelo "chave-valor".

## **Incompatibilidade de impedância** objeto-relacional (**impedance mismatch)**

A impedância objeto-relacional é um conjunto de dificuldades ao transitar entre dados em armazenamentos de dados relacionais e dados em modelos de objetos orientados ao domínio. 

Sistemas de Gerenciamento de Bancos de Dados Relacionais (SGBDR) são o método padrão para armazenar dados em um banco de dados dedicado, enquanto a programação orientada a objetos (OO) é o método padrão para design centrado em negócios nas linguagens de programação. 

O problema reside nem nos bancos de dados relacionais nem na programação OO, mas na dificuldade conceitual de mapeamento entre os dois modelos lógicos.  As questões variam de escala de aplicação a escala empresarial, sempre que dados relacionais armazenados são usados em modelos de objetos orientados ao domínio, e vice-versa.

## Solucionando a impedância

### Arquiteturas alternativas

NoSQL e/ou Bancos de dados orientados a objetos.
Mapeamento funcional-relacional. 

### Vantagens

- Direto para frameworks e automação em torno do transporte, apresentação e validação de dados
- Código menor, mais rápido, mais ágil
- Esquema de banco de dados dinâmico
- Correspondência de namespace e semântica
- Restrições expressivas
- Evita mapeamento complexo

### Desvantagens

- Sem tipagem estática. Acessores tipados mitigam isso.
- Custo de desempenho de indireção
- Ignora conceitos como polimorfismo
