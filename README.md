Response Time
===================

## Outline
- What is response time?
- Medians are stoopid.
- We care about the 9s.
- Not on solid footing.
- Is it good enough?
- Appendix: Coordinated Ommission (or response time is hard).

### What is response time?

When talking about system performance people confuse several related concepts.  The most commonly confused concepts are response time (or latency) with throughput.  Throughput measures the number of events a system can handle in a specified time window.  Latency measures the amount of time a single event takes from start to finish.  Different systems have different characteristics about the relationship between latency and throughput.  For instance, in a system that receives an event a minute:

- System A handles each event as it arrives and takes 1 minute per event.
- System B batches each event once per hour, but can process 60 events per minute in a batch.

These 2 systems have the same throughput for a given hour, but System B exhibits much slower response time for the vast majority of its events.

Another common fallacy is conflating the latency of a system with its ability to handle load.  Load may impact the latency of the system, but typically it is not in a linear fashion.  That is, in most systems the latency of the system stays stable as load is added until the capacity of the system is reached, at which point the system dramatically degrades (often to failure).

### Medians are stoopid.

Throughput is an easy number to talk about and visulaize.  70K RPS (requests per second) or 250 RPM (requests per minute) clearly state the throughput of the system and allow for planning around.  The only variable is the time measurement and the only complexity comes when changing the time measurement does not linearly change the throughput (ie batching effects).

Unfortunately, response times do not have such an easy number.  Unlike throughput where the number over a time period is the measurement, latency is measured as a collection of numbers over a time period.  Frequently these collections of numbers can be very big (for instance a 70k RPS system would have over 4 million latency measurements for an hour).  These very big numbers get difficult to reason about and therefore people use aggregation techniques that often obscure the true response time situation.

The worst of these aggregation techniques is reporting on response time using the median response time of the period.  The median response time only tells you what the *slowest* response time was, for the **fastest** half of your responses.  It tells you literally nothing about the **slowest** half the responses, which are typically the ones that cause problems. For instance if your response times in milliseconds were 1,1,1,1,1,1,1...(1000 more 1s)...,1,1,2,60000 the median response time would be 1 millisecond without any indication that there is a one minute response time in the period.

The typical response to this is to then use the mean latency, but in our example set above that number is 60 milliseconds, which is significantly different.

### We care about the 9s.

### Not on solid footing.

### Is it good enough?

### Appendix: Coordinated Omission (or response time is hard).
