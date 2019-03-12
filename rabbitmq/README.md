### RABBITMQ Source

# Requis
This source expects rabbitmq to be running on the same k8s cluster. If you look at the source.yaml file, you'll see that you can pass environnement variable to the `Container Source` : 
- `RABBITMQ_USER`           --> Rabbitmq user for login (can also be pass as a secret)
- `RABBITMQ_PASSWORD`       --> Rabbitmq password for login (can also be pass as a secret)
- `RABBITMQ_PORT`           --> Rabbitmq port for the brocker (default `:5672`)
- `CONTAINER_SOURCE_NAME`   --> Container name (optional)
- `DEBUG`                   --> If true every action will be log (optional)
- `EXCHANGE_NAME`           --> RabbitMq Exchange name (optional)

# Message structure form producer
Message strucutre in the queue have to follow this specification :
- The Exchange Name have to be the same as `EXCHANGE_NAME` environnement variable
- The RoutingKey as to be format like this : `FUNCTION_NAME.NAMESPACE.function`
    - You can also add `.critical` if you don't when to lose data on error. This will look like this :  `FUNCTION_NAME.NAMESPACE..critical.function`
- I you need a response pass a call back url to `ReplyTo`. You will also need to pass a `CorrelationId`.
- You can pass any body

# Message structure to the function
Message send to you knative function will be done with http. This is the POST structure that will be passing to you function : 
````
    {
        owner          : "producer"  # Producer name
        body           : {}          # Message Body
        critical       : false       # true if the message is critical and you need to have a fall back on error
        correlationId  : 19190393    # ID of the message
    }
````
Your function need to expose a server endpoint on 8080 port to be able to have the data.

# Usage
- Deploy RabbitMq brocker to your k8s cluster

- Create Container source with `kubectl apply -f source.yaml`

# TODO 
  - Support HTTPS
  - Better support for dead letter queue