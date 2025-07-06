# C-TASK-2
import hashlib
import os

# File to store credentials
CREDENTIALS_FILE = "users.txt"

def hash_password(password):
    """Return SHA256 hashed password."""
    return hashlib.sha256(password.encode()).hexdigest()

def is_username_taken(username):
    """Check if the username already exists."""
    if not os.path.exists(CREDENTIALS_FILE):
        return False
    with open(CREDENTIALS_FILE, "r") as file:
        for line in file:
            stored_username, _ = line.strip().split(":")
            if stored_username == username:
                return True
    return False

def register():
    print("\n--- Register ---")
    username = input("Enter a username: ").strip()
    password = input("Enter a password: ").strip()

    if is_username_taken(username):
        print("❌ Username already taken. Try a different one.")
        return

    hashed_password = hash_password(password)
    with open(CREDENTIALS_FILE, "a") as file:
        file.write(f"{username}:{hashed_password}\n")
    
    print("✅ Registration successful!")

def login():
    print("\n--- Login ---")
    username = input("Enter your username: ").strip()
    password = input("Enter your password: ").strip()
    hashed_password = hash_password(password)

    if not os.path.exists(CREDENTIALS_FILE):
        print("❌ No users found. Please register first.")
        return

    with open(CREDENTIALS_FILE, "r") as file:
        for line in file:
            stored_username, stored_hash = line.strip().split(":")
            if stored_username == username and stored_hash == hashed_password:
                print("✅ Login successful!")
                return
    print("❌ Login failed. Invalid username or password.")

# Main menu
def main():
    while True:
        print("\n=== Login & Registration System ===")
        print("1. Register")
        print("2. Login")
        print("3. Exit")

        choice = input("Enter your choice (1/2/3): ").strip()
        if choice == "1":
            register()
        elif choice == "2":
            login()
        elif choice == "3":
            print("👋 Exiting...")
            break
        else:
            print("❌ Invalid choice. Try again.")

# Run the program
main()
