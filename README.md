# Hill-Cipher

import java.util.*;

public class Hill {
	
	public static void main(String [] arg){
		int choice;
		int [] top = new int [2];
		int [] bottom = new int [2];
		
		int determinedant;
		
		
		System.out.println("So this is for Hill Cipher");
		Scanner keyboard = new Scanner(System.in);
		
		System.out.println("Let's start by making a matrix together");
		
		System.out.println("please enter a number between 0 and 26 for the top left of the matrix");		
		top[0] = keyboard.nextInt();
		
		System.out.println("please enter a number between 0 and 26 for the top right of the matrix");
		top[1] = keyboard.nextInt();
		
		System.out.println("please enter a number between 0 and 26 for the bottom left of the matrix");
		bottom[0] = keyboard.nextInt();
		
		
		System.out.println("please enter a number between 0 and 26 for the bottom right of the matrix");
		bottom[1] = keyboard.nextInt();
				
		if(((top[0] < 26) && (top[0] >0)) && ((top[1] < 26) && (top[1] >0)) && ((bottom[0] < 26) && (bottom[0] >0)) && ((bottom[1] < 26) && (bottom[1] >0))){
			determinedant = ((top[0]*bottom[1])-(top[1]*bottom[0])%26);
			System.out.println("this is the determinent:" + determinedant);
			
			if((determinedant % 2) == 1 && determinedant != 13){
				System.out.println("your determinent is valid");
				System.out.println("here is your success matrix");
				
				System.out.println(Arrays.toString(top));
				System.out.println(Arrays.toString(bottom));
				
				System.out.println("Now please enter 1 for encryption or 2 for decryption");
				choice = keyboard.nextInt();
					if(choice == 1){
						encrypt(top, bottom);
					}
					else if(choice ==2){
						decrypt(top, bottom,determinedant);
					}
					else{
						System.out.println("go fuck yourself");
			}
				
			}
			else{
				System.out.println("the determinent isn't coprime to 26 its not gonna work");
			}
		}
		else{
			System.out.println("NO NO NO");
		}
	}
	
	
	
	public static void encrypt(int [] top,int [] bottom){
		int p;
		ArrayList<Integer> cipher = new ArrayList<Integer>();
		ArrayList<Character> plain = new ArrayList<Character>();
		
	
		
		System.out.println("please enter your plaintext in lowercase letters");
		Scanner keyboard = new Scanner(System.in);
		String ct = keyboard.nextLine();


		for(int j = 0; j<ct.length(); j++){
			char a = ct.charAt(j); 
			p = (int)(a-97);
			cipher.add(p);			
		}
		for(int b = 0; b<cipher.size();b+=2){
			int q = (((top[0]*cipher.get(b))+(top[1]*cipher.get(b+1))) % 26);
			int e = (((bottom[0]*cipher.get(b))+(bottom[1]*cipher.get(b+1))) % 26);
			char w = (char)(q+97);
			char y = (char)(e+97);
			plain.add(w);
			plain.add(y);
		}
		System.out.println(plain);
	}		
	
	
	
	public static void decrypt(int [] top,int [] bottom,int determinedant) {
		int multiverse = 0;
		int [] itop = new int[2];
		int [] ibottom = new int[2];
		int p;
		ArrayList<Integer> cipher = new ArrayList<Integer>();
		ArrayList<Character> plain = new ArrayList<Character>();
		
		
		
		for(int i = 0; i<26;i++){
			if((i*determinedant)%26 == 1){
				multiverse = i;
				System.out.println("this is your multiplicative inverse:" + multiverse);
			}
		}
				
		
		itop[0] = (multiverse *bottom[1]) %26;
		if (itop[0] < 0){
			int temp1 = 26 - Math.abs((multiverse * bottom[1])) % 26 ; 
			itop[0] = temp1; 
		} else {
			itop[0] = ((multiverse *bottom[1]) %26);
		}
		
		itop[1] = ((top[1]*-1)*multiverse)%26;
		if (itop[1] < 0){
			int temp2 = 26 - Math.abs((top[1]*-1)*multiverse)%26; 
			itop[1] = temp2; 
		} else {
			itop[1] = ((top[1]*-1)*multiverse)%26;
		}
		
		ibottom[0] = ((bottom[0]*-1)*multiverse)%26;
		if (ibottom[0] < 0){
			int temp3 = 26 - Math.abs((bottom[0]*-1)* multiverse)%26; 
			ibottom[0] = temp3; 
		} else {
			ibottom[0] = ((bottom[0]*-1)*multiverse)%26;
		}
		
		ibottom[1] = ((top[0])*multiverse)%26;
		if (ibottom[1] < 0){
			int temp4 = 26 - Math.abs((top[0])*multiverse)%26; 
			ibottom[1] = temp4; 
		} else {
			ibottom[1] = ((top[0])*multiverse)%26;
		}
		
		System.out.println("here is your inverse matrix");
		System.out.println(Arrays.toString(itop));
		System.out.println(Arrays.toString(ibottom));
		
		System.out.println("please enter your ciphertext in uppercase letters");
		Scanner keyboard = new Scanner(System.in);
		String ct = keyboard.nextLine();


		for(int j = 0; j<ct.length(); j++){
			char a = ct.charAt(j); 
			p = (int)(a-65);
			cipher.add(p);			
		}
		for(int b = 0; b<cipher.size();b+=2){
			int q = (((itop[0]*cipher.get(b))+(itop[1]*cipher.get(b+1))) % 26);
			int e = (((ibottom[0]*cipher.get(b))+(ibottom[1]*cipher.get(b+1))) % 26);
			char w = (char)(q+65);
			char y = (char)(e+65);
			plain.add(w);
			plain.add(y);
		}
		System.out.println(plain);
	}		
}
