# RabbitMQ

O RabbitMQ é um sistema de mensageria essencial em sistemas distribuídos, atuando como um intermediário que permite que microsserviços e outros componentes se comuniquem de forma assíncrona.

## **Message Broker e Protocolos Suportados**

O RabbitMQ é um Message Broker que implementa protocolos como AMQP, MQTT, STOMP e HTTP. O AMQP, baseado no TCP, é o protocolo mais comumente utilizado, garantindo alta velocidade de comunicação.

## **Baseado em Erlang para Desempenho e Confiabilidade**

Desenvolvido em Erlang, o RabbitMQ é conhecido por sua alta concorrência e tolerância a falhas, tornando-o extremamente rápido e confiável. Sua estrutura de desacoplamento entre serviços o torna uma escolha poderosa.

## **Conceito de Exchanges e Filas**

O RabbitMQ usa Exchanges e Filas para rotear mensagens. Exchanges determinam qual fila receberá uma mensagem com base em regras específicas.

## **Tipos de Exchanges no RabbitMQ**

- **Direct Exchange**: Roteia mensagens com base em chaves de roteamento específicas.
- **Fanout Exchange**: Distribui mensagens para todas as filas vinculadas, atendendo a todos os consumidores.
- **Topic Exchange**: Roteia mensagens com base em padrões de chave de roteamento, permitindo maior flexibilidade.
- **Headers Exchange**: Roteia mensagens com base em cabeçalhos de mensagens, ignorando a chave de roteamento.

## **Filas: Armazenamento de Mensagens**

As filas armazenam mensagens até que os consumidores as leiam. O RabbitMQ segue o princípio FIFO, garantindo que as mensagens sejam entregues na ordem em que foram recebidas.

## **Propriedades das Filas**

As filas podem ser configuradas com várias propriedades, incluindo durabilidade, exclusividade, limites de mensagens e tamanho. Essas propriedades ajudam a gerenciar o comportamento da fila.

- Durabilidade: As filas podem ser declaradas como duráveis, o que significa que sobreviverão a reinicializações do sistema. Filas não duráveis são removidas quando o sistema é reiniciado.
- Exclusividade: Filas exclusivas só podem ser acessadas pela conexão que as declarou. Quando a conexão se desconecta, a fila é excluída.
- Limite de Mensagens e Tamanho: É possível definir limites para o número de mensagens ou o tamanho total da fila. Isso ajuda a evitar o congestionamento da fila.

**Mensagens Mortas (Dead Letter Queues)**

Mensagens não entregues, devido à falta de consumidores ou outros motivos, podem ser encaminhadas para uma Dead Letter Queue. Isso permite a análise e o tratamento posterior dessas mensagens não processadas.

**Lazy Queues e Armazenamento em Disco**

O RabbitMQ oferece Lazy Queues, onde as mensagens podem ser armazenadas em disco para evitar sobrecargas de memória quando há um grande volume de mensagens.

## Confiabilidade

O RabbitMQ desempenha um papel crucial na garantia da confiabilidade em sistemas de mensageria distribuída, assegurando que as mensagens enviadas e recebidas não se percam durante o processo de comunicação.

### **Garantindo a Entrega de Mensagens**

A confiabilidade no RabbitMQ visa garantir que as mensagens não sejam perdidas no meio do caminho e que sejam processadas corretamente pelos consumidores. Imagine que cada mensagem é valiosa, e é essencial ter certeza de que essas mensagens são entregues e processadas com sucesso.

### **Mecanismos de Confiabilidade no RabbitMQ**

O RabbitMQ oferece três mecanismos principais para garantir a confiabilidade:

- **Consumer Acknowledgement (Confirmação do Consumidor)**: Isso ocorre quando um consumidor recebe uma mensagem e envia uma confirmação (Ack) de que a mensagem foi recebida e processada com sucesso.

- **Publisher Confirm (Confirmação do Publicador)**: Esse mecanismo permite que o publicador (quem envia as mensagens) tenha a certeza de que o RabbitMQ recebeu a mensagem com sucesso. Isso é crucial quando se trata de mensagens críticas ou valiosas. O publicador recebe uma confirmação da Exchange, indicando que a mensagem foi entregue com sucesso.

- **Filas e Mensagens Duráveis**: Embora nem sempre seja aplicável, é possível configurar filas e mensagens como duráveis e persistentes. Isso garante que as mensagens não sejam perdidas, mesmo em caso de falha do sistema, tornando-se um mecanismo adicional de confiabilidade.

#### **Consumer Acknowledgement (Confirmação do Consumidor)**

Dentro do mecanismo de Consumer Acknowledgement, existem três tipos de confirmação:

- **Basic Ack**: O consumidor envia uma confirmação de que recebeu e processou a mensagem com sucesso. A mensagem é removida da fila.
- **Basic Reject**: Se o consumidor encontra algum problema ao processar a mensagem, ele pode enviar um reject, fazendo com que a mensagem retorne à fila.
- **Basic Nack (Negative Acknowledgement)**: Similar ao Basic Reject, o Nack também permite ao consumidor rejeitar a mensagem, mas pode rejeitar várias mensagens de uma vez, diferentemente do Reject, que rejeita uma mensagem de cada vez.

O processo de confirmação do consumidor garante que as mensagens sejam processadas e gerenciadas adequadamente, impedindo a perda de informações importantes.

#### **Publisher Confirm (Confirmação do Publicador)**

No mecanismo de Publisher Confirm, o publicador envia mensagens para o RabbitMQ com um ID associado. A Exchange confirma o recebimento da mensagem com sucesso, e o publicador recebe essa confirmação. No entanto, se ocorrer algum problema na Exchange, um Nack (Negative Acknowledgement) será emitido, indicando que a mensagem não foi entregue com sucesso. Nesse caso, o publicador deve tomar medidas para garantir a entrega bem-sucedida da mensagem.