# Reliable, Scalable, and Maintainable Applications

## Thinking About Data Systems

### Reliability

The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or soft‐ware faults, and even human error).

### Scalability

As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.

### Maintainability

Over time, many different people will work on the system (engineering and operations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively.

## Reliability

Reliability means making systems work correctly, even when faults occur. Faults can be in hardware (typically random and uncorrelated), software (bugs are typically systematic and hard to deal with), and humans (who inevitably make mistakes from time to time). Fault-tolerance techniques can hide certain types of faults from the end user.

## Scalability

### Describing Load

Load can be described with a few numbers which we call **load parameters**. The best choice of parameters depends on the architecture of your system: it may be requests per second to a web server, the read/write ratio in a database, the number of simultaneously active users in a chat room, the hit rate on a cache, or something else. Perhaps the average case is what matters for you, or perhaps your bottleneck is dominated by a small number of extreme cases.

### Describing Performance

In a batch processing system such as Hadoop, we usually care about **throughput**—the number of records we can process per second, or the total time it takes to run a job on a dataset of a certain size. In online systems, what’s usually more important is the service’s **response time**—that is, the time between a client sending a request and receiving a response.

In practice, in a system handling a variety of requests, the response time can vary a lot. We therefore need to think of response time not as a single number, but as a **distribution** of values that you can measure.

## Maintainability

### Three design principles for software systems:

1. Operability: Make it easy for operations teams to keep the system running smoothly.

2. Simplicity: Make it easy for new engineers to understand the system, by removing as much complexity as possible from the system. (Note this is not the same as simplicity of the user interface.)

3. Evolvability: Make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. Also known as extensibility, modifiability, or plasticity.
