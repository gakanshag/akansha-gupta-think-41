
import javax.swing.*;
import javax.swing.table.DefaultTableModel;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.List;

// User class
class User {
    String name;
    int age;
    String location;

    User(String name, int age, String location) {
        this.name = name;
        this.age = age;
        this.location = location;
    }

    Object[] toRow() {
        return new Object[]{name, age, location};
    }
}

public class UserFilterUI extends JFrame {
    private final List<User> allUsers = new ArrayList<>();
    private final DefaultTableModel tableModel = new DefaultTableModel(new String[]{"Name", "Age", "Location"}, 0);
    private final JTextField nameField = new JTextField();
    private final JTextField ageField = new JTextField();
    private final JTextField locationField = new JTextField();

    public UserFilterUI() {
        setTitle("User Filter App");
        setSize(600, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Sample data
        allUsers.add(new User("Akansha", 22, "Delhi"));
        allUsers.add(new User("Raj", 25, "Mumbai"));
        allUsers.add(new User("Neha", 22, "Delhi"));
        allUsers.add(new User("Amit", 30, "Pune"));
        allUsers.add(new User("Sneha", 28, "Bangalore"));

        // Filter UI
        JPanel filterPanel = new JPanel(new GridLayout(2, 3, 8, 5));
        filterPanel.add(new JLabel("Name:"));
        filterPanel.add(new JLabel("Age:"));
        filterPanel.add(new JLabel("Location:"));
        filterPanel.add(nameField);
        filterPanel.add(ageField);
        filterPanel.add(locationField);

        JButton filterButton = new JButton("Apply Filters");
        filterButton.addActionListener(e -> filterUsers());

        // Table
        JTable table = new JTable(tableModel);
        add(filterPanel, BorderLayout.NORTH);
        add(new JScrollPane(table), BorderLayout.CENTER);
        add(filterButton, BorderLayout.SOUTH);

        loadTableData(allUsers);
        setVisible(true);
    }

    private void filterUsers() {
        String nameText = nameField.getText().trim().toLowerCase();
        String ageText = ageField.getText().trim();
        String locationText = locationField.getText().trim().toLowerCase();

        tableModel.setRowCount(0); // Clear previous rows

        for (User user : allUsers) {
            boolean matches = true;

            if (!nameText.isEmpty() && !user.name.toLowerCase().contains(nameText)) matches = false;
            if (!ageText.isEmpty()) {
                try {
                    int ageInput = Integer.parseInt(ageText);
                    if (user.age != ageInput) matches = false;
                } catch (NumberFormatException ignored) {
                    continue; // skip invalid input
                }
            }
            if (!locationText.isEmpty() && !user.location.toLowerCase().contains(locationText)) matches = false;

            if (matches) tableModel.addRow(user.toRow());
        }
    }

    private void loadTableData(List<User> users) {
        tableModel.setRowCount(0);
        for (User user : users) {
            tableModel.addRow(user.toRow());
        }
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(UserFilterUI::new);
    }
}
