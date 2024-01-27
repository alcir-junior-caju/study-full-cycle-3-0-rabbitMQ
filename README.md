# Curso Full Cycle 3.0 - Módulo RabbitMQ

<div>
    <img alt="Criado por Alcir Junior [Caju]" src="https://img.shields.io/badge/criado%20por-Alcir Junior [Caju]-%23f08700">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-%23f08700">
</div>

---

## Descrição

O Curso Full Cycle é uma formação completa para fazer com que pessoas desenvolvedoras sejam capazes de trabalhar em projetos expressivos sendo capazes de desenvolver aplicações de grande porte utilizando de boas práticas de desenvolvimento.

---

## Repositório Pai
https://github.com/alcir-junior-caju/study-full-cycle-3-0

---

## Visualizar o projeto na IDE:

Para quem quiser visualizar o projeto na IDE clique no teclado a tecla `ponto`, esse recurso do GitHub é bem bacana

---

### O que é RabbitMQ
- Message Broker;
- Implementa AMQP, MQTT, STOMP e HTTP;
- Desenvolvido em Erlang;
- Desacoplamento entre serviços;
- Rápido e Poderoso;
- Padrão de mercado;

### Como funciona
- Client;
- Server;
    - TCP: Abre apenas uma única conexão persistente;
        - Channel A - Multiplexing connections / 1 Thread por channel;
        - Channel B - Multiplexing connections / 1 Thread por channel;
- Publisher;
    - Exchange;
        - Write Queue;
- Consumer;
    - Read Queue;
- Exchange types:
    - Direct;
        - Producer;
            - Exchange;
                - Bind - Routing key;
                    - Queue - Routing key X;
                    - Queue - Routing key Y;
                    - Queue - Routing key Z;
    - Fanout;
        - Mesma estrutura mas sem Routing key, que envia para todas filas;
    - Topic;
        - Mesma estrutura mas com regras para cada routing key, ex.: X.*, *.Y.*, *.Z;
    - Headers;

### Queues
- FIFO - First In, First Out;
- Propriedades:
    - Durable: Se ela deve ser salva mesmo depois do restart do broker;
    - Auto-delete: Removida automaticamente quando o consumer se desconecta;
    - Expiry: Define o tempo que não há mensagens ou clientes consumindo;
    - Message TTL: Tempo de vida da mensagem;
    - Overflow: Quando a fila transborda;
        - Drop head - remove a última;
        - Reject publish - não publica na fila;
    - Exclusive: Somente channel que criou pode acessar;
    - Max length ou bytes: Quantidade de mensagens, o tamanho em bytes máximo permitido;
        - Caso aconteça, teremos um overflow e podemos escolher em remover as mensagens mais antigas ou rejeitar a nova;

### Dead letter queues
- Algumas mensagens não conseguem ser entregues por qualquer motivo;
- São encaminhadas para uma Exchange específica que roteia as mensagens para uma dead letter queue;
- Tais mensagens podem ser consumidas e averiguadas posteriormente;

### Lazy Queues
- Mensagens são armazendas em disco;
- Exige alto I/O;
- Quando há milhões de mensagens em uma fila, por qualquer motivo, há a possibilidade de liberar a memória, jogando especificamente as mensagens da fila em questão em disco;

### Simulador de filas
[tryrabbitmq.com](https://tryrabbitmq.com/)

### Confiabilidade
- Como garantir que as mensagens não serão perdidas no meio do caminho?
- Como garantir que as mensagens puderam ser processadas corretamente pelos consumidores;
- Recursos do RabbitMQ pensados para resolver tais situações
    - Consumer acknowledgement;
        - Basic.Ack - Toda vez que você manda uma mensagem para um consumer e o consumer da uma `ack` como resposta;
        - Basic.Reject - Por algum motivo o consumer não consumiu a mensagem e retornou um `reject` então devolve a mensagem;
        - Basic.Nack - Mesmo funcionamento do `reject` mas rejeita mais de uma mensagem ao mesmo tempo;
    - Publisher confirm;
    - Filas e mensagens duráveis / persistidas;

### Publish Confirms
- Publish envia a mensagem;
    - Mensagem com id (int)
        - Exchange recebe e retorna o `ack` com o `id` da mensagem;
        - Exchange não consegue receber a mensagem e retorna um `nack` com o `id` da mensagem;
