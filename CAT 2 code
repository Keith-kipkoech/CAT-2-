import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.sql.*;

public class RegistrationForm extends JFrame {

    private JTextField nameField, mobileField, addressField;
    private JRadioButton maleRadio, femaleRadio;
    private JTextArea displayArea;
    private Connection connection; // Database connection

    public RegistrationForm() {
        setTitle("Registration Form");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new GridLayout(7, 2));

        // Initialize components
        nameField = new JTextField(20);
        mobileField = new JTextField(20);
        addressField = new JTextField(20);
        maleRadio = new JRadioButton("Male");
        femaleRadio = new JRadioButton("Female");
        ButtonGroup genderGroup = new ButtonGroup();
        genderGroup.add(maleRadio);
        genderGroup.add(femaleRadio);
        displayArea = new JTextArea(10, 20);
        displayArea.setEditable(false);

        // Add components to the frame
        add(new JLabel("Name:"));
        add(nameField);
        add(new JLabel("Mobile:"));
        add(mobileField);
        add(new JLabel("Gender:"));
        JPanel genderPanel = new JPanel(new FlowLayout(FlowLayout.LEFT));
        genderPanel.add(maleRadio);
        genderPanel.add(femaleRadio);
        add(genderPanel);
        add(new JLabel("Address:"));
        add(addressField);

        JButton registerButton = new JButton("Register");
        add(registerButton);
        add(new JLabel("")); // Placeholder for layout
        add(new JScrollPane(displayArea));

        // Connect to the database
        try {
            String url = "jdbc:mysql://localhost:3306/your_database_name";
            String username = "your_username";
            String password = "your_password";
            connection = DriverManager.getConnection(url, username, password);
        } catch (SQLException e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(this, "Database connection failed!");
        }

        // Add action listener to the register button
        registerButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                registerUser();
            }
        });

        pack();
        setLocationRelativeTo(null);
        setVisible(true);
    }

    private void registerUser() {
        String name = nameField.getText();
        String mobile = mobileField.getText();
        String gender = maleRadio.isSelected() ? "Male" : "Female";
        String address = addressField.getText();

        // Input validation
        if (name.isEmpty() || mobile.isEmpty() || (!maleRadio.isSelected() && !femaleRadio.isSelected()) || address.isEmpty()) {
            JOptionPane.showMessageDialog(this, "Please fill in all fields.");
            return;
        }

        // Database insertion
        String sql = "INSERT INTO users (name, mobile, gender, address) VALUES (?, ?, ?, ?)";
        try (PreparedStatement preparedStatement = connection.prepareStatement(sql)) {
            preparedStatement.setString(1, name);
            preparedStatement.setString(2, mobile);
            preparedStatement.setString(3, gender);
            preparedStatement.setString(4, address);
            int rowsAffected = preparedStatement.executeUpdate();
            if (rowsAffected > 0) {
                JOptionPane.showMessageDialog(this, "User registered successfully!");
                displayData();
            } else {
                JOptionPane.showMessageDialog(this, "Registration failed!");
            }
        } catch (SQLException e) {
            e.printStackTrace();
            JOptionPane.showMessageDialog(this, "Database error: " + e.getMessage());
        }
    }

    private void displayData() {
     displayArea.setText(""); // Clear previous text
    String sql = "SELECT * FROM users";
    try (Statement statement = connection.createStatement();
         ResultSet resultSet = statement.executeQuery(sql)) {
        StringBuilder data = new StringBuilder();
        while (resultSet.next()) {
            data.append("ID: ").append(resultSet.getInt("id")).append(", ");
            data.append("Name: ").append(resultSet.getString("name")).append(", ");
            data.append("Mobile: ").append(resultSet.getString("mobile")).append(", ");
            data.append("Gender: ").append(resultSet.getString("gender")).append(", ");
            data.append("Address: ").append(resultSet.getString("address")).append("\n");
        }
        displayArea.setText(data.toString());
    } catch (SQLException e) {
        e.printStackTrace();
        JOptionPane.showMessageDialog(this, "Error retrieving data: " + e.getMessage());
    }
}


    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new RegistrationForm();
            }
        });
    }
}
