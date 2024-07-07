# PRODIGY_SD_03
coding a program to implement a simple contact management system
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class ContactManager {
    private List<Contact> contacts;
    private Scanner scanner;

    // Constructor
    public ContactManager() {
        contacts = new ArrayList<>();
        scanner = new Scanner(System.in);
        loadContacts(); // Load contacts from file upon initialization
    }

    // Method to add a new contact
    public void addContact() {
        System.out.print("Enter name: ");
        String name = scanner.nextLine();
        System.out.print("Enter phone number: ");
        String phoneNumber = scanner.nextLine();
        System.out.print("Enter email address: ");
        String email = scanner.nextLine();

        Contact newContact = new Contact(name, phoneNumber, email);
        contacts.add(newContact);
        System.out.println("Contact added successfully.");
    }

    // Method to view all contacts
    public void viewContacts() {
        if (contacts.isEmpty()) {
            System.out.println("No contacts found.");
        } else {
            System.out.println("Contact List:");
            for (Contact contact : contacts) {
                System.out.println(contact);
            }
        }
    }

    // Method to edit an existing contact
    public void editContact() {
        System.out.print("Enter the name of the contact to edit: ");
        String nameToEdit = scanner.nextLine();

        boolean found = false;
        for (Contact contact : contacts) {
            if (contact.getName().equalsIgnoreCase(nameToEdit)) {
                System.out.print("Enter new phone number: ");
                String newPhoneNumber = scanner.nextLine();
                System.out.print("Enter new email address: ");
                String newEmail = scanner.nextLine();

                contact.setPhoneNumber(newPhoneNumber);
                contact.setEmail(newEmail);
                System.out.println("Contact updated successfully.");
                found = true;
                break;
            }
        }

        if (!found) {
            System.out.println("Contact not found.");
        }
    }

    // Method to delete a contact
    public void deleteContact() {
        System.out.print("Enter the name of the contact to delete: ");
        String nameToDelete = scanner.nextLine();

        Contact contactToRemove = null;
        for (Contact contact : contacts) {
            if (contact.getName().equalsIgnoreCase(nameToDelete)) {
                contactToRemove = contact;
                break;
            }
        }

        if (contactToRemove != null) {
            contacts.remove(contactToRemove);
            System.out.println("Contact deleted successfully.");
        } else {
            System.out.println("Contact not found.");
        }
    }

    // Method to save contacts to file
    public void saveContacts() {
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("contacts.dat"))) {
            oos.writeObject(contacts);
            System.out.println("Contacts saved successfully.");
        } catch (IOException e) {
            System.out.println("Error saving contacts: " + e.getMessage());
        }
    }

    // Method to load contacts from file
    @SuppressWarnings("unchecked")
    public void loadContacts() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("contacts.dat"))) {
            contacts = (List<Contact>) ois.readObject();
            System.out.println("Contacts loaded successfully.");
        } catch (IOException | ClassNotFoundException e) {
            // If file doesn't exist or error reading, just continue with an empty list
            System.out.println("No contacts found or error loading contacts: " + e.getMessage());
        }
    }

    // Main method to run the contact manager program
    public static void main(String[] args) {
        ContactManager contactManager = new ContactManager();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\nWelcome to Contact Manager");
            System.out.println("1. Add New Contact");
            System.out.println("2. View All Contacts");
            System.out.println("3. Edit Contact");
            System.out.println("4. Delete Contact");
            System.out.println("5. Save Contacts");
            System.out.println("6. Exit");

            System.out.print("Enter your choice: ");
            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline character after nextInt()

            switch (choice) {
                case 1:
                    contactManager.addContact();
                    break;
                case 2:
                    contactManager.viewContacts();
                    break;
                case 3:
                    contactManager.editContact();
                    break;
                case 4:
                    contactManager.deleteContact();
                    break;
                case 5:
                    contactManager.saveContacts();
                    break;
                case 6:
                    contactManager.saveContacts(); // Save contacts before exiting
                    System.out.println("Exiting Contact Manager. Goodbye!");
                    scanner.close();
                    System.exit(0);
                    break;
                default:
                    System.out.println("Invalid choice. Please enter a number from 1 to 6.");
            }
        }
    }
}
