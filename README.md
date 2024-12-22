import java.util.ArrayList;
import java.util.Scanner;

class MenuItem {
    private int id;
    private String name;
    private double price;

    public MenuItem(int id, String name, double price) {
        this.id = id;
        this.name = name;
        this.price = price;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return id + ". " + name + " - $" + price;
    }
}

class Order {
    private ArrayList<MenuItem> items;

    public Order() {
        items = new ArrayList<>();
    }

    public void addItem(MenuItem item) {
        items.add(item);
        System.out.println(item.getName() + " added to the order.");
    }

    public void removeItem(int itemId) {
        for (MenuItem item : items) {
            if (item.getId() == itemId) {
                items.remove(item);
                System.out.println(item.getName() + " removed from the order.");
                return;
            }
        }
        System.out.println("Item not found in the order.");
    }

    public void displayOrder() {
        if (items.isEmpty()) {
            System.out.println("Order is empty.");
        } else {
            System.out.println("\nCurrent Order:");
            double total = 0;
            for (MenuItem item : items) {
                System.out.println(item);
                total += item.getPrice();
            }
            System.out.println("Total: $" + total);
        }
    }

    public double calculateTotal() {
        double total = 0;
        for (MenuItem item : items) {
            total += item.getPrice();
        }
        return total;
    }

    public boolean isEmpty() {
        return items.isEmpty();
    }
}

class Restaurant {
    private ArrayList<MenuItem> menu;

    public Restaurant() {
        menu = new ArrayList<>();
        initializeMenu();
    }

    private void initializeMenu() {
        menu.add(new MenuItem(1, "Burger", 5.99));
        menu.add(new MenuItem(2, "Pizza", 8.99));
        menu.add(new MenuItem(3, "Pasta", 7.49));
        menu.add(new MenuItem(4, "Fries", 2.99));
        menu.add(new MenuItem(5, "Soda", 1.99));
    }

    public void displayMenu() {
        System.out.println("\n--- Menu ---");
        for (MenuItem item : menu) {
            System.out.println(item);
        }
    }

    public MenuItem findMenuItemById(int id) {
        for (MenuItem item : menu) {
            if (item.getId() == id) {
                return item;
            }
        }
        return null;
    }
}

public class RestaurantOrderingSystem {
    public static void main(String[] args) {
        Restaurant restaurant = new Restaurant();
        Order order = new Order();
        Scanner scanner = new Scanner(System.in);

        while (true) {
            System.out.println("\n--- Restaurant Ordering System ---");
            System.out.println("1. View Menu");
            System.out.println("2. Add Item to Order");
            System.out.println("3. Remove Item from Order");
            System.out.println("4. View Current Order");
            System.out.println("5. Checkout");
            System.out.println("6. Exit");
            System.out.print("Choose an option: ");

            int choice = scanner.nextInt();
            scanner.nextLine(); // Consume newline

            switch (choice) {
                case 1:
                    restaurant.displayMenu();
                    break;

                case 2:
                    restaurant.displayMenu();
                    System.out.print("Enter the item ID to add to your order: ");
                    int addItemId = scanner.nextInt();
                    MenuItem addItem = restaurant.findMenuItemById(addItemId);
                    if (addItem != null) {
                        order.addItem(addItem);
                    } else {
                        System.out.println("Invalid item ID.");
                    }
                    break;

                case 3:
                    order.displayOrder();
                    System.out.print("Enter the item ID to remove from your order: ");
                    int removeItemId = scanner.nextInt();
                    order.removeItem(removeItemId);
                    break;

                case 4:
                    order.displayOrder();
                    break;

                case 5:
                    order.displayOrder();
                    if (order.isEmpty()) {
                        System.out.println("Your order is empty. Cannot checkout.");
                    } else {
                        double total = order.calculateTotal();
                        System.out.println("Thank you for your order! Total: $" + total);
                        System.out.println("Order placed successfully. Goodbye!");
                        scanner.close();
                        System.exit(0);
                    }
                    break;

                case 6:
                    System.out.println("Exiting the system. Goodbye!");
                    scanner.close();
                    System.exit(0);
                    break;

                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
}
