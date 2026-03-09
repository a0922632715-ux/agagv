"""
vehicle.py
AGV Vehicle Model
Define the behavior and state of an AGV in the system.
"""

class Vehicle:
    def __init__(self, vid, position=(0, 0), speed=1.0):
        """
        Initialize an AGV vehicle.

        Parameters
        ----------
        vid : int
            Vehicle ID
        position : tuple
            Initial position (x, y)
        speed : float
            Movement speed
        """

        self.id = vid
        self.position = position
        self.speed = speed

        # vehicle state
        self.busy = False
        self.current_task = None
        self.total_distance = 0

    def assign_task(self, task):
        """
        Assign a task to the vehicle.
        """

        self.current_task = task
        self.busy = True

        print(f"[AGV {self.id}] Assigned Task {task.id}")

    def move_to(self, target):
        """
        Simulate vehicle movement to target position.
        """

        x1, y1 = self.position
        x2, y2 = target

        distance = abs(x1 - x2) + abs(y1 - y2)

        self.total_distance += distance
        self.position = target

        print(f"[AGV {self.id}] Moving to {target} (distance={distance})")

    def execute_task(self):
        """
        Execute the assigned task.
        """

        if self.current_task is None:
            return

        # move to pickup location
        self.move_to(self.current_task.start)

        # move to delivery location
        self.move_to(self.current_task.end)

        self.complete_task()

    def complete_task(self):
        """
        Complete the current task.
        """

        if self.current_task:
            print(f"[AGV {self.id}] Completed Task {self.current_task.id}")
            self.current_task.completed = True

        self.current_task = None
        self.busy = False

    def status(self):
        """
        Print current vehicle status.
        """

        state = "Busy" if self.busy else "Idle"

        print(
            f"AGV {self.id} | Position: {self.position} | "
            f"State: {state} | Distance: {self.total_distance}"
        )
