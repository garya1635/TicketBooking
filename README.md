import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class User {
    private String username;
    private Map<String, Integer> bookedSeats;

    public User(String username) {
        this.username = username;
        this.bookedSeats = new HashMap<>();
    }

    public String getUsername() {
        return username;
    }

    public Map<String, Integer> getBookedSeats() {
        return bookedSeats;
    }

    public void bookSeat(String seat, int numTickets) {
        bookedSeats.put(seat, numTickets);
    }
}

class BookingSystem {
    private Map<String, Integer> availableSeats;
    private Map<String, User> users;

    public BookingSystem() {
        availableSeats = new HashMap<>();
        users = new HashMap<>();
    }

    public void login(String username) {
        if (!users.containsKey(username)) {
            users.put(username, new User(username));
        }
    }

    public void displayAvailableSeats() {
        System.out.println("Available Seats:");
        for (Map.Entry<String, Integer> entry : availableSeats.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue() + " seats");
        }
    }

    public void displayUserBookedSeats(String username) {
        System.out.println("Booked Seats for " + username + ":");
        User user = users.get(username);
        for (Map.Entry<String, Integer> entry : user.getBookedSeats().entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue() + " seats");
        }
    }

    public void bookSeats(String username, String seat, int numTickets) {
        availableSeats.put(seat, availableSeats.getOrDefault(seat, 0) - numTickets);
        User user = users.get(username);
        user.bookSeat(seat, numTickets);
        System.out.println("Seats booked successfully!");
    }

    public void logout(String username) {
        users.remove(username);
    }
}

public class Main {
    public static void main(String[] args) {
        BookingSystem bookingSystem = new BookingSystem();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.print("Enter username to login (Type 'exit' to quit): ");
            String username = scanner.nextLine();
            if (username.equals("exit")) {
                break;
            }
            bookingSystem.login(username);
            bookingSystem.displayAvailableSeats();
            bookingSystem.displayUserBookedSeats(username);

            System.out.print("Enter the seat you want to book: ");
            String seat = scanner.nextLine();
            System.out.print("Enter the number of tickets: ");
            int numTickets = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            bookingSystem.bookSeats(username, seat, numTickets);

            System.out.print("Do you want to logout? (yes/no): ");
            String logoutChoice = scanner.nextLine();
            if (logoutChoice.equalsIgnoreCase("yes")) {
                bookingSystem.logout(username);
            }
        }

        System.out.println("Goodbye!");
    }
}
