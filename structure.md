# Structure

How would the code structure look like for a CQRS-based implementation, versus the traditional web API/CRUD approach? The difference is outlined below. The events created could be send to a background task to be published to the message queue for clients to consume them. An orchestrator may listen to interesting events and carries out further actions, chaining the pipelining until a specific task is completed.

```go
package main

import (
	"context"
	"fmt"
)

func main() {
	fmt.Println("Hello, playground")
}

// The typical Service/UseCase may look like this.
// The request/response could be mapped as the API request/response directly.
type Service interface {
	GetBooks(ctx context.Context, req GetBooksRequest) (*GetBooksResponse, error)
}

// For CQRS-based naming, it might not differ that much except the naming.
// If we are serving the response back as API however, we need to map the response back to another struct/data transfer object (DTO).
type CommandHandler interface {
	RetrieveBookHandler(ctx context.Context, cmd RetrieveBooksCommand) (*BookRetrievedEvent, error)
}
```
