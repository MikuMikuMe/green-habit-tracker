# green-habit-tracker

# Green Habit Tracker Application

The Green Habit Tracker is a habit-tracking application that encourages eco-friendly behavior. It uses gamification to reward users for sustainable actions. Below is a basic implementation of the application in Python, including comments and error handling.

```python
import json
import datetime

# Define the possible eco-friendly habits
HABITS = {
    1: "Use a reusable water bottle",
    2: "Take public transportation",
    3: "Recycle waste",
    4: "Reduce energy consumption",
    5: "Plant a tree",
}

# Define rewards for each habit in points
REWARDS = {
    1: 10,  # points for using a reusable water bottle
    2: 20,  # points for taking public transportation
    3: 15,  # points for recycling waste
    4: 25,  # points for reducing energy consumption
    5: 50,  # points for planting a tree
}

class GreenHabitTracker:
    def __init__(self):
        self.user_data = {}
        self.load_data()

    def load_data(self):
        """Loads user data from a JSON file."""
        try:
            with open('user_data.json', 'r') as file:
                self.user_data = json.load(file)
            print("User data loaded successfully.")
        except FileNotFoundError:
            print("No user data found. Starting fresh.")
            self.user_data = {}
        except json.JSONDecodeError:
            print("Error reading user data. Starting fresh.")
            self.user_data = {}

    def save_data(self):
        """Saves user data to a JSON file."""
        try:
            with open('user_data.json', 'w') as file:
                json.dump(self.user_data, file, indent=4)
            print("User data saved successfully.")
        except Exception as e:
            print(f"Error saving data: {e}")

    def track_habit(self, user, habit_id):
        """Tracks a habit for the user and updates their points."""
        if habit_id not in HABITS:
            print("Invalid habit ID.")
            return

        today = str(datetime.date.today())
        if user not in self.user_data:
            self.user_data[user] = {"points": 0, "habits": {}}
        
        if today not in self.user_data[user]["habits"]:
            self.user_data[user]["habits"][today] = []

        if habit_id not in self.user_data[user]["habits"][today]:
            self.user_data[user]["habits"][today].append(habit_id)
            self.user_data[user]["points"] += REWARDS[habit_id]
            print(f"Habit tracked: {HABITS[habit_id]}. Points earned: {REWARDS[habit_id]}")
        else:
            print(f"Habit already tracked today: {HABITS[habit_id]}. No additional points awarded.")

        self.save_data()

    def display_user_info(self, user):
        """Displays the user's habit track record and total points."""
        if user not in self.user_data:
            print("User does not exist.")
            return

        print(f"User: {user}")
        print(f"Total Points: {self.user_data[user]['points']}")
        print("Habits tracked:")

        for date, habits in self.user_data[user].get("habits", {}).items():
            print(f"Date: {date}")
            for habit_id in habits:
                print(f"  - {HABITS[habit_id]}")

    def run(self):
        """Runs the main application loop."""
        while True:
            print("\nGreen Habit Tracker")
            print("1. Track Habit")
            print("2. View User Info")
            print("3. Exit")
            try:
                choice = int(input("Choose an option: "))
            except ValueError:
                print("Please enter a valid choice.")
                continue

            if choice == 1:
                user = input("Enter user name: ").strip()
                print("Choose a habit to track:")
                for id, habit in HABITS.items():
                    print(f"{id}. {habit}")
                try:
                    habit_id = int(input("Enter habit ID: "))
                except ValueError:
                    print("Please enter a valid habit ID.")
                    continue
                self.track_habit(user, habit_id)
            elif choice == 2:
                user = input("Enter user name: ").strip()
                self.display_user_info(user)
            elif choice == 3:
                print("Exiting...")
                break
            else:
                print("Invalid choice. Please select a valid option.")

if __name__ == "__main__":
    app = GreenHabitTracker()
    app.run()
```

### Explanation:
- **Data Storage**: User data is stored in a `user_data.json` file for persistence between sessions.
- **Gamification**: Each habit has associated points which are added to the user's total upon successful tracking.
- **Error Handling**: Includes handling for file I/O and conversion errors.
- **Comments**: The code is commented to explain major steps and functions.
- **Modular Design**: The `GreenHabitTracker` class encapsulates functionality for handling user data and interacting with the application.