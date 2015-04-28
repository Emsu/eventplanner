eventplanner
============

Not sure whether you want to build an event based microservice system or a web API microservice system? Don't choose! Stay flexible.

`eventplanner` is a lightweight application wrapper that enables you to build your microservices as web APIs and convert those API microservices to event based microservices on demand (what Netflix has called 'orchestration' vs 'collaboration'). `eventplanner` is language agnostic and is configured through JSON. You can even run your microservice as both an API and an event based service at the same time! Simply expose both the `eventplanner` instance and the API server to other services.

## How?

Using a JSON configuration file to map. This allows you to separate the implementation

## Steps

- Build your microservices as standard web API using any language you want.
- Configure `eventplanner` to wrap your service.

## Configuration

Here are the configuration options:

{
	"transport": "kafka", // Message broker to use
	"brokerHost" : "localhost" // Host for the message broker
	"brokerPort" : 9092 // Port for message broker
	"host": "localhost", // Host pair for the wrapped application
	"port": 8000, // Port for the wrapped application
	"contentType": "" // (optional) Defaults to "application/json"
	"paths": [
        {
            "event": "", // My suggestion is to use something like "{{path}}.{{method}}" for easy portability back to HTTP requests
            "url": "", // URL of wrapped application to request
            "method": "", // Method to make the request with
            // The payload sent along with the event is transparently passed along with the configured content type.
        },
        // ... more path objects
	],
}

## Messaging Systems Supported

- Kafka
- RabbitMQ

Feel free to contribute to support more Messaging Queues. The implementation should be pretty simple.

## Service Implementation Suggestions

Your API will probably need to call other microservices either through sending events or HTTP requests. I'd recommend you use an event emitter abstraction and make your HTTP requests through an event abstraction. The event payload you send through your message broker should also map closely enough to a REST API call for you to convert your EventEmitter to HTTP calls or messages to a broker easily. For example:

    class EventEmitter(object):

        def emit(event, payload):
            """ Stub where you can easily convert emit between HTTP request and sending an event to a broker

            For example, you can convert a POST to '{SERVICE_URL}/user' to an event 'user.post' or vice versa
            """

In an `EventEmitter` like this one, you can easily switch implementations by modifying the EventEmitter class instead of maintaining it across the code base.

## TODOs

Reimplement in another language for performance.
