## For normal .py files (Tkinter and CustomTkinter are not supported)
- Can also be used for discord.py bots

1. Import the required modules/libraries.
```python
from threading import Timer
from asyncio import sleep, create_task
```

2. Create a dictionary to store the functions that are scheduled to be called after a certain amount of time:
```python
# format: {function_name: Timer/Task}
after_events = {}
```

3. The functions below are for scheduling functions to be called after a certain amount of time (seconds):
```python
def schedule_create(seconds: float | int, func, async_func=False):
    if async_func is True:
        async def task():
            await sleep(seconds)
            await func()
        time_func = create_task(task())
        after_events[func.__name__] = time_func
    else:
        time_func = Timer(seconds, func)
        after_events[func.__name__] = time_func
        time_func.start()
    return time_func

def schedule_cancel(func):
    time_func = after_events.get(func.__name__)
    if time_func is not None:
        time_func.cancel()
        del after_events[func.__name__]
        return True
    else:
        return False
```
4. Schedule a function to be called after 3 seconds:
```python

# Our test functions
def test_func(): # normal function
    print("test_func call")
    return
async def test_func_async(): # async function
    print("test_func_async call")
    return

schedule_create(3, test_func) # normal function call
schedule_create(3, test_func_async, async_func=True) # async function call
```

5. Cancel the scheduled functions:
```python
schedule_cancel(test_func) # normal function cancel
schedule_cancel(test_func_async) # async function cancel
```