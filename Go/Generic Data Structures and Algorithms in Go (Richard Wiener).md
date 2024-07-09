### CH1

Go applications compile to a binary executable so they can be distributed to a customer without having to package an interpreter and runtime libraries as is the case with Python and other interpreted languages.

Go introduced a lightweight process called a goroutine that requires less memory overhead than a thread. Goroutines are functions that run concurrent with other functions. When a regular function is invoked, the code below the function gets executed after the function completes its work. When a goroutine function is invoked, the code directly below it gets executed immediately since the goroutine runs concurrently with code beneath it.

We often want to be able to synchronize the sequence of goroutines and have them communicate with each other. We introduce the powerful construct of the **channel** to accomplish this.
