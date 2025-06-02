Open doc to dump notes, ideas, wip/omitted slides, etc.

---

<!-- This slide could fit into the Next on Harper or Working Directory sections. Pretty much anywhere we are talking about Harper's architecture  -->

# Parallel Computing in Node.js

## **Child Processes**
- Execute operations concurrently across multiple, distinct processes *separate from the main process*
- Processes can communicate via inter-process communication (IPC) channels
- Managed via `node:child_process` and `node:cluster` modules

## **Worker Threads**
- Execute operations concurrently across multiple threads *within the same process*
- Workers can share memory by transferring `ArrayBuffer` instances or using `SharedArrayBuffer`
- Managed via `node:worker_threads` module

---