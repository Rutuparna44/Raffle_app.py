# Raffle_app.py
import random

MAX_TICKETS_PER_DRAW = 5
TICKET_PRICE = 5
INITIAL_POT = 100
MIN_NUMBER = 1
MAX_NUMBER = 15

draw_started = False
tickets_bought = 0
pot_size = INITIAL_POT
user_tickets = {}

def generate_ticket_numbers():
    numbers = random.sample(range(MIN_NUMBER, MAX_NUMBER + 1), MAX_TICKETS_PER_DRAW)
    return sorted(numbers)

def display_menu():
    print("Welcome to the Raffle App")
    print(f"Status: {'Draw has started' if draw_started else 'Draw has not started'}")
    print(f"Current Pot Size: ${pot_size}")
    print("[1] Start a New Draw")
    print("[2] Buy Tickets")
    print("[3] Run Raffle")
    print("[4] Exit")

def handle_option(option):
    global draw_started
    global tickets_bought
    global pot_size
    global user_tickets

    if option == "1":
        if draw_started:
            print("Draw has already started. Cannot start a new draw.")
        else:
            print("Starting a new draw...")
            draw_started = True
            tickets_bought = 0
            pot_size = INITIAL_POT
            user_tickets = {}
    elif option == "2":
        if draw_started:
            name, num_tickets = input("Enter your name and number of tickets to buy (e.g., James,1): ").split(',')
            try:
                num_tickets = int(num_tickets)
                if num_tickets < 6:
                    if tickets_bought + num_tickets > MAX_TICKETS_PER_DRAW:
                        print(f"You can buy a maximum of {MAX_TICKETS_PER_DRAW - tickets_bought} tickets.")
                    else:
                        for _ in range(num_tickets):
                            ticket_numbers = generate_ticket_numbers()
                            user_tickets[name] = ticket_numbers
                            tickets_bought += 1
                            print(f"Hi {name}, you have purchased {tickets_bought} ticket(s).")
                            print(f"Ticket {tickets_bought}: {' '.join(map(str, ticket_numbers))}")
                        pot_size += num_tickets * TICKET_PRICE
                else:
                    print("Users can purchase up to a maximum of 5 tickets per draw")
            except ValueError:
                print("Invalid input. Please enter a valid number.")
        else:
            print("Draw has not started yet. Please start a new draw first.")
        input("Press any key to return to the main menu.")
    elif option == "3":
        if draw_started:
            if tickets_bought > 0:
                print("Running the raffle...")
                winning_numbers = generate_ticket_numbers()
                print(f"The winning numbers are: {winning_numbers}")
                winners = {}
                for name, ticket_numbers in user_tickets.items():
                    if ticket_numbers == winning_numbers:
                        winners[name] = ticket_numbers
                num_winners = len(winners)
                if num_winners > 0:
                    reward_groups = {2: 0.2, 3: 0.3, 4: 0.4, 5: 0.5}
                    total_rewards = {}
                    for group, reward_percentage in reward_groups.items():
                        total_rewards[group] = round(pot_size * reward_percentage / num_winners, 2)
                    print(f"We have {num_winners} winner(s)!")
                    for name, ticket_numbers in winners.items():
                        print(f"{name} won with ticket numbers: {ticket_numbers}")
                    print("Total rewards for each group:")
                    for group, reward in total_rewards.items():
                        print(f"Group {group}: ${reward}")
                else:
                    print("No winners this time.")
                draw_started = False
            else:
                print("No tickets bought yet. Please buy tickets before running the raffle.")
        else:
            print("Draw has not started yet. Please start a new draw first.")
        input("Press any key to return to the main menu.")
    elif option == "4":
        print("Exiting the program...")
        # Add any cleanup code here if needed
    else:
        print("Invalid option. Please try again.")

def main():
    while True:
        display_menu()
        option = input("Enter your choice (1-4): ")
        handle_option(option)
        if option == "4":
            break

if __name__ == "__main__":
    main()
