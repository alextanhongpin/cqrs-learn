## Event Manager with Golang

Sample event manager with golang. The responsibility of the event manager is to just publish the events from command handlers.
```go
package main

import (
	"fmt"
	"time"

	"play.ground/event"
)

func NewGreetEvent() event.Payload {
	return event.Payload{
		"msg": "hello world",
	}
}

func main() {
	mgr, cancel := event.NewEventManager(10)
	defer cancel()

	for i := 0; i < 10; i++ {
		ok := mgr.Send(event.Event{
			ID:        "1",
			Name:      "hello",
			Timestamp: time.Now().UnixNano(),
			Payload:   NewGreetEvent(),
		})
		if i == 5 {
			cancel()
		}
		fmt.Printf("closed: %t\n", ok)
	}
}
-- go.mod --
module play.ground
-- event/event.go --
package event

import (
	"fmt"
	"sync"
	"time"
)

type Payload map[string]interface{}

type Event struct {
	Name      string
	ID        string
	Timestamp int64
	// AggregateID
	// AggregateType
	// EventID
	// EventType
	// Context
	Payload Payload
}

func (evt Event) Hash() string {
	return fmt.Sprintf("%s:%s", evt.ID, evt.Name)
}

type EventManager struct {
	sync.RWMutex
	ch    chan Event
	cache map[interface{}]bool
}

func NewEventManager(n int) (*EventManager, func()) {
	evtmgr := &EventManager{
		ch:    make(chan Event, n),
		cache: make(map[interface{}]bool),
		// Message queue.
	}
	cancel := evtmgr.loop()
	return evtmgr, cancel
}

func (e *EventManager) loop() func() {
	var once sync.Once
	var wg sync.WaitGroup
	wg.Add(1)
	go func() {
		defer wg.Done()
		fmt.Println("initialized")
		for {
			select {
			case msg, ok := <-e.ch:
				if !ok {
					return
				}
				// Fake work.
				// time.Sleep(1 * time.Second)
				// Sends to the message queue.

				_, exist := e.cache[msg.Hash()]
				if exist {
					fmt.Printf("duplicate: %+v\n", msg)
					continue
				}
				// Add to cache.
				e.cache[msg.Hash()] = true
				fmt.Printf("msg: %+v\n", msg)
			}
		}
	}()
	return func() {
		once.Do(func() {
			close(e.ch)
			wg.Wait()
			fmt.Println("shutdown")
		})
	}
}

func (e *EventManager) Send(evt Event) (closed bool) {
	defer func() {
		if err := recover(); err != nil {
			closed = true
		}
	}()
	// Ensures that the channels are not blocked.
	select {
	case e.ch <- evt:
		closed = false
	case <-time.After(5 * time.Second):
		closed = false
	}
	return
}
```
