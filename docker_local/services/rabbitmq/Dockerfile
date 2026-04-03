# Start from the official RabbitMQ image with management UI
FROM rabbitmq:3.13-management

# Set RabbitMQ version (must match the base image tag, e.g. 3.13.x)
ENV RABBITMQ_VERSION=3.13.0

# Download and install the delayed message exchange plugin
RUN apt-get update && apt-get install -y curl \
    && curl -L -o /opt/rabbitmq/plugins/rabbitmq_delayed_message_exchange-${RABBITMQ_VERSION}.ez \
       https://github.com/rabbitmq/rabbitmq-delayed-message-exchange/releases/download/v${RABBITMQ_VERSION}/rabbitmq_delayed_message_exchange-${RABBITMQ_VERSION}.ez \
    && rabbitmq-plugins enable --offline rabbitmq_delayed_message_exchange \
    && apt-get remove -y curl \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/*