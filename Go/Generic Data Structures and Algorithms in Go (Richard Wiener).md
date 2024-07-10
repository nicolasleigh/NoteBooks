### CH1

Go applications compile to a binary executable so they can be distributed to a customer without having to package an interpreter and runtime libraries as is the case with Python and other interpreted languages.

Go introduced a lightweight process called a goroutine that requires less memory overhead than a thread. Goroutines are functions that run concurrent with other functions. When a regular function is invoked, the code below the function gets executed after the function completes its work. When a goroutine function is invoked, the code directly below it gets executed immediately since the goroutine runs concurrently with code beneath it.

We often want to be able to synchronize the sequence of goroutines and have them communicate with each other. We introduce the powerful construct of the **channel** to accomplish this.

Channel direction can be added to a goroutine signature. An arrow pointing to the **chan** from the right, requires the goroutine to assign to the channel (a generator). An arrow to the left of **chan** and pointing to the channel variable requires the goroutine to only consume values in the channel.

If an attempt is made to send information to the channel when it is specified as a consumer, or if an attempt is made to access information from the channel in the case that it is specified as a generator, a compiler error will occur.

A pervasive problem using concurrency is **race condition**. This problem occurs when two or more goroutines modify the same shared data.

We can correct the race-condition problem by using a mutex. This locks the global countValue while each goroutine modifies its value and protects this shared data from being corrupted.

The code m.Lock() within each goroutine protects the global countValue from modification outside of the goroutine in which it is invoked. No other goroutine can change countValue until the m.Unlock() is invoked.

Program execution is slowed down using the mutex, but the program is protected from the race condition.
