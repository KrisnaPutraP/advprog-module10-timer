# Tutorial 10 - Asynchronous Programming - Timer

## Understanding how it works

![img](/img/img.png)

Because an async function runs independently of the main execution flow, the main program doesn’t wait for the async task to finish. It means that even though the `println!()` statement is placed after the line `spawner.spawn( async { ... });`, the string "hey hey" appears before "howdy!". Since "hey hey" is printed in the main flow, it appears immediately. Meanwhile, the async task runs separately and prints "howdy!" later. 

## Multiple Spawn and removing drop

### Removing `drop(spawner)`

![img](/img/dropspawneroff.png)

Calling `drop(spawner)` signals that we’re done using the spawner. The spawner works like a message queue. Every time we spawn a task, it gets added to the queue. When the executor runs, it pulls tasks from this queue for execution. By dropping the spawner, we indicate that no more tasks will be added, allowing the executor to process the remaining ones and eventually finish, leading to the program’s termination.

![img](/img/dropspawneron.png)

The output order of "done2!" appearing after "done3!", even though their sequence in the code suggests otherwise, happens because the tasks run concurrently. Since all three tasks execute simultaneously without waiting for one another, their completion order isn’t guaranteed to match the order in the code.

Additionally, with `drop(spawner)` now uncommented, the program explicitly signals that no more tasks will be added to the queue. This allows the executor to wrap up the remaining tasks and finalize execution, ensuring the program can terminate properly.

>What is the effect of spawning?

Spawning allows tasks to be created and scheduled for execution without blocking the main program flow. When a task is spawned, it gets placed into a queue managed by the spawner, rather than executing immediately.

>What is the spawner for, what is the executor for, what is the drop for?

- Spawner: Acts like a task dispatcher, adding new tasks to the queue so they can be executed later.
- Executor: Pulls tasks from the queue and runs them, ensuring that spawned tasks actually get executed.
- Drop(spawner): Signals that no more tasks will be added to the queue, allowing the executor to process any remaining tasks and ultimately complete the program.

>What is the correlation of all of that?

The correlation between them is that the spawner adds tasks, the executor processes them, and drop(spawner) ensures the system knows when to stop accepting new tasks and wrap things up. Without dropping the spawner, the program may not finish as expected since the executor could still wait for more incoming tasks. 









