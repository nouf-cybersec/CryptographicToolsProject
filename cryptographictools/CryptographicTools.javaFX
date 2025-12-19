package cryptographictools;

import javafx.application.Application;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.effect.DropShadow;
import javafx.scene.layout.*;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.stage.Stage;
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;

public class CryptographicTools extends Application {
    
    private Stage primaryStage;
    private Scene mainScene;

    public static void main(String[] args) {
        launch(args);
    }

    @Override
    public void start(Stage stage) {
        this.primaryStage = stage;

        VBox mainLayout = new VBox(20);
        mainLayout.setAlignment(Pos.CENTER);
        mainLayout.setPadding(new Insets(20));
        mainLayout.setStyle("-fx-background-color: #2C3E50;");

        Label welcome = new Label("Select Cipher Technique");
        welcome.setFont(new Font("Arial", 22));
        welcome.setTextFill(Color.web("#ECF0F1"));
        welcome.setEffect(new DropShadow(2, Color.GRAY));

        ComboBox<String> cipherSelector = new ComboBox<>();
        cipherSelector.getItems().addAll("Morse Code", "AES", "Rail Fence");
        cipherSelector.setStyle("-fx-font-size: 16px;");
        cipherSelector.setPromptText("Choose encryption method");

        Button proceed = new Button("Start");
        proceed.setStyle("-fx-background-color: #3498DB; -fx-text-fill: white; -fx-font-weight: bold; -fx-padding: 10px 20px;");

        proceed.setOnAction(e -> {
            String choice = cipherSelector.getValue();
            if (choice != null) {
                switch (choice) {
                    case "Morse Code":
                        showMorseCodeUI();
                        break;
                    case "AES":
                        showAESUI();
                        break;
                    case "Rail Fence":
                        showRailFenceUI();
                        break;
                }
            }
        });

        mainLayout.getChildren().addAll(welcome, cipherSelector, proceed);
        mainScene = new Scene(mainLayout, 500, 400);
        primaryStage.setScene(mainScene);
        primaryStage.setTitle("Multi-Cipher Tool");
        primaryStage.show();
    }

    // ================= Morse UI ===================
    private static final String[] morseCode = {
        ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..", ".---", "-.-",
        ".-..", "--", "-.", "---", ".--.", "--.-", ".-.", "...", "-", "..-", "...-",
        ".--", "-..-", "-.--", "--..",
        "-----", ".----", "..---", "...--", "....-", ".....", "-....", "--...",
        "---..", "----."
    };

    private static final char[] alphabet = "ABCDEFGHIJKLMNOPQRSTUVWXYZ".toCharArray();
    private static final char[] numbers = "0123456789".toCharArray();

    public static String encryptMorse(String message) {
        String encryptedMessage = "";
        message = message.toUpperCase();
        for (int i = 0; i < message.length(); i++) {
            char currentChar = message.charAt(i);
            if (currentChar == ' ') {
                encryptedMessage += " / ";
            } else {
                int index = -1;
                if (currentChar >= 'A' && currentChar <= 'Z') {
                    index = currentChar - 'A';
                } else if (currentChar >= '0' && currentChar <= '9') {
                    index = currentChar - '0' + 26;
                }
                if (index != -1) {
                    encryptedMessage += morseCode[index] + " ";
                }
            }
        }
        return encryptedMessage.trim();
    }

    private static String decryptMorse(String morseMessage) {
        StringBuilder decryptedMessage = new StringBuilder();
        String[] morseWords = morseMessage.split(" / ");
        for (String word : morseWords) {
            String[] morseChars = word.split(" ");
            for (String morseChar : morseChars) {
                int index = -1;
                for (int i = 0; i < morseCode.length; i++) {
                    if (morseCode[i].equals(morseChar)) {
                        index = i;
                        break;
                    }
                }
                if (index != -1) {
                    if (index < 26) {
                        decryptedMessage.append(alphabet[index]);
                    } else {
                        decryptedMessage.append(numbers[index - 26]);
                    }
                }
            }
            decryptedMessage.append(" ");
        }
        return decryptedMessage.toString().trim();
    }

    private void showMorseCodeUI() {
        VBox layout = getStyledLayout("Morse Code Tool");

        TextArea input = createTextArea();
        TextArea output = createTextArea();
        output.setEditable(false);

        Button encrypt = new Button("🔒 Encrypt");
        Button decrypt = new Button("🔓 Decrypt");
        styleButton(encrypt, "#1ABC9C");
        styleButton(decrypt, "#FF9800");

        encrypt.setOnAction(e -> output.setText(encryptMorse(input.getText().trim())));
        decrypt.setOnAction(e -> output.setText(decryptMorse(input.getText().trim())));

        HBox buttonBox = new HBox(20, encrypt, decrypt);
        buttonBox.setAlignment(Pos.CENTER);
        
        layout.getChildren().addAll(
                createLabel("Enter Text:"), input,
                createLabel("Result:"), output,
                buttonBox
        );

        primaryStage.setScene(new Scene(layout, 500, 400));
    }

    // ================= AES UI ===================
    private static final String ALGORITHM = "AES";
    private static final String KEY = "1234567890123456";

    public static String encryptAES(String plainText) throws Exception {
        SecretKeySpec key = new SecretKeySpec(KEY.getBytes(), ALGORITHM);
        Cipher cipher = Cipher.getInstance(ALGORITHM);
        cipher.init(Cipher.ENCRYPT_MODE, key);
        byte[] encryptedBytes = cipher.doFinal(plainText.getBytes());
        return Base64.getEncoder().encodeToString(encryptedBytes);
    }

