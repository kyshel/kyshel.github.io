---
layout: default
---






# Run java in shell

Run java code in shell is simple.

Before start, please make sure you has installed jdk.  
In CentOS7, run command below(here is 1.8.0):

    # yum install java-1.8.0-devel

Then, Let's begin.

1. Create new file named Box.class and add below code:
    
		public class Box {
			public static void main(String[] args){
				System.out.println("Hello, world !");
			}
		}
2. Run  

		# javac Box.java


		
3. Run  

		# java Box


4.  You will see the console print `Hello,world!`
