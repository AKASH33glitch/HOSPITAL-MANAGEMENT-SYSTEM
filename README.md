import java.util.*;

class Patient {
    private int id;
    private String name;
    private int age;
    private String disease;
    private String assignedDoctor;
    private String username;
    private String password;

    public Patient(int id, String name, int age, String disease, String username, String password) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.disease = disease;
        this.assignedDoctor = "Not Assigned";
        this.username = username;
        this.password = password;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getDisease() {
        return disease;
    }

    public String getAssignedDoctor() {
        return assignedDoctor;
    }

    public void setAssignedDoctor(String doctorName) {
        this.assignedDoctor = doctorName;
    }

    public String getUsername() {
        return username;
    }

    public String getPassword() {
        return password;
    }

    @Override
    public String toString() {
        return "Patient ID: " + id + ", Name: " + name + ", Age: " + age + ", Disease: " + disease + ", Assigned Doctor: " + assignedDoctor;
    }
}

class HospitalManagementSystem {
    private List<Patient> patients = new ArrayList<>();
    private int nextId = 1;

    public void addPatient(String name, int age, String disease, String username, String password) {
        patients.add(new Patient(nextId++, name, age, disease, username, password));
        System.out.println("Patient added successfully.");
    }

    public void viewAllPatients() {
        if (patients.isEmpty()) {
            System.out.println("No patients found.");
        } else {
            for (Patient patient : patients) {
                System.out.println(patient);
            }
        }
    }

    public void searchPatientById(int id) {
        for (Patient patient : patients) {
            if (patient.getId() == id) {
                System.out.println(patient);
                return;
            }
        }
        System.out.println("Patient not found.");
    }

    public void deletePatientById(int id) {
        Iterator<Patient> iterator = patients.iterator();
        while (iterator.hasNext()) {
            if (iterator.next().getId() == id) {
                iterator.remove();
                System.out.println("Patient deleted successfully.");
                return;
            }
        }
        System.out.println("Patient not found.");
    }

    public void updatePatientById(int id, String newName, int newAge, String newDisease) {
        for (Patient patient : patients) {
            if (patient.getId() == id) {
                patients.set(patients.indexOf(patient), new Patient(id, newName, newAge, newDisease, patient.getUsername(), patient.getPassword()));
                System.out.println("Patient updated successfully.");
                return;
            }
        }
        System.out.println("Patient not found.");
    }

    public void assignDoctorToPatient(int id, String doctorName) {
        for (Patient patient : patients) {
            if (patient.getId() == id) {
                patient.setAssignedDoctor(doctorName);
                System.out.println("Doctor " + doctorName + " assigned to patient ID: " + id);
                return;
            }
        }
        System.out.println("Patient not found.");
    }

    public boolean patientLogin(String username, String password) {
        for (Patient patient : patients) {
            if (patient.getUsername().equals(username) && patient.getPassword().equals(password)) {
                System.out.println("Login successful. Welcome, " + patient.getName() + "!");
                return true;
            }
        }
        System.out.println("Invalid username or password.");
        return false;
    }
}

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        HospitalManagementSystem hms = new HospitalManagementSystem();

        while (true) {
            try {
                System.out.println("\n--- Hospital Management System ---");
                System.out.println("1. Add Patient");
                System.out.println("2. View All Patients");
                System.out.println("3. Search Patient by ID");
                System.out.println("4. Delete Patient by ID");
                System.out.println("5. Update Patient by ID");
                System.out.println("6. Assign Doctor to Patient");
                System.out.println("7. Patient Login");
                System.out.println("8. Exit");
                System.out.print("Enter your choice: ");

                int choice = scanner.nextInt();
                scanner.nextLine(); // consume newline

                switch (choice) {
                    case 1:
                        System.out.print("Enter name: ");
                        String name = scanner.nextLine();
                        System.out.print("Enter age: ");
                        int age = scanner.nextInt();
                        scanner.nextLine(); // consume newline
                        System.out.print("Enter disease: ");
                        String disease = scanner.nextLine();
                        System.out.print("Enter username: ");
                        String username = scanner.nextLine();
                        System.out.print("Enter password: ");
                        String password = scanner.nextLine();
                        hms.addPatient(name, age, disease, username, password);
                        break;

                    case 2:
                        hms.viewAllPatients();
                        break;

                    case 3:
                        System.out.print("Enter patient ID: ");
                        int id = scanner.nextInt();
                        hms.searchPatientById(id);
                        break;

                    case 4:
                        System.out.print("Enter patient ID: ");
                        int deleteId = scanner.nextInt();
                        hms.deletePatientById(deleteId);
                        break;

                    case 5:
                        System.out.print("Enter patient ID: ");
                        int updateId = scanner.nextInt();
                        scanner.nextLine(); // consume newline
                        System.out.print("Enter new name: ");
                        String newName = scanner.nextLine();
                        System.out.print("Enter new age: ");
                        int newAge = scanner.nextInt();
                        scanner.nextLine(); // consume newline
                        System.out.print("Enter new disease: ");
                        String newDisease = scanner.nextLine();
                        hms.updatePatientById(updateId, newName, newAge, newDisease);
                        break;

                    case 6:
                        System.out.print("Enter patient ID: ");
                        int assignId = scanner.nextInt();
                        scanner.nextLine(); // consume newline
                        System.out.print("Enter doctor name: ");
                        String doctorName = scanner.nextLine();
                        hms.assignDoctorToPatient(assignId, doctorName);
                        break;

                    case 7:
                        System.out.print("Enter username: ");
                        String loginUsername = scanner.nextLine();
                        System.out.print("Enter password: ");
                        String loginPassword = scanner.nextLine();
                        hms.patientLogin(loginUsername, loginPassword);
                        break;

                    case 8:
                        System.out.println("Exiting system. Goodbye!");
                        scanner.close();
                        return;

                    default:
                        System.out.println("Invalid choice. Please enter a number between 1 and 8.");
                }
            } catch (InputMismatchException e) {
                System.out.println("Invalid input. Please enter a valid number.");
                scanner.nextLine(); // Clear invalid input
            } catch (Exception e) {
                System.out.println("An error occurred: " + e.getMessage());
            }
        }
    }
}
