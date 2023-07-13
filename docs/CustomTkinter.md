## For CustomTkinter GUI files
- This does **NOT** support async functions

1. Import the required modules/libraries.
```python
from customtkinter import CTk, CTkLabel
```

2. Create a dictionary to store the functions that are scheduled to be called after a certain amount of time:
```python
# format: {function_name: after_event_id}
after_events = {}
```

3. Create a CustomTkinter  window and a test widget:
```python
window = CTk()
test_widget = CTkLabel(window, text="test")
test_widget.pack()
```

4. The functions below are for scheduling functions to be called after a certain amount of time (ms):
```python
def schedule_create(widget, ms, func, cancel_after_finished=False, *args, **kwargs):
    event_id = widget.after(ms, lambda: func(*args, **kwargs))
    after_events[func.__name__] = event_id
    if cancel_after_finished:
        widget.after(ms, lambda: schedule_cancel(widget, func))

def schedule_cancel(widget, func):
    try:
        event_id = after_events.get(func.__name__)
        if event_id is not None:
            widget.after_cancel(event_id)
            del after_events[func.__name__]
    except: 
        pass
```

5. Schedule a function to be called after 3 seconds:
```python
schedule_create(window, 3000, test_func) # using the window widget
schedule_create(test_widget, 3000, test_func) # using the test_widget widget
```

6. Cancel the scheduled functions:
```python
schedule_cancel(window, test_func) # using the window widget
schedule_cancel(test_widget, test_func) # using the test_widget widget
```

7. Cancel after the function has been called (also deletes the function from the dictionary):
```python
schedule_create(window, 3000, test_func, cancel_after_finished=True) # using the window widget
schedule_create(test_widget, 3000, test_func, cancel_after_finished=True) # using the test_widget widget
```