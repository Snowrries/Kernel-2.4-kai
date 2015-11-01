# Kernel-2.4-kai
MLFQ
Multi Level Feedback Queue.

Assignment specifications: 
4.1 Algorithm Statement
Multiple FIFO queues are used and the operation is as follows:
  1. A new task is inserted at the end (tail) of the top-level FIFO queue.
  2. At some stage the task reaches the head of the queue and is assigned the CPU.
  3. If the task is completed within the time quantum of the given queue, it leaves the system.
  4. If the task voluntarily relinquishes control of the CPU, it leaves the queuing network, and when the task
    becomes ready again it is inserted at the tail of the same queue which it relinquished earlier.
  5. If the task uses all the quantum time, it is pre-empted and inserted at the end of the next lower level queue.
    This next lower level queue will have a time quantum which is more than that of the previous higher level
    queue.
  6. This scheme will continue until the task completes or it reaches the base level queue.
• At the base level queue the tasks circulate in round robin fashion until they complete and leave the system.
  Tasks in the base level queue can also be scheduled on a first come first served basis.
• If a task blocks for I/O, it is “promoted” one level, and placed at the end of the next-higher queue. This
  allows I/O bound tasks to be favored by the scheduler and allows tasks to “escape” the base level queue.
  For scheduling, the scheduler always start picking up tasks from the head of the highest level queue. If the highest
  level queue has become empty, then only will the scheduler take up a task from the next lower level queue. The same
  policy is implemented for picking up in the subsequent lower level queues. Meanwhile, if a task comes into any of
  the higher level queues, it will preempt a task in the lower level queue.

Also, a new task is always inserted at the tail of the top level queue with the assumption that it would be a
short time consuming task. Long tasks will automatically sink to lower level queues based on their time consumption
and interactivity level. In the multilevel feedback queue, a task is given just one chance to complete at a given queue
level before it is forced down to a lower level queue.

4.2 Scheduler Parameters

Two major sets of parameters that you must have to implement your scheduler are number of queues and the
quantum for each queue. You should assume 256 queues. To select the quantum sizes, select a reasonable quantum
for your base queue (e.g. 10ms), and logarithmically increase the quantum for each queue.
