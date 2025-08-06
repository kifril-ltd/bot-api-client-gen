# Bot API Client

A Go client library for interacting with the Message Gateway Bot API. This client provides a comprehensive set of methods for managing bots, channels, chats, dialogs, messages, and more.

## Features

- **REST API Client**: Generated from OpenAPI specification for type-safe API interactions
- **WebSocket Support**: Real-time communication with the messaging platform
- **Comprehensive API Coverage**: Methods for all Bot API operations including:
  - Bot management
  - Channel management
  - Chat and dialog operations
  - Message sending and receiving
  - File uploads and management
  - User and member management
  - Command handling

## Installation

```bash
go get github.com/kifril-ltd/bot-api-client-gen
```

## Usage

### Initializing the Client

```go
package main

import (
    "context"
    "log"

    "github.com/kifril-ltd/bot-api-client-gen"
)

func main() {
    // Create a new client with the server URL
    client, err := bot_api_client.NewClientWithResponses("https://api.example.com")
    if err != nil {
        log.Fatalf("Error creating client: %v", err)
    }

    // Use the client to make API calls
    // ...
}
```

### REST API Examples

#### Listing Bots

```go
ctx := context.Background()
resp, err := client.ListBotsWithResponse(ctx, nil)
if err != nil {
    log.Fatalf("Error listing bots: %v", err)
}

if resp.JSON200 != nil {
    for _, bot := range resp.JSON200.Bots {
        log.Printf("Bot: %s", bot.Name)
    }
}
```

#### Sending a Message

```go
ctx := context.Background()
message := bot_api_client.SendMessageJSONRequestBody{
    ChatID: "chat123",
    Type:   "text",
    Content: bot_api_client.MessageContent{
        Text: "Hello, world!",
    },
}

resp, err := client.SendMessageWithResponse(ctx, message)
if err != nil {
    log.Fatalf("Error sending message: %v", err)
}

if resp.JSON200 != nil {
    log.Printf("Message sent with ID: %s", resp.JSON200.MessageID)
}
```

### WebSocket Support

The client also provides WebSocket support for real-time communication:

```go
package main

import (
    "context"
    "log"

    "github.com/kifril-ltd/bot-api-client-gen"
    "github.com/kifril-ltd/bot-api-client-gen/ws"
)

func main() {
    // Create a WebSocket controller
    controller := ws.NewController("wss://api.example.com/ws", "your-token")

    // Set up event handlers
    controller.OnMessageReceived(func(msg ws.MessageReceivedEvent) {
        log.Printf("Received message: %s", msg.Content.Text)
    })

    // Connect to the WebSocket server
    if err := controller.Connect(context.Background()); err != nil {
        log.Fatalf("Error connecting to WebSocket: %v", err)
    }

    // Keep the connection alive
    select {}
}
```

## Error Handling

The client provides detailed error responses that can be handled as follows:

```go
resp, err := client.SendMessageWithResponse(ctx, message)
if err != nil {
    log.Fatalf("Error sending message: %v", err)
}

if resp.JSONDefault != nil {
    log.Printf("API Error: %s - %s", resp.JSONDefault.Code, resp.JSONDefault.Message)
}
```

## Custom Time Handling

The client includes custom time handling for API requests and responses:

- `DateTimeRFC3339`: Standard RFC3339 format
- `DateTimeRFC3339Micro`: Extended format with microsecond precision

## License

This library is distributed under the terms of the license agreement provided with the software.
