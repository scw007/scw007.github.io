# Pseudo Overloading in Go
Ever wish you could write overloading functions in Go? Well, you can (sorta). I wanted to write this because it was not very obvious to me.

If you don't know about overloading, the idea is that you write one function that takes different parameters. In Java, for example, you write multiple functions with the different supported sets of parameters.

```java
    public void printType(int x)
    {
        System.out.println("integer")
    }

    public int printType(string x)
    {
        System.out.println("string")
    }
```

Overloading is not officially supported by Go. However, there's a pattern you can use in your code to emulate overloading. All you need is `interface`.

```golang
package main

import "fmt"

func printType(in interface{}) {
	switch in.(type) {
	case int:
		fmt.Println("integer")
	case string:
		fmt.Println("string")
	}
}

func main() {
	printType(1)
	printType("stringy")
}
```

And the output is...
```
integer
string
```

In fact, this is already in use by the standard library:

```golang
func Println(a ...interface{}) (n int, err error)
```

Notice that this also works for multiple parameters, aka variadic functions, aka the `...`.

I ran into an interesting use case recently when trying to write a bot for discord. [discordgo](https://github.com/bwmarrin/discordgo) has an example of this using event handler functions.

```golang
...
    discord.AddHandler(onJoinVoice)
    discord.AddHandler(onSendText)
...

func onJoinVoice(s *discordgo.Session, m *discordgo.VoiceStateUpdate) {
..
func onSendText(s *discordgo.Session, m *discordgo.MessageCreate) {
...
```

You can see how this is implemented [here](https://github.com/bwmarrin/discordgo/blob/v0.23.2/event.go#L120) and [here](https://github.com/bwmarrin/discordgo/blob/v0.23.2/eventhandlers.go#L916).

If you use this pattern, I'd suggest writing good documentation as to what types you support.
