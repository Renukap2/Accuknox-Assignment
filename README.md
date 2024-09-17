# Accuknox-Assignment
1.By default, Django signals are executed synchronously. This means that when a signal is triggered, the associated receivers (the functions that respond to the signal) are executed in the same thread and must complete before the next step in the process can continue.


2.Yes, by default, Django signals run in the same thread as the caller. This means that when a signal is sent, the connected signal handlers are executed in the same thread as the signal sender, and the sender will wait for all handlers to complete before continuing its execution


3.Yes, by default, Django signals run in the same database transaction as the caller. This means that if a signal handler modifies the database, those changes are part of the same transaction as the original operation that triggered the signal. If the transaction is rolled back, the changes made by the signal handler will also be rolled back.

Custom Classes in Python

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

# Example usage:
rect = Rectangle(10, 5)
for dimension in rect:
    print(dimension)

    Output:

{'length': 10}
{'width': 5}




