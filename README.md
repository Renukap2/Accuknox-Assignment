# Accuknox-Assignment
1.By default, Django signals are executed synchronously. This means that when a signal is triggered, the associated receivers (the functions that respond to the signal) are executed in the same thread and must complete before the next step in the process can continue.
import time
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.contrib.auth.models import User
@receiver(post_save, sender=User)
def my_signal_receiver(sender, instance, **kwargs):
    print("Signal received. Starting a time-consuming task...")
    time.sleep(5)  # Simulate a task that takes 5 seconds
    print("Task completed.")
user = User(username='testuser')
user.save()

print("User save operation completed.")

2.Yes, by default, Django signals run in the same thread as the caller. This means that when a signal is sent, the connected signal handlers are executed in the same thread as the signal sender, and the sender will wait for all handlers to complete before continuing its execution
import threading
import time
from django.db.models.signals import post_save
from django.dispatch import receiver
from django.db import models

class MyModel(models.Model):
    name = models.CharField(max_length=100)
@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, **kwargs):
    print(f"Signal received for: {instance.name}")
    print(f"Signal handler thread ID: {threading.get_ident()}")
    time.sleep(2)  # Simulate a delay
    print("Signal handler finished")
obj = MyModel(name="Test")
obj.save()

print("Object created")
print(f"Main thread ID: {threading.get_ident()}")

3.Yes, by default, Django signals run in the same database transaction as the caller. This means that if a signal handler modifies the database, those changes are part of the same transaction as the original operation that triggered the signal. If the transaction is rolled back, the changes made by the signal handler will also be rolled back.

from django.db import models, transaction
from django.db.models.signals import post_save
from django.dispatch import receiver
class MyModel(models.Model):
    name = models.CharField(max_length=100)
    processed = models.BooleanField(default=False)
@receiver(post_save, sender=MyModel)
def my_signal_handler(sender, instance, **kwargs):
    print("Signal received. Modifying the instance within the same transaction.")
    instance.processed = True
    instance.save()
try:
    with transaction.atomic():
        obj = MyModel(name="Test")
        obj.save()
        print(f"Object created with processed={obj.processed}")
        raise Exception("Simulating an error to trigger rollback")
except Exception as e:
    print(f"Transaction rolled back due to: {e}")
print(f"Final state in database: {MyModel.objects.filter(name='Test').exists()}")

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




