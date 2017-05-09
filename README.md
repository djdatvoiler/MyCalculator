# MyCalculator
I wrote this Java program to build the calculator mimicking the windows calculator.


import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import javax.swing.*;


public class CalculatorGUI implements KeyListener,ActionListener{
	static double result = 0;
	static int intResult = 0;
	static String resultContains = "";
	JPanel[] row = new JPanel[6];
	JButton[] button = new JButton[19];
	String[] buttonString = {"7","8","9","+","C","4","5","6","-","\u221A",
						"1","2","3","*","+/-",".","/","=","0"};
	
	int[] dimW = {310,53,100,110};
	int[] dimH = {35,40};
	
	Dimension displayDimension = new Dimension(dimW[0],45);
	Dimension regularDimension = new Dimension(dimW[1],dimW[1]);
	Dimension zeroButton = new Dimension(dimW[3],53);
	
	boolean[] function = new boolean[4];
	
	ArrayList<Double> number = new ArrayList<Double>();
	ArrayList<String> operand = new ArrayList<String>();
	ArrayList<Object> ae = new ArrayList<Object>(); //Use to store Action Events
	
	JTextField display = new JTextField(20);
	JTextField displayHistory = new JTextField(20);
	JFrame frame = new JFrame("Calculator");
	JPanel panel = new JPanel();
	Font font = new Font("Times new Roman", Font.BOLD, 14);
	//UI Creation
	CalculatorGUI(){			
		frame.setResizable(false);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.setVisible(true);
		frame.setSize(380,450);		
		GridLayout layout = new GridLayout(6,5,2,0);		
		panel.setLayout(layout);	
		for(int i=0;i<4;i++)
			function[i] = false;
		
		FlowLayout f1 = new FlowLayout(FlowLayout.CENTER);
		FlowLayout f2 = new FlowLayout(FlowLayout.CENTER,4,1);//4 for horizontal and 1 for vertical gap
		
		for(int i=0;i<6;i++)
			row[i] = new JPanel();
		
		row[0].setLayout(f1);
		row[1].setLayout(f1);
		
		for(int i=2;i<6;i++)
			row[i].setLayout(f2);
		
		for(int i=0;i<19;i++){
			button[i] = new JButton();
			button[i].setText(buttonString[i]);
			button[i].setFont(font);
			button[i].addActionListener(this);
		}		
		displayHistory.setEditable(false);	
		displayHistory.setHorizontalAlignment(JTextField.RIGHT);		
		displayHistory.setPreferredSize(new Dimension(dimW[0],35));		
		display.setEditable(false);
		display.setHorizontalAlignment(JTextField.RIGHT);		
		display.setPreferredSize(displayDimension);
		display.setFont(font);
		
		for(int i=0;i<18;i++)
			button[i].setPreferredSize(regularDimension);			
		
		button[18].setPreferredSize(zeroButton);
		
		
		row[0].add(displayHistory);
		panel.add(row[0]);
		row[1].add(display);
		panel.add(row[1]);
		
		int k=0;
		for(int i=2;i<5;i++){
			for(int j=0;j<5;j++){
				row[i].add(button[k]);
				k++;
			}
			panel.add(row[i]);
		}
		row[5].add(button[18]);
		for(int i=15;i<18;i++)
			row[5].add(button[i]);
		
		panel.add(row[5]);		
		display.setText("0");
		
		display.addKeyListener(this); //Key Listener 
		
		frame.getContentPane().add(panel);
		display.requestFocusInWindow();
	}
	
	//UI finish
	@Override
	public void keyPressed(KeyEvent arg0) {
		// TODO Auto-generated method stub
		
	}
	@Override
	public void keyReleased(KeyEvent arg0) {
		// TODO Auto-generated method stub
		
	}
	@Override
	public void keyTyped(KeyEvent ke) {
		
		System.out.println("Testing");
			if(ke.getKeyChar() == '7'){
				System.out.println("come on");
				display.setText("7");
			}		
	}
	
