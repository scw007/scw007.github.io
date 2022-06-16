# Closure Fun in Go

Riddle me this, what does the following code print? 

```golang
package main

import "fmt"

type button struct {
	OnClick func()
}

func main() {
	// create some buttons
	buttonTexts := []string{"one", "two", "three"}
	buttons := []button{}
	for _, val := range buttonTexts {
		but := button{
			OnClick: func() {
				fmt.Println(val)
			},
		}
		buttons = append(buttons, but)
	}

	// click the buttons as a test
	for _, but := range buttons {
		but.OnClick()
	}

}
```

<details>
    <summary>Answer:</summary>

    ```
    three
    three
    three
    ```

</details>

I knew that closures had to keep a reference of the variables, so that when a variable's value outside the closure changes, the inner function would have the new value. For example:

```golang
package main

import "fmt"

func main() {
	one := 1
	anon := func() {
		fmt.Println(one)
	}
	one = 2

	anon()
}
```

Prints

```
2
```

What I didn't know, was that when you use a range loop, the address of the iterated variable *stays the same*. Make this change to the original code:

```golang
	for _, val := range buttonTexts {
		fmt.Println("in", val, &val)
		but := button{

			OnClick: func() {
				fmt.Println("out", val, &val)
			},
		}
		buttons = append(buttons, but)
	}
```

And you should get:

```
in one 0xc00009e210
in two 0xc00009e210
in three 0xc00009e210
out three 0xc00009e210
out three 0xc00009e210
out three 0xc00009e210
```

I've found the easiest way to get around this problem is to use another closure. Here's my final code and output.

```golang
package main

import "fmt"

type button struct {
	OnClick func()
}

func main() {
	// create some buttons
	buttonTexts := []string{"one", "two", "three"}
	buttons := []button{}
	for _, val := range buttonTexts {
		fmt.Println("in", val, &val)
		anonFunc := func(anonVal string) func() {
			return func() {
				fmt.Println("out", anonVal, &anonVal)
			}
		}(val)
		but := button{
			OnClick: anonFunc,
		}
		buttons = append(buttons, but)
	}

	// click the buttons as a test
	for _, but := range buttons {
		but.OnClick()
	}
```

```
in one 0xc00009e210
in two 0xc00009e210
in three 0xc00009e210
out one 0xc00009e230
out two 0xc00009e260
out three 0xc00009e2a0
```

Hope this saves you the hours of headache that I experienced!
