# Tutorial 10 - Asynchronous Programming - Timer

![img](/img/img.png)

Because an async function runs independently of the main execution flow, the main program doesn’t wait for the async task to finish. It means that even though the `println!()` statement is placed after the line `spawner.spawn( async { ... });`, the string "hey hey" appears before "howdy!". Since "hey hey" is printed in the main flow, it appears immediately. Meanwhile, the async task runs separately and prints "howdy!" later. 



