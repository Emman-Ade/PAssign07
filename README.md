/**File: PAssign08.java
 * Class: CSCI 1302
 * Author: Emmanuel Adeniyi
 * Created on : Oct 31, 2023
 * Last Updated on: Nov 17, 2023
 * Description: Creating a IPhone keypad using KeyPadPane class as base, and creating a KeyPadCustomPane to add further user interaction
   Link to Github: https://github.com/Emman-Ade/hello-world
 */
package keypad;

import java.io.File;
import java.net.MalformedURLException;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.StackPane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;
import javafx.scene.control.TextField;


public class PAssign08 extends Application {

	@Override
	public void start(Stage primaryStage) throws MalformedURLException {

		//label that will hold our phone Number values
		Label lbNumbers = new Label();
		//new code: creating a KeyPadCustomPane that has our info from KeyPadPane and our new custom pane we created
		KeyPadCustomPane keypad = new KeyCustomPane();
		//using a GridPane to display our values
		GridPane main = new GridPane();
		//creating a File that contains a phone Image, and displaying in our ImageView
		File imgFile = new File("src/keypad/phoneIMG.jpg");
		Image imgCall = new Image(imgFile.toURL().toString());
		ImageView viewCall = new ImageView(imgCall);

		//setting the size of our image
		viewCall.setFitHeight(50);
		viewCall.setFitWidth(50);

		//creating a button that will delete a number value when pressed
		Button buttonBack = new Button("x");

		//arrays of buttons using our created 
		Button[] buttonLabels = {keypad.btn1,keypad.btn2,keypad.btn3,keypad.btn4,keypad.btn5,keypad.btn6,keypad.btn7,keypad.btn8,keypad.btn9,keypad.btnAsterisk,keypad.btn0,keypad.btnPound};

		//StringBuilder that holds all our phoneNumber values
		StringBuilder phoneNumber = new StringBuilder();

		//iterating to create all of our circles and display all of our buttons
		for(int i = 0; i < buttonLabels.length; i++) {
			Circle circle = new Circle(20);
			circle.setFill(Color.GREEN);

			//StackPane to create buttons on top of our Green Circles we've created
			StackPane stack = new StackPane();
			stack.getChildren().addAll(circle,buttonLabels[i]);

			//finding the rows and columns of each number
			int row = i / 3;
			int col = i % 3;
			//adding them to our GridPane
			main.add(stack,col,row);

			//our buttonNumbers(i starts at 0, so + 1 to start at the correct value)
			final int buttonNumber = i + 1;

			//actionEvent for our buttons, using lambda expression
			buttonLabels[i].setOnAction(e -> {
				//only going up to 10 digits xxx-xxx-xxxx format phone number
				if(phoneNumber.length() <= 11) {
					//add a dash between values once enough digits have been inputed
					if(phoneNumber.length() == 3 || phoneNumber.length() == 7) {
						phoneNumber.append('-');
					}
					//setting the correct values of *, 0, and # and not the values they got from iteration, the other values are the same
					if(buttonNumber == 10) {
						phoneNumber.append('*');
					}else if(buttonNumber == 11) {
						phoneNumber.append('0');
					}else if(buttonNumber == 12) {
						phoneNumber.append('#');
					}else {
						phoneNumber.append(buttonNumber);
					}
					//after all that we take our phoneNumber values and print them out
					lbNumbers.setText(phoneNumber.toString());

				}


			});

			//ActionEvent to delete buttons using lambda expression once again
			buttonBack.setOnAction(e -> {
				//only working if any phoneNumber values have been pressed
				if(phoneNumber.length() > 0) {
					//delete the latest character
					phoneNumber.deleteCharAt(phoneNumber.length() - 1);
					//then display the phoneNumber value
					lbNumbers.setText(phoneNumber.toString());
				}
			});
		}
		//adding our x button, our image, and our phoneNumber labels to our GridPane using ideal formatting
		main.add(buttonBack, 6, 3,3,1);
		main.add(viewCall, 1, 4,4,1);
		main.add(lbNumbers, 5, 5, 5,1);
  
  		//new Code: adding our new KeyPad elements to our GridPane
  		main.add(keypad,7,6,3,5);

		//creating a scene to hold GridPane, with our desired size
		Scene scene = new Scene(main,400,400);

		primaryStage.setTitle("Phone Pad");
		//setting our Stage's scene to said scene we just created
		primaryStage.setScene(scene);
		//showing our stage
		primaryStage.show();

	}
	public static void main(String[] args) {
		//main method to launch JavaFX
		launch(args);
	}
}

//new code, creating a custom keypad that adds contacts
class KeyPadCustomPane extends KeyPadPane{
	//initalizing textfields to input data
	private TextField contactTxt;
	private TextField numberTxt;

	//default constructor that calls createInfo and createContacts method so we can have access to them in PAssign08
	public KeyPadCustomPane() {
		createInfo();
		createContacts();
	}
	private void createInfo() {
		//creating our textfields
		contactTxt = new TextField();
		numberTxt = new TextField();

		//setting the width so it can be large enough for users
		contactTxt.setMinWidth(200);
		contactTxt.setMaxWidth(Double.MAX_VALUE);
		contactTxt.setPrefWidth(200);

		numberTxt.setMinWidth(200);
		numberTxt.setMaxWidth(Double.MAX_VALUE);
		numberTxt.setPrefWidth(200);
	}

	//creating our contacts
	private void createContacts() {
		//label that shows our contacts
		Label lbContacts = new Label();

		//adding all our new elements to our keypad using this, and with proper formatting
		this.add(new Label("Contact Name: "),0,6);
		this.add(new Label("Phone Number: "),0,7);
		this.add(contactTxt,1,6,2,1);
		this.add(numberTxt,1,7,2,1);
		this.add(lbContacts, 0, 9, 3, 1);
		Button buttonContact = new Button("Add Contact");
		this.add(buttonContact,0,8,2,1);

		//Action Event if button is pressed
		buttonContact.setOnAction(e ->{

			//getting the text from contactTxt and numberTxt
			String contactName = contactTxt.getText();
			String contactNumber = numberTxt.getText();

			//error checking if any information is inputed by user
			if(contactName != null && contactNumber != null && !contactName.isEmpty() && !contactNumber.isEmpty()) {
				//call allContacts method to print it to main console
				allContacts(contactName, contactNumber);

				//set the text for lbContacts to print info to GUI Application
				lbContacts.setText("Contact Name: " + contactName + "\nContact Number: " + contactNumber);
			}else {
				//if nothing is inputed give this message
				lbContacts.setText("Insert name and number");
			}

			//after button is pressed it clears the contents of the two textfields
			contactTxt.clear();
			numberTxt.clear();
		});
	}

	//allContacts method for printing out our information to java
	private void allContacts(String contactName, String contactNumber) {
		System.out.println("Contact Name: " + contactName);
		System.out.println("Contact Number: " + contactNumber);
	}


}

