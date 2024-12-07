# Pharmacy-management-system.java
import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.HashMap;

public class PharmacyManagementSystem extends JFrame {
    private JTextField prescriptionCodeField, medicationNameField;
    private JTextArea recordArea;
    private HashMap<String, String> prescriptions;

    public PharmacyManagementSystem() {
        prescriptions = new HashMap<>();

        // Frame settings
        setTitle("Pharmacy Management System");
        setSize(600, 400);
        setDefaultCloseOperation(EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        // Top Panel for Prescription Input
        JPanel inputPanel = new JPanel(new GridBagLayout());
        GridBagConstraints gbc = new GridBagConstraints();
        gbc.insets = new Insets(5, 5, 5, 5);
        gbc.fill = GridBagConstraints.HORIZONTAL;

        JLabel codeLabel = new JLabel("Prescription Code:");
        gbc.gridx = 0;
        gbc.gridy = 0;
        inputPanel.add(codeLabel, gbc);

        prescriptionCodeField = new JTextField(20);
        gbc.gridx = 1;
        inputPanel.add(prescriptionCodeField, gbc);

        JLabel medicationLabel = new JLabel("Medication Name:");
        gbc.gridx = 0;
        gbc.gridy = 1;
        inputPanel.add(medicationLabel, gbc);

        medicationNameField = new JTextField(20);
        gbc.gridx = 1;
        inputPanel.add(medicationNameField, gbc);

        JButton addButton = new JButton("Add Prescription");
        gbc.gridx = 0;
        gbc.gridy = 2;
        inputPanel.add(addButton, gbc);

        JButton verifyButton = new JButton("Verify Prescription");
        gbc.gridx = 1;
        inputPanel.add(verifyButton, gbc);

        // Center Panel for Record Display
        recordArea = new JTextArea(15, 40);
        recordArea.setEditable(false);
        JScrollPane scrollPane = new JScrollPane(recordArea);

        // Bottom Panel for Dispense Medication
        JPanel dispensePanel = new JPanel(new FlowLayout());
        JButton dispenseButton = new JButton("Dispense Medication");
        dispensePanel.add(dispenseButton);

        // Adding components to the Frame
        add(inputPanel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);
        add(dispensePanel, BorderLayout.SOUTH);

        // Button Action Listeners
        addButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String code = prescriptionCodeField.getText().trim();
                String medication = medicationNameField.getText().trim();

                if (code.isEmpty() || medication.isEmpty()) {
                    recordArea.append("Please enter both prescription code and medication name.\n");
                } else if (prescriptions.containsKey(code)) {
                    recordArea.append("Prescription code already exists: " + code + "\n");
                } else {
                    prescriptions.put(code, medication);
                    recordArea.append("Added Prescription: " + code + " -> " + medication + "\n");
                    prescriptionCodeField.setText("");
                    medicationNameField.setText("");
                }
            }
        });

        verifyButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String code = prescriptionCodeField.getText().trim();
                if (code.isEmpty()) {
                    recordArea.append("Please enter a prescription code to verify.\n");
                } else if (prescriptions.containsKey(code)) {
                    recordArea.append("Prescription Verified: " + code + "\n");
                } else {
                    recordArea.append("Invalid Prescription: " + code + "\n");
                }
            }
        });

        dispenseButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String code = prescriptionCodeField.getText().trim();
                if (code.isEmpty()) {
                    recordArea.append("Please enter a prescription code to dispense medication.\n");
                } else if (prescriptions.containsKey(code)) {
                    recordArea.append("Medication Dispensed: " + prescriptions.get(code) + "\n");
                    prescriptions.remove(code); // Remove after dispensing
                } else {
                    recordArea.append("No Medication Found for: " + code + "\n");
                }
            }
        });
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            PharmacyManagementSystem system = new PharmacyManagementSystem();
            system.setVisible(true);
        });
    }
}