    public static String decryptAES(String encryptedText) throws Exception {
        SecretKeySpec key = new SecretKeySpec(KEY.getBytes(), ALGORITHM);
        Cipher cipher = Cipher.getInstance(ALGORITHM);
        cipher.init(Cipher.DECRYPT_MODE, key);
        byte[] decryptedBytes = cipher.doFinal(Base64.getDecoder().decode(encryptedText));
        return new String(decryptedBytes);
    }

    private void showAESUI() {
        VBox layout = getStyledLayout("AES Encryption/Decryption Tool");

        TextArea input = createTextArea();
        TextArea output = createTextArea();
        output.setEditable(false);

        Button encrypt = new Button("🔒 Encrypt");
        Button decrypt = new Button("🔓 Decrypt");
        styleButton(encrypt, "#1ABC9C");
        styleButton(decrypt, "#FF9800");

        encrypt.setOnAction(e -> {
            try {
                output.setText(encryptAES(input.getText().trim()));
            } catch (Exception ex) {
                output.setText("Encryption Error: " + ex.getMessage());
            }
        });

        decrypt.setOnAction(e -> {
            try {
                output.setText(decryptAES(input.getText().trim()));
            } catch (Exception ex) {
                output.setText("Decryption Error: " + ex.getMessage());
            }
        });

       HBox buttonBox = new HBox(20, encrypt, decrypt);
        buttonBox.setAlignment(Pos.CENTER);
        
        layout.getChildren().addAll(
                createLabel("Enter Text:"), input,
                createLabel("Result:"), output,
                buttonBox
        );

        primaryStage.setScene(new Scene(layout, 500, 400));
    }

    // ================= Rail Fence UI ===================
    private char[][] matrix;

    public String encryptRail(String plainText, int key) {
        return doCypher(plainText, key, true);
    }

    public String decryptRail(String cipherText, int key) {
        return doCypher(cipherText, key, false);
    }

    private String doCypher(String s, int key, boolean encrypt) {
        matrix = new char[key][s.length()];
        StringBuilder sb = new StringBuilder();
        boolean moveDown = true;

        int row = 0;
        int c = 0;

        for (int col = 0; col < s.length(); col++) {
            matrix[row][col] = s.charAt(col);
            row += moveDown ? 1 : -1;
            if (row == 0 || row == key - 1) moveDown = !moveDown;
        }

        if (encrypt) {
            for (int i = 0; i < key; i++)
                for (int j = 0; j < s.length(); j++)
                    if (matrix[i][j] != 0) sb.append(matrix[i][j]);
        } else {
            for (int i = 0; i < key; i++)
                for (int j = 0; j < s.length(); j++)
                    if (matrix[i][j] != 0) matrix[i][j] = s.charAt(c++);

            for (int i = 0; i < s.length(); i++)
                for (int j = 0; j < key; j++)
                    if (matrix[j][i] != 0) sb.append(matrix[j][i]);
        }

        return sb.toString();
    }

    private void showRailFenceUI() {
        VBox layout = getStyledLayout("Rail Fence Cipher Tool");

        TextArea input = createTextArea();
        TextArea output = createTextArea();
        output.setEditable(false);

        TextField keyField = new TextField();
        keyField.setPromptText("Enter number of rails");

        Button encrypt = new Button("🔒 Encrypt");
        Button decrypt = new Button("🔓 Decrypt");
        styleButton(encrypt, "#1ABC9C");
        styleButton(decrypt, "#FF9800");

        encrypt.setOnAction(e -> {
            try {
                int key = Integer.parseInt(keyField.getText().trim());
                output.setText("Encrypted:\n" + encryptRail(input.getText().trim(), key));
            } catch (Exception ex) {
                output.setText("Error: " + ex.getMessage());
            }
        });

        decrypt.setOnAction(e -> {
            try {
                int key = Integer.parseInt(keyField.getText().trim());
                output.setText("Decrypted:\n" + decryptRail(input.getText().trim(), key));
            } catch (Exception ex) {
                output.setText("Error: " + ex.getMessage());
            }
        });

        HBox buttonBox = new HBox(20, encrypt, decrypt);
        buttonBox.setAlignment(Pos.CENTER);
        layout.getChildren().addAll(
                createLabel("Enter Text:"), input,
                createLabel("Key (Number of Rails):"), keyField,
                buttonBox,
                createLabel("Result:"), output
        );

        primaryStage.setScene(new Scene(layout, 600, 500));
    }

    // ================= UI UTIL METHODS ===================
    private VBox getStyledLayout(String titleText) {
        VBox layout = new VBox(15);
        layout.setAlignment(Pos.CENTER);
        layout.setPadding(new Insets(20));
        layout.setStyle("-fx-background-color: #2C3E50;");

        Label title = new Label(titleText);
        title.setFont(new Font("Arial", 24));
        title.setTextFill(Color.web("#ECF0F1"));
        title.setEffect(new DropShadow(3, Color.GRAY));

        layout.getChildren().add(title);
        return layout;
    }

    private Label createLabel(String text) {
        Label label = new Label(text);
        label.setFont(new Font("Arial", 14));
        label.setTextFill(Color.web("#ECF0F1"));
        return label;
    }

    private TextArea createTextArea() {
        TextArea area = new TextArea();
        area.setWrapText(true);
        area.setPrefHeight(80);
        return area;
    }

    private void styleButton(Button button, String bgColor) {
        button.setStyle("-fx-background-color: " + bgColor + "; -fx-text-fill: white; "
                + "-fx-font-size: 16px; -fx-font-weight: bold; -fx-padding: 10px 20px; "
                + "-fx-background-radius: 5px;");
    }
}
