# AI Personal Shopper (with Mood Swings) - Enhanced Version
# Now includes customized input, varied output, calendar events, real-time context, purpose tagging, gender-based clothing suggestions, and multiple outfit recommendations

import random
import datetime
import time

# Base Clothing Class
class Clothing:
    def __init__(self, name, category, color, personality, purpose, gender):
        self.name = name
        self.category = category
        self.color = color
        self.personality = personality
        self.purpose = purpose
        self.gender = gender

    def talk(self, user_name, current_time):
        return (f"Hello {user_name}! It's {current_time}. I'm your {self.name}, a {self.color} {self.category}. "
                f"I'm feeling {self.personality['mood']} and I {self.personality['quirk']}. "
                f"Perfect for your {self.purpose} needs today!")

    def match_event(self, event_type, user_gender):
        return event_type in self.personality['preferred_events'] and self.gender in ["unisex", user_gender.lower()]


# Subclasses for Clothing Types
class Dress(Clothing):
    pass

class Jacket(Clothing):
    pass

class Shirt(Clothing):
    pass

class Skirt(Clothing):
    pass

class TShirt(Clothing):
    pass

class Blazer(Clothing):
    pass

class Jeans(Clothing):
    pass

class Saree(Clothing):
    pass

class Kurta(Clothing):
    pass


# Wardrobe Management
class Wardrobe:
    def __init__(self):
        self.clothes = []

    def add_clothing(self, clothing):
        self.clothes.append(clothing)

    def recommend(self, event, user_gender):
        filtered = [c for c in self.clothes if c.match_event(event, user_gender)]
        random.shuffle(filtered)
        return filtered[:3] if filtered else []


# Calendar-based Event Auto-Suggestion
calendar_event_map = {
    "1": "party",
    "2": "brunch",
    "3": "work",
    "4": "casual",
    "5": "date",
    "6": "picnic",
    "7": "gala"
}

# Accurate Real-Time Timestamp with Seconds
now = datetime.datetime.now(datetime.timezone.utc).astimezone()  # Local timezone
current_time = now.strftime("%A, %d %B %Y at %H:%M:%S %Z")

# Take User Input
user_name = input("What's your name?: ").strip().title()
user_gender = input("What's your gender? (male/female): ").strip().lower()
user_mood = input("How are you feeling today? (e.g., playful, chill, elegant): ").strip().lower()
weather = input("What's the weather like? (e.g., sunny, chilly, rainy): ").strip().lower()

# Manual Occasion Selection
print("\nSelect today's occasion:")
for key, val in calendar_event_map.items():
    print(f"{key}. {val.title()}")
selected_key = input("Enter the number of your occasion: ").strip()
event = calendar_event_map.get(selected_key, "casual")
print(f"\nHello {user_name}! Selected event for today is: {event.upper()}\n")

# Create Wardrobe and Items
wardrobe = Wardrobe()

# Female Clothing
wardrobe.add_clothing(Dress("Flirty Red Dress", "dress", "red", {
    'mood': "bold",
    'preferred_events': ["party", "date"],
    'quirk': "can't wait to dance"
}, purpose="night out or romantic vibe", gender="female"))

wardrobe.add_clothing(Dress("Elegant Black Gown", "dress", "black", {
    'mood': "elegant",
    'preferred_events': ["gala", "party"],
    'quirk': "am made for elegance"
}, purpose="formal and luxurious settings", gender="female"))

wardrobe.add_clothing(Skirt("Fun Polka Skirt", "skirt", "pink", {
    'mood': "bubbly",
    'preferred_events': ["brunch", "casual", "picnic"],
    'quirk': "am full of fun and flair"
}, purpose="cheerful casual hangouts", gender="female"))

wardrobe.add_clothing(Saree("Royal Silk Saree", "saree", "gold", {
    'mood': "graceful",
    'preferred_events': ["gala", "work"],
    'quirk': "elegance is my middle name"
}, purpose="traditional festive wear", gender="female"))

# Male Clothing
wardrobe.add_clothing(Shirt("Smart White Shirt", "shirt", "white", {
    'mood': "sharp",
    'preferred_events': ["work", "meeting"],
    'quirk': "give off leadership vibes"
}, purpose="professional and business attire", gender="male"))

wardrobe.add_clothing(Kurta("Casual Cotton Kurta", "kurta", "light blue", {
    'mood': "comfy",
    'preferred_events': ["casual", "picnic"],
    'quirk': "relax and chill"
}, purpose="comfortable ethnic wear", gender="male"))

wardrobe.add_clothing(Blazer("Formal Navy Blazer", "blazer", "navy", {
    'mood': "professional",
    'preferred_events': ["work", "gala"],
    'quirk': "crisp and powerful"
}, purpose="executive formal look", gender="male"))

wardrobe.add_clothing(Jeans("Weekend Blue Jeans", "jeans", "denim", {
    'mood': "cool",
    'preferred_events': ["casual", "brunch"],
    'quirk': "laid-back but stylish"
}, purpose="relaxed outings", gender="male"))

# Unisex Clothing
wardrobe.add_clothing(Jacket("Cool Denim Jacket", "jacket", "blue", {
    'mood': "laid-back",
    'preferred_events': ["casual", "concert", "brunch"],
    'quirk': "prefer the chill vibe"
}, purpose="stylish layering for relaxed outings", gender="unisex"))

wardrobe.add_clothing(Shirt("Weekend Linen Shirt", "shirt", "beige", {
    'mood': "relaxed",
    'preferred_events': ["picnic", "casual", "brunch"],
    'quirk': "flow with the breeze"
}, purpose="easy-going weekends", gender="unisex"))

wardrobe.add_clothing(Jacket("Urban Night Jacket", "jacket", "black", {
    'mood': "cool",
    'preferred_events': ["party", "concert"],
    'quirk': "shine under night lights"
}, purpose="evening urban wear", gender="unisex"))

# Recommendations (multiple)
recommendations = wardrobe.recommend(event, user_gender)
if recommendations:
    print("Today's outfit suggestions:")
    for r in recommendations:
        print("\n-", r.talk(user_name, current_time))
else:
    print("Sorry, nothing suitable in your wardrobe for today's occasion. Time to shop!")