	public void clear(){
		try{
			displayHistory.setText("");
			number.clear();
			display.setText("0");
			operand.clear();
			ae.clear();
			result = 0;
			intResult = 0;
			resultContains = "";
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	public void getPosNeg(){
		double value = Double.parseDouble(display.getText());
		try{
			if(value !=0)
			{
			value = -1 * value;
			display.setText(Double.toString(value));
			}
			
		}catch(Exception ex){
			ex.printStackTrace();
		}
	}
	
	 public void getSqrt() {

	        try {
	            double value = Math.sqrt(Double.parseDouble(display.getText()));
	            display.setText(Double.toString(value));
	        } catch(NumberFormatException e) {
	        	e.printStackTrace();
	        }
	    }
	 public void firstOperand(String op){
		
			// displayHistory.setText(op+" " + Integer.parseInt(display.getText()));
			 switch (op) {
			 case "+": result = number.get(number.size() -2) + number.get(number.size() -1);
			 			break;
			 case "-": result = number.get(number.size() -2) - number.get(number.size() -1);
			 			break;
			 case "*": result = number.get(number.size() -2) * number.get(number.size() -1);
	 					break;
			 case "/": result = number.get(number.size() -2) / number.get(number.size() -1);
	 					break;
			}
			 number.add(result);
			 //If there is point, show double and if there is not show int
			 //intResult = (int) result;
			// resultContains = Double.toString(result);
			// if(resultContains.contains(".")){		
				 display.setText(Double.toString(result));
			// }
			// else
			//	 display.setText(Integer.toString(intResult));	 
		 }
	
public boolean checkingButtons(){
if(ae.get(ae.size()-1).equals(button[0]) || 
						ae.get(ae.size()-1).equals(button[1]) ||
						ae.get(ae.size()-1).equals(button[2]) ||
						ae.get(ae.size()-1).equals(button[5]) ||
						ae.get(ae.size()-1).equals(button[6]) ||
						ae.get(ae.size()-1).equals(button[7]) ||
						ae.get(ae.size()-1).equals(button[10]) ||
						ae.get(ae.size()-1).equals(button[11]) ||
						ae.get(ae.size()-1).equals(button[12]) ||
						ae.get(ae.size()-1).equals(button[15])){
	return true;
}
else
	return false;
}
	
	public void actionPerformed(ActionEvent ev) {
		if (ev.getSource() == button[0]){
			
			if(ae.size()>0){
				if(checkingButtons()){					
					display.setText(display.getText()+"7");					
				}
				else{
					display.setText("7");				
				}
			}
				else
					display.setText("7");
			ae.add(ev.getSource());
			}
		
		if(ev.getSource() == button[1]){			
			
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"8");
				}
				else{
					display.setText("8");				
				}
				}
				else
					display.setText("8");
			ae.add(ev.getSource());
		}
		if(ev.getSource() == button[2]){			
			
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"9");
				}
				else{
					display.setText("9");				
				}
				}
				else
					display.setText("9");
			ae.add(ev.getSource());
		}
		//Addition button
		if(ev.getSource() == button[3]){			
				number.add(Double.parseDouble(display.getText()));				
				operand.add("+");				
				ae.add(ev.getSource());
				displayHistory.setText(displayHistory.getText()+Double.toString(number.get(number.size()-1)));				
				displayHistory.setText(displayHistory.getText()+" "+operand.get(operand.size()-1) +" " );
				
			if(operand.size()<2){
				
			}
		}
		if(ev.getSource() == button[4])
			clear();
		if(ev.getSource() == button[5]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"4");
				}
				else{
					display.setText("4");				
				}
				}
				else
					display.setText("4");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[6]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"5");
				}
				else{
					display.setText("5");				
				}
				}
				else
					display.setText("5");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[7]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"6");
				}
				else{
					display.setText("6");				
				}
				}
				else
					display.setText("6");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[8]){
			number.add(Double.parseDouble(display.getText()));				
			operand.add("-");
			ae.add(ev.getSource());
			displayHistory.setText(displayHistory.getText()+Double.toString(number.get(number.size()-1)));				
			displayHistory.setText(displayHistory.getText()+" "+operand.get(number.size()-1) +" " );
		}
		if(ev.getSource() == button[9])
			getSqrt();
		if(ev.getSource() == button[10]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"1");
				}
				else{
					display.setText("1");				
				}
				}
				else
					display.setText("1");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[11]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"2");
				}
				else{
					display.setText("2");				
				}
				}
				else
					display.setText("2");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[12]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"3");
				}
				else{
					display.setText("3");				
				}
				}
				else
					display.setText("3");
			ae.add(ev.getSource());
		}
			
		if(ev.getSource() == button[13]){
			number.add(Double.parseDouble(display.getText()));				
			operand.add("*");				
			ae.add(ev.getSource());
			displayHistory.setText(displayHistory.getText()+Double.toString(number.get(number.size()-1)));				
			displayHistory.setText(displayHistory.getText()+" "+operand.get(operand.size()-1) +" " );			
	}
		if(ev.getSource() == button[14]){
			ae.add(ev.getSource());
			getPosNeg();
		}
		if(ev.getSource() == button[15]){
			display.setText(display.getText()+".");
			ae.add(ev.getSource());
		}
		if(ev.getSource() == button[16]){
			number.add(Double.parseDouble(display.getText()));				
			operand.add("/");				
			ae.add(ev.getSource());
			displayHistory.setText(displayHistory.getText()+Double.toString(number.get(number.size()-1)));				
			displayHistory.setText(displayHistory.getText()+" "+operand.get(operand.size()-1) +" " );			
	}
		if(ev.getSource() == button[17]){		//=	
			ae.add(ev.getSource());
			number.add(Double.parseDouble(display.getText()));
			firstOperand(operand.get(operand.size()-1));
			displayHistory.setText(" ");
		}	
		if(ev.getSource() == button[18]){
			if(ae.size()>0){
				if(checkingButtons()){
					display.setText(display.getText()+"0");
				}
				else{
					display.setText("0");				
				}
				}
				else
					display.setText("0");
			ae.add(ev.getSource());
		}
	}
	
	public static void main(String[] args){
		CalculatorGUI c = new CalculatorGUI();
		
		}
}//Calculator GUI class ends


	
