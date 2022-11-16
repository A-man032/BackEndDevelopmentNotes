# Chrome

## Browser Architecture

**Single process model (单进程模型)**: one process with many different threads.

**Multi-process model (多进程模型)**: many different processes with a few threads communicating over IPC (Inter Process Communication).

![image-20221111113559251](.\png\image-20221111113559251.png)

**Chrome's multi-process architecture**

![image-20221111113917048](.\png\image-20221111113917048.png)

**Browser process** coordinates with other processes that take care of different parts of the application, including address bar, bookmarks, back and forward button. It also handles the invisible, privileged parts of  a web browser such as network requests and file access.

For **renderer process**, multiple process are created and assigned to each tab.

**Plugin process** controls any plugins used in website, for example, flash.

**GPU process** handles GPU tasks in isolation from other process.

![image-20221111132432151](.\png\image-20221111132432151.png)

## The benefit of multi-process architecture in Chrome

### Significantly improves fault tolerance.

A simple scenario, there are 3 tabs open and each tab is run by an independent renderer process. If one tab become unresponsive, then you can close it and move on while keep other tabs alive.  If all tabs are running on one process, when one tab become unresponsive, all the tabs are unresponsive.

### Security and sandboxing

Since operating systems provide a way to restrict processes' privileges, the browser can sandbox certain processes from certain features.

### Shortcoming

Multi-process model will **take up more memory**. 

More memory usage as they can't be shared the way they would be if they were threads inside the same process. Because processes have their own private memory space, they often contains copies of common infrastructure. 

In order to save memory, Chrome puts a limit on how many processes it can spin up depending on how much memory and CPU power your device has. **When Chrome hits the limit, it starts to run multiple tabs from the same site in one process.**

## Saving more memory

Chrome runs each part of the browser program as a **service** allowing to easily split into different processes or aggregate into one.

The benefit of service is that 