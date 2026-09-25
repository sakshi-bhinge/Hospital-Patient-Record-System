import java.sql.*;
import java.util.Scanner;

public class HospitalPatientRecordSystem {

    // Database Configuration
    private static final String URL = "jdbc:mysql://localhost:3306/hospital_db";
    private static final String USER = "root";
    private static final String PASSWORD = "your_password"; // change this

    // Simple admin login (basic access control)
    private static final String ADMIN_USERNAME = "admin";
    private static final String ADMIN_PASSWORD = "admin123";

    // Get database connection
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(URL, USER, PASSWORD);
    }

    // Add new patient
    public static void addPatient(String firstName, String lastName, int age,
                                  String gender, String diagnosis, String history) {

        String sql = "INSERT INTO patients (first_name, last_name, age, gender, diagnosis, medical_history) VALUES (?, ?, ?, ?, ?, ?)";

        try (Connection conn = getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setString(1, firstName);
            stmt.setString(2, lastName);
            stmt.setInt(3, age);
            stmt.setString(4, gender);
            stmt.setString(5, diagnosis);
            stmt.setString(6, history);

            stmt.executeUpdate();
            System.out.println("Patient added successfully.");

        } catch (SQLException e) {
            System.out.println("Database Error: " + e.getMessage());
        }
    }

    // View all patients
    public static void viewPatients() {

        String sql = "SELECT * FROM patients";

        try (Connection conn = getConnection();
             Statement stmt = conn.createStatement();
             ResultSet rs = stmt.executeQuery(sql)) {

            System.out.println("\nID | Name | Age | Gender | Diagnosis");
            System.out.println("-----------------------------------------------------");

            while (rs.next()) {
                System.out.println(
                        rs.getInt("id") + " | " +
                        rs.getString("first_name") + " " +
                        rs.getString("last_name") + " | " +
                        rs.getInt("age") + " | " +
                        rs.getString("gender") + " | " +
                        rs.getString("diagnosis")
                );
            }

        } catch (SQLException e) {
            System.out.println("Database Error: " + e.getMessage());
        }
    }

    // View single patient details
    public static void viewPatientById(int id) {

        String sql = "SELECT * FROM patients WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setInt(1, id);
            ResultSet rs = stmt.executeQuery();

            if (rs.next()) {
                System.out.println("\nPatient Details:");
                System.out.println("ID: " + rs.getInt("id"));
                System.out.println("Name: " + rs.getString("first_name") + " " + rs.getString("last_name"));
                System.out.println("Age: " + rs.getInt("age"));
                System.out.println("Gender: " + rs.getString("gender"));
                System.out.println("Diagnosis: " + rs.getString("diagnosis"));
                System.out.println("Medical History: " + rs.getString("medical_history"));
            } else {
                System.out.println("Patient not found.");
            }

        } catch (SQLException e) {
            System.out.println("Database Error: " + e.getMessage());
        }
    }

    // Update medical history
    public static void updateMedicalHistory(int id, String newHistory) {

        String sql = "UPDATE patients SET medical_history = ? WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setString(1, newHistory);
            stmt.setInt(2, id);

            int rows = stmt.executeUpdate();

            if (rows > 0) {
                System.out.println("Medical history updated successfully.");
            } else {
                System.out.println("Patient not found.");
            }

        } catch (SQLException e) {
            System.out.println("Database Error: " + e.getMessage());
        }
    }

    // Delete patient
    public static void deletePatient(int id) {

        String sql = "DELETE FROM patients WHERE id = ?";

        try (Connection conn = getConnection();
             PreparedStatement stmt = conn.prepareStatement(sql)) {

            stmt.setInt(1, id);

            int rows = stmt.executeUpdate();

            if (rows > 0) {
                System.out.println("Patient record deleted.");
            } else {
                System.out.println("Patient not found.");
            }

        } catch (SQLException e) {
            System.out.println("Database Error: " + e.getMessage());
        }
    }

    // Simple login method
    public static boolean login(Scanner sc) {

        System.out.println("=== Hospital Admin Login ===");
        System.out.print("Username: ");
        String username = sc.nextLine();

        System.out.print("Password: ");
        String password = sc.nextLine();

        return username.equals(ADMIN_USERNAME) && password.equals(ADMIN_PASSWORD);
    }

    // Main program
    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        if (!login(sc)) {
            System.out.println("Access Denied.");
            return;
        }

        while (true) {

            System.out.println("\n===== Hospital Patient Record System =====");
            System.out.println("1. Add Patient");
            System.out.println("2. View All Patients");
            System.out.println("3. View Patient Details");
            System.out.println("4. Update Medical History");
            System.out.println("5. Delete Patient");
            System.out.println("6. Exit");
            System.out.print("Enter choice: ");

            int choice = sc.nextInt();
            sc.nextLine(); // clear buffer

            switch (choice) {

                case 1:
                    System.out.print("First Name: ");
                    String first = sc.nextLine();

                    System.out.print("Last Name: ");
                    String last = sc.nextLine();

                    System.out.print("Age: ");
                    int age = sc.nextInt();
                    sc.nextLine();

                    System.out.print("Gender: ");
                    String gender = sc.nextLine();

                    System.out.print("Diagnosis: ");
                    String diagnosis = sc.nextLine();

                    System.out.print("Medical History: ");
                    String history = sc.nextLine();

                    addPatient(first, last, age, gender, diagnosis, history);
                    break;

                case 2:
                    viewPatients();
                    break;

                case 3:
                    System.out.print("Enter Patient ID: ");
                    int viewId = sc.nextInt();
                    viewPatientById(viewId);
                    break;

                case 4:
                    System.out.print("Enter Patient ID: ");
                    int updateId = sc.nextInt();
                    sc.nextLine();

                    System.out.print("New Medical History: ");
                    String newHistory = sc.nextLine();

                    updateMedicalHistory(updateId, newHistory);
                    break;

                case 5:
                    System.out.print("Enter Patient ID: ");
                    int deleteId = sc.nextInt();
                    deletePatient(deleteId);
                    break;

                case 6:
                    System.out.println("System Closed.");
                    sc.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid choice.");
            }
        }
    }
}
