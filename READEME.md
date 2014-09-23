VM
====
 
This vm will be inside a OS process. It will contains
* scheduler
* a runing queue of task
 
Yes, that's it. 
 
 
for scheduler

* a queue container for task
* execute task in from the queue
* IO watcher
* timer
* interruots
It will be a co-op multitasking, which means applications that voluntarily ceded time to one another.
(Like motohawk and eLDS system, because the application is carefully design for specific hardware, and each code path is review, the response time is more reliablable)
but if a task is taking too long, then OS is forcing it to yield
 
 
for the running queue

* dequeue task
* run the task
  * if END or ERR, task is removed, or run the task and then put at the end of the queue
** if IO/ timer calls, call the host kernel, and put the task in the end of queue with a ID? so callback to set this task with ready state
** then fetch next task in the queue
 
 
for the task
task is a function, which is a stack of
 
* Function itself, it is sort of string constants for instruction
* Programe counter-- offset of the prototype
* registry...local variables
* a reference to the function prototype
 
Instrcution and operantion
==========================
instruction = operation + [0-3] operands

OP ABC

```
0         5 | 6          13 | 14           22 | 23            31   Bit
------------|---------------|-----------------|-----------------
     OP     |       A       |        B        |        C           Field
------------|---------------|-----------------------------------
     6              8                9                 9           Bits
  [0..63]        [0..255]         [0..511]          [0..511]       Range
```


OP ABx — Operation OP with operands A and Bs|Bu:

``` 
0         5 | 6          13 | 14                              31   Bit
------------|---------------|-----------------------------------
     OP     |       A       |              Bs/Bu                   Field
------------|---------------|-----------------------------------
     6              8                        18                    Bits
  [0..63]        [0..255]             Bu: [0..262143]              Range
                                      Bu: [-131071..131072]
```
 
OP Bxx — Operation OP with operand Bss|Buu:

``` 
0         5 | 6                                               31   Bit
------------|---------------------------------------------------
     OP     |                     Bss/Buu                          Field
------------|---------------------------------------------------
     6                               26                            Bits
  [0..63]                   Buu: [0..67108863]                     Range
                            Bss: [-33554431..33554432]
```
 
 
 
Yield:
this op need able to pause any task and resume at that state
 
* yielding for other tasks, so they could be run or scheduled from IO events
* or yielding for a time to expire