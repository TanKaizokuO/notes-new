---
tags:
  - main
---
---

Async Python is a way to write code that _doesn’t block_. Instead of stopping everything while waiting for an operation to finish (like a web request, file read, or sleep), async Python lets other things run during the wait. This is especially useful for **I/O-bound** operations — things that are slow because of external resources (like the internet or disk), not your CPU.

---

# Async vs Threads vs Multiprocessing

| Feature     | `asyncio` (Async) | Threads               | Multiprocessing  |
| ----------- | ----------------- | --------------------- | ---------------- |
| Use Case    | I/O-bound tasks   | I/O-bound (sometimes) | CPU-bound tasks  |
| Concurrency | Cooperative       | Pre-emptive           | True parallelism |
| Overhead    | Low               | Medium                | High             |
| Complexity  | Medium            | Medium                | High             |
| GIL Aware   | Yes               | Yes                   | No               |

---

# Asynchronous Coding in Python

```python
# Let's define an async function
import asyncio

async def do_some_work():
	print("Starting work")
	await asyncio.sleep(1)
	print("Work complete")
	
do_some_work()
```

**OUTPUT :**  <coroutine object do_some_work at 0x756989c15a80>

```python
await do_some_work()
```
**OUTPUT :**  
```
Starting work
Work complete
```

> **Will this work?**
```python
async def do_a_lot_of_work():
	do_some_work()
	do_some_work()
	do_some_work()

await do_a_lot_of_work()
```
**OUTPUT :**  
```
/tmp/ipykernel_5952/1833959015.py:4: RuntimeWarning: coroutine 'do_some_work' was never awaited
  do_some_work()
RuntimeWarning: Enable tracemalloc to get the object allocation traceback
/tmp/ipykernel_5952/1833959015.py:5: RuntimeWarning: coroutine 'do_some_work' was never awaited
  do_some_work()
RuntimeWarning: Enable tracemalloc to get the object allocation traceback
/tmp/ipykernel_5952/1833959015.py:6: RuntimeWarning: coroutine 'do_some_work' was never awaited
  do_some_work()
RuntimeWarning: Enable tracemalloc to get the object allocation traceback
```
> **Now fixing above error**
```python
async def do_a_lot_of_work():
		await do_some_work()
		await do_some_work()
		await do_some_work()
await do_a_lot_of_work()
```
**OUTPUT :**  
```
Starting work
Work complete
Starting work
Work complete
Starting work
Work complete
```

It's important to recognize that this is not "multi-threading" in the way that we may be used to.  The asyncio library is running on a single thread, but it's using a loop to switch between tasks while one is waiting.

```python
async def do_a_lot_of_work_in_parallel():
	await asyncio.gather(do_some_work(), do_some_work(), do_some_work())
	
await do_a_lot_of_work_in_parallel()
```

```
Starting work
Starting work
Starting work
Work complete
Work complete
Work complete
```

---
