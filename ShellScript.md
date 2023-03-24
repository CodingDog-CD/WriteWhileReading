# Note Shell Script

## Flow Control

### Until
```
until [condition] 
do
  [command]
done

```
The code snippet shows a general use case of **until**. </br>
In each loop, the [condition] will be evaluated. if the condition is false, command is going to be carried out. </br>
This process repeats until the [condition] becomes true. Then the program jumps out of the until loop and carry out subsequent commands. </br>
Normally, there should be instruction within the loop body to control the condition. Otherwise the until loop could become a permanent loop. Of course, there could be external event influencing the condition. E.g. a server needs some time to startup and become reachable.


