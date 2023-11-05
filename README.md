import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

class Automobile {
    private String make;
    private String model;
    private String color;
    private int year;
    private int mileage;

    public void addVehicle(Scanner scanner) {
        System.out.print("Enter year: ");
        year = Integer.parseInt(scanner.nextLine());
        System.out.print("Enter make: ");
        make = scanner.nextLine();
        System.out.print("Enter model: ");
        model = scanner.nextLine();
        System.out.print("Enter color: ");
        color = scanner.nextLine();
        System.out.print("Enter mileage: ");
        mileage = Integer.parseInt(scanner.nextLine());
    }

    @Override
    public String toString() {
        return year + " " + make + " " + model + " Color: " + color + " Mileage: " + mileage;
    }
}

public class Main {
    public static void main(String[] args) {
        List<String> vehicleList = new ArrayList<>();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n1. Add a Vehicle");
            System.out.println("2. Delete a Vehicle");
            System.out.println("3. View Inventory");
            System.out.println("4. Update Inventory");
            System.out.println("5. Export Inventory");
            System.out.println("6. Quit");
            System.out.print("What would you like to do? ");

            String ans = scanner.nextLine();

            switch (ans) {
                case "1":
                    Automobile car = new Automobile();
                    car.addVehicle(scanner);
                    vehicleList.add(car.toString());
                    break;

                case "2":
                    if (!vehicleList.isEmpty()) {
                        System.out.print("Enter position of vehicle to remove: ");
                        try {
                            int pos = Integer.parseInt(scanner.nextLine()) - 1;
                            if (pos >= 0 && pos < vehicleList.size()) {
                                vehicleList.remove(pos);
                                System.out.println("Successfully removed vehicle");
                            } else {
                                System.out.println("Invalid position. Vehicle not removed.");
                            }
                        } catch (NumberFormatException e) {
                            System.out.println("Invalid input. Vehicle not removed.");
                        }
                    } else {
                        System.out.println("Inventory is empty. No vehicle to remove.");
                    }
                    break;

                case "3":
                    if (!vehicleList.isEmpty()) {
                        int index = 1;
                        for (String vehicle : vehicleList) {
                            System.out.println(index + ". " + vehicle);
                            index++;
                        }
                    } else {
                        System.out.println("Inventory is empty.");
                    }
                    break;

                case "4":
                    if (!vehicleList.isEmpty()) {
                        System.out.print("Enter the position of the vehicle to edit: ");
                        try {
                            int pos = Integer.parseInt(scanner.nextLine()) - 1;
                            if (pos >= 0 && pos < vehicleList.size()) {
                                Automobile updatedCar = new Automobile();
                                updatedCar.addVehicle(scanner);
                                vehicleList.set(pos, updatedCar.toString());
                                System.out.println("Vehicle was updated");
                            } else {
                                System.out.println("Invalid position. Vehicle not updated.");
                            }
                        } catch (NumberFormatException e) {
                            System.out.println("Invalid input. Vehicle not updated.");
                        }
                    } else {
                        System.out.println("Inventory is empty. No vehicle to update.");
                    }
                    break;

                case "5":
                    if (!vehicleList.isEmpty()) {
                        try {
                            writeToFile("vehicle_inv.txt", vehicleList);
                            System.out.println("Inventory exported to vehicle_inv.txt");
                        } catch (IOException e) {
                            System.out.println("Failed to export inventory.");
                        }
                    } else {
                        System.out.println("Inventory is empty. Nothing to export.");
                    }
                    break;

                case "6":
                    System.out.println("Goodbye!");
                    return;

                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }

    private static void writeToFile(String filePath, List<String> vehicleList) throws IOException {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (String vehicle : vehicleList) {
                writer.write(vehicle + "\n");
            }
        }
    }
}
