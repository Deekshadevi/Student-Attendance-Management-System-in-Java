import java.io.*;
import java.util.*;

class Student {
    String rollNo;
    String name;
    boolean isPresent;

    Student(String rollNo, String name) {
        this.rollNo = rollNo;
        this.name = name;
        this.isPresent = false;
    }
}

public class AttendanceSystem {
    static Scanner sc = new Scanner(System.in);
    static List<Student> students = new ArrayList<>();
    static final String FILE_NAME = "attendance.txt";

    public static void main(String[] args) {
        loadData();

        while (true) {
            System.out.println("\n===== Student Attendance Management =====");
            System.out.println("1. Add Student");
            System.out.println("2. Mark Attendance");
            System.out.println("3. View Attendance");
            System.out.println("4. Save & Exit");
            System.out.print("Enter choice: ");
            int choice = sc.nextInt();

            switch (choice) {
                case 1 -> addStudent();
                case 2 -> markAttendance();
                case 3 -> viewAttendance();
                case 4 -> {
                    saveData();
                    System.out.println("Data saved. Exiting...");
                    return;
                }
                default -> System.out.println("Invalid choice. Try again!");
            }
        }
    }

    static void addStudent() {
        System.out.print("Enter Roll No: ");
        String roll = sc.next();
        sc.nextLine(); 
        System.out.print("Enter Name: ");
        String name = sc.nextLine();
        students.add(new Student(roll, name));
        System.out.println("Student added successfully!");
    }

    static void markAttendance() {
        for (Student s : students) {
            System.out.print("Is " + s.name + " (Roll No: " + s.rollNo + ") present? (y/n): ");
            String ans = sc.next();
            s.isPresent = ans.equalsIgnoreCase("y");
        }
        System.out.println("Attendance marked successfully!");
    }

    static void viewAttendance() {
        System.out.println("\n--- Attendance Report ---");
        for (Student s : students) {
            System.out.println(s.rollNo + " - " + s.name + " : " + (s.isPresent ? "Present" : "Absent"));
        }
    }

    static void saveData() {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter(FILE_NAME))) {
            for (Student s : students) {
                bw.write(s.rollNo + "," + s.name + "," + s.isPresent);
                bw.newLine();
            }
        } catch (IOException e) {
            System.out.println("Error saving data: " + e.getMessage());
        }
    }

    static void loadData() {
        File file = new File(FILE_NAME);
        if (!file.exists()) return;
        try (BufferedReader br = new BufferedReader(new FileReader(FILE_NAME))) {
            String line;
            while ((line = br.readLine()) != null) {
                String[] data = line.split(",");
                if (data.length == 3) {
                    Student s = new Student(data[0], data[1]);
                    s.isPresent = Boolean.parseBoolean(data[2]);
                    students.add(s);
                }
            }
        } catch (IOException e) {
            System.out.println("Error loading data: " + e.getMessage());
        }
    }
}
