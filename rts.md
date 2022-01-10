# Implementing an Real Time Strategy (RTS) game in Go
I've been doing a lot of game development in Go, and I'd like to share how I've been implementing an RTS game using https://ebiten.org/.

## Networking Overview
I think its important to start with the networking aspect of any multiplayer game. In my experience, it is harder to implement multiplayer in an existing game than to do it initially.

Here's an overview of how my initial client-server networking model works:

1. Once per frame, all player clients send their game inputs to the server.
2. On one server thread per client, put the inputs on an input queue.
3. In a single server thread that executes at a specific rate:  
    1. Process the input queue.
    2. Update the gamestate.
    3. Send the gamestate to all the players
4. In a client thread, all players receive the gamestate.
5. In another thread, continually draw the gamestate.

Because the server is the authority on the game state, cheating is reduced dramatically. Clients can really only cheat by sending inputs or by parsing the gamestate. For instance, a cheater could make a hack that automatically sends inputs to target an enemy based on the latest gamestate faster than a regular player. The advantage is pretty minimal here. And because the client only sends inputs and draws the gamestate, the client is "dumb" and simple.

One problem of this simple approach is that there is latency between the player's inputs and when the results are drawn on screen. However, because this is an RTS, you don't really "control" your character, you make orders for your units to follow. It is harder to perceive latency here as compared to a platformer or an FPS.

### Sending data
How should the client communicate with the server and vice versa? You basically have to make a decision between TCP and UDP. TCP has a lot overhead for a variety of reasons, such as guaranteed delivery and resending of packets. However, our data is time sensitive. Why resend gamestate that is already too old to use? UDP is much faster. However, UDP is connectionless and does not guarantee delivery of a packet. So even if you go for UDP, you will still need some sort of system to acknowledge that a particular packet has been received.

Also, no matter which protocal you chose, using `recv` directly requires you to write your own messaging protocal, usually by reserving the first couple bytes of data for the message payload size, and then reading that number of bytes from the socket.

Well, there is the option of using a higher level interface for networking to deal with those sorts of issues. WebSockets use TCP, and WebRTC uses UDP. These technologies take care of a lot of these lower issues for you.

In the end, I ended up going with WebSockets using https://github.com/nhooyr/websocket for the following reasons:
1) KISS (Keep It Simple, Stupid). nhooyr's websocket library is ridiculously easy and simple to use.
2) I don't have to deal with lower level TCP/UDP socket issues.
3) I haven't had any perceivable latency issues using websockets.
4) The library works with Web Assembly. Easy way to run the game in the browser.

If I run into any issues regarding latency, I'll consider a switch to WebRTC, and if that doesn't work out, to UDP.

It's pretty simple to send and receive payloads with websockets. Here's my generic message sending and receiving code:
```golang
package network

import (
	"context"
	"encoding/json"
	"errors"

	"nhooyr.io/websocket"
)

type MessageType int

const (
    Inputs MessageType = iota
    Gamestate
)

type Message struct {
	Type    MessageType
	Payload []byte
}

func Read(ws *websocket.Conn) (Message, error) {
	_, payload, err := ws.Read(context.Background())
	if err != nil {
		return Message{}, err
	}
	var msg Message
	err = json.Unmarshal(payload, &msg)
	return msg, err
}


func Write(ws *websocket.Conn, msg Message) error {
	payload, _ := json.Marshal(msg)
	return ws.Write(context.Background(), websocket.MessageText, payload)
}
```
Eventually, I will also integrate compression and decompression to these functions.

Notice the `Payload` is of type `[]byte`. This way, I can store anything in a `Message`, and then when reading, I can use the `Type` to know how to read it. Normally, it's just json marshalled.

