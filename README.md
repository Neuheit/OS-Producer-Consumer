# OS-Producer-Consumer
An extra credit assignment for my Operating Systems course. 

# Requirements
- Any Linux Distribution
- GCC

# Description
These programs use shared memory and semaphores to properly syncronize data to each other. The producer will first wait a random interval to create an item (incrementing the shared integer) and write it to the shared buffer. The consumer will also wait a random interval to read this shared integer, decrement it, and write it back to the shared buffer. It will repeat this 6 times. After this both programs will deallocate their shared memory and semaphores.

# Syncronization
To ensure that only 2 items exist in the buffer at a time, 3 semaphores are used: 

`fill`: Used to syncronize when a new "item" is added to the buffer. The producer signals this when it has added an item and the consumer waits for an item to be added. It defaults to 0 (non-signaled) so the producer can write first.

`avail`: Used to syncronize when an existing "item" is removed from the buffer. The consumer signals this when an item is removed, and the producer waits for the item to be removed. It defaults to 2 (signaled) so the producer can write first, and is able to remain signaled for 2 iterations (to represent the table limit).

`mutex`: Used for mutual exclusion for both decrementing in the consumer and incrementing in the consumer. This defaults to 1 (signaled) which ensures that only either the producer or the consumer is writing to the shared buffer.

# Usage

```
$ gcc producer.c -pthread -lrt -o producer
$ gcc consumer.c -pthread -lrt -o consumer
$ ./producer & ./consumer &
```

# Example Output

```
Producer ready to create 20 items.

Consumer ready to receive 20 items.

Item produced, there are now 1 item(s) in the table.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item produced, there are now 1 item(s) in the table.
Item consumed, 0 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item produced, there are now 2 item(s) in the table.
Item consumed, 1 remaining.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Item consumed, 0 remaining.
Item produced, there are now 1 item(s) in the table.
Producer cleaned up!
Item consumed, 0 remaining.
Consumer cleaned up!
```
