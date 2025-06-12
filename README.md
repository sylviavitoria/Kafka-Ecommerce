# Projeto ecommerce-kafka

## 📄 Descrição

Este é um projeto de e-commerce que utiliza **Apache Kafka** para processamento de eventos distribuídos. O sistema permite a criação de pedidos e o envio de e-mails de confirmação, além de detectar possíveis fraudes de forma assíncrona.

---

## 🧱 Estrutura do Projeto

O projeto é estruturado em vários serviços que se comunicam através de tópicos do Kafka:

* **NewOrderMain**: Cria novos pedidos e envia eventos para processamento.
* **EmailService**: Serviço que processa eventos de e-mail.
* **FraudDetectorService**: Serviço que analisa pedidos para detecção de fraudes.
* **LogService**: Serviço que registra todos os eventos do sistema.

---

## 🔧 Componentes Principais

### `KafkaDispatcher`

Responsável pelo envio de mensagens para tópicos do Kafka.

### `KafkaService`

Abstração para consumo de mensagens dos tópicos do Kafka.

### `ConsumerFunction`

Interface para processamento de mensagens consumidas.

---

## 💻 Executando via Git Bash + IntelliJ IDEA

### 1. Inicie o Zookeeper:

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

**O que faz:**
O Apache Kafka depende do **Zookeeper** para gerenciar seu cluster. Este comando inicia o serviço do Zookeeper, necessário antes de iniciar o Kafka.
🔹 **Executar em um terminal separado**, pois ele permanece ativo durante toda a execução.

---

### 2. Em outro terminal, inicie o Kafka:

```bash
bin/kafka-server-start.sh config/server.properties
```

**O que faz:**
Este comando inicializa o **servidor Kafka** com base nas configurações definidas no arquivo `server.properties`. Kafka só pode ser iniciado **depois** que o Zookeeper estiver em execução.

---

### 3. (Opcional) Em um outro terminal, visualize mensagens no tópico `ECOMMERCE_NEW_ORDER`:

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic ECOMMERCE_NEW_ORDER --from-beginning
```

**O que faz:**
Este comando permite visualizar, no terminal, as mensagens publicadas no tópico `ECOMMERCE_NEW_ORDER`.

* A opção `--from-beginning` garante que **todas as mensagens anteriores** sejam exibidas desde o início do tópico.
  ✅ Útil para debug e validação.

---

### 4. Rode a aplicação na IntelliJ IDEA:

**O que fazer:**
Abra o projeto no IntelliJ IDEA e execute as seguintes classes **nessa ordem**, para garantir o funcionamento correto do sistema:

1. **`EmailService`**: escuta o tópico de envio de e-mails e simula o envio com base no pedido criado.
2. **`FraudDetectorService`**: consome eventos de novos pedidos e analisa possíveis fraudes.
3. **`LogService`**: escuta todos os tópicos do sistema e registra os eventos recebidos, funcionando como um monitoramento.
4. **`NewOrderMain`**: envia eventos de criação de pedidos e solicita o envio de e-mails, simulando uma nova compra no e-commerce.

> 💡 **Importante:** Esses serviços devem estar em execução **antes** do `NewOrderMain`, para que consigam processar os eventos emitidos.

---

