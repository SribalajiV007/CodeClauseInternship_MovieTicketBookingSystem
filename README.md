import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

enum Genre {
 ACTION, COMEDY, DRAMA, HORROR, ROMANCE, SCIENCE_FICTION
}

class Movie {
 private String title;
 private Genre genre;
 private int duration; 

 public Movie(String title, Genre genre, int duration) {
     this.title = title;
     this.genre = genre;
     this.duration = duration;
 }

 public String getTitle() {
     return title;
 }

 public Genre getGenre() {
     return genre;
 }

 public int getDuration() {
     return duration;
 }
}

//Class representing a Theater
class Theater {
 private String name;
 private List<Movie> movies;
 private boolean[][] seats; // 2D array representing seat availability

 public Theater(String name, int numRows, int numSeatsPerRow) {
     this.name = name;
     this.movies = new ArrayList<>();
     this.seats = new boolean[numRows][numSeatsPerRow];
     // Initialize all seats as available
     for (int i = 0; i < numRows; i++) {
         for (int j = 0; j < numSeatsPerRow; j++) {
             seats[i][j] = true;
         }
     }
 }

 public String getName() {
     return name;
 }

 public List<Movie> getMovies() {
     return movies;
 }

 public boolean[][] getSeats() {
     return seats;
 }

 public void addMovie(Movie movie) {
     movies.add(movie);
 }

 public boolean bookSeat(int row, int seatNumber) {
     if (row < 0 || row >= seats.length || seatNumber < 0 || seatNumber >= seats[0].length) {
         return false; // Invalid seat
     }
     if (seats[row][seatNumber]) {
         seats[row][seatNumber] = false; // Mark seat as booked
         return true;
     } else {
         return false; // Seat already booked
     }
 }
}

//Main class
public class MovieTicketBookingSystem {
 private static Scanner scanner = new Scanner(System.in);

 public static void main(String[] args) {
     // Welcome message and collect user data
     System.out.println("Welcome to CodeClause Cinemas");
     System.out.print("Please enter your name: ");
     String name = scanner.nextLine();
     System.out.print("Please enter your phone number: ");
     String phoneNumber = scanner.nextLine();
     System.out.print("Please enter your city: ");
     String city = scanner.nextLine();

     // Create a theater
     Theater theater = new Theater("Cineplex", 5, 10);

     // Add some movies
     theater.addMovie(new Movie("Manjummel Boys", Genre.ACTION, 150));
     theater.addMovie(new Movie("Premalu", Genre.ROMANCE, 152));
     theater.addMovie(new Movie("Aavesham", Genre.ACTION, 195));
     theater.addMovie(new Movie("Kung Fu Panda 4", Genre.COMEDY, 120));
     theater.addMovie(new Movie("Ghostbusters: Frozen Empire", Genre.COMEDY, 110));
     theater.addMovie(new Movie("Crew", Genre.DRAMA, 160));
     theater.addMovie(new Movie("Ghilli", Genre.ACTION, 170));
     theater.addMovie(new Movie("Romeo", Genre.ROMANCE, 155));
     theater.addMovie(new Movie("Double Tuckerr", Genre.COMEDY, 125));
     theater.addMovie(new Movie("Kasoombu", Genre.DRAMA, 140));

     // Main menu
     int choice;
     do {
         System.out.println("\nHello, " + name + "! What would you like to do?");
         System.out.println("1. List Movies");
         System.out.println("2. Book Tickets");
         System.out.println("3. Exit");
         System.out.print("Enter your choice: ");
         choice = scanner.nextInt();
         scanner.nextLine(); // Consume newline

         switch (choice) {
             case 1:
                 listMovies(theater);
                 break;
             case 2:
                 bookTickets(theater, name);
                 break;
             case 3:
                 System.out.println("Exiting...");
                 break;
             default:
                 System.out.println("Invalid choice. Please try again.");
         }
     } while (choice != 3);

     scanner.close();
 }

 // Method to list available movies
 private static void listMovies(Theater theater) {
     System.out.println("Available Movies:");
     List<Movie> movies = theater.getMovies();
     for (int i = 0; i < movies.size(); i++) {
         System.out.println((i + 1) + ". " + movies.get(i).getTitle());
     }
 }

 // Method to book tickets
 private static void bookTickets(Theater theater, String userName) {
     listMovies(theater);
     System.out.print("Enter the movie number: ");
     int movieNumber = scanner.nextInt();
     scanner.nextLine(); // Consume newline

     List<Movie> movies = theater.getMovies();
     if (movieNumber < 1 || movieNumber > movies.size()) {
         System.out.println("Invalid movie number.");
         return;
     }

     Movie selectedMovie = movies.get(movieNumber - 1);
     System.out.println("You have selected: " + selectedMovie.getTitle());

     System.out.print("Enter number of seats to book: ");
     int numSeats = scanner.nextInt();
     scanner.nextLine(); // Consume newline

     boolean[][] seats = theater.getSeats();
     int bookedSeats = 0;
     StringBuilder seatNumbers = new StringBuilder();

     for (int i = 0; i < seats.length; i++) {
         for (int j = 0; j < seats[0].length; j++) {
             if (seats[i][j]) {
                 System.out.println("Seat " + (i + 1) + "-" + (j + 1) + " available");
                 bookedSeats++;
                 seatNumbers.append((i + 1) + "-" + (j + 1)).append(", ");
                 if (bookedSeats >= numSeats) {
                     break;
                 }
             }
         }
         if (bookedSeats >= numSeats) {
             break;
         }
     }

     System.out.println("Do you want to proceed with booking? (yes/no)");
     String proceed = scanner.nextLine().toLowerCase();
     if (proceed.equals("yes")) {
         for (int i = 0; i < seats.length; i++) {
             for (int j = 0; j < seats[0].length; j++) {
                 if (seats[i][j]) {
                     theater.bookSeat(i, j);
                     numSeats--;
                     if (numSeats == 0) {
                         break;
                     }
                 }
             }
             if (numSeats == 0) {
                 break;
             }
         }
         System.out.println("Booking successful!");
         System.out.println("You have booked for " + selectedMovie.getTitle() + " and your seat numbers are: " + seatNumbers.toString());
         System.out.println("Thank You for Choosing CodeClause Cinemas");
     } else {
         System.out.println("Booking cancelled.");
     }
 }
}
