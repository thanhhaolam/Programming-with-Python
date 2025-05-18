# Programming with Python
## Objective: 
- Forming the understanding of design and implement solutions using unified modelling language to meet the business requirments
- Practising *Object-Oriented Programming (OOP)*
- Understanding the concepts of Classes and Inheritance

## Tasks
### 1: Create the login/signup menu for a restaurant application in respect of covid safety measures, which allows customers to place online orders
### 2: Proceeding customers' pickup and dine-in orders
### 3: transaction implementations
```
from datetime import date
import re

class User:
    
    def __init__(self):
        self.name = ""
        self.phone_number = ""
        self.password = ""
        self.dob = ""
        self.address = ""
        self.address_valid = "N"
        self.user_list = {}

    def signup(self):

        # Prompt user to enter personal information with validation and append to user list if it is valid
        self.name = input('Please enter your name: ')
        self.user_list['name'] = self.name  # Use self.user_list
        while True:
            self.phone_number = input('Please enter your mobile number: ')
            # Check if phone number is valid
            if str(self.phone_number[0]) == '0' and len(self.phone_number) == 10:
                self.user_list['phone_number']=self.phone_number  # Use self.user_list
                break
            else:  # Error message for invalid input
                print('You have entered the number in an invalid format. Please start again:')
        while True:
            self.password = input('Please enter your password: ')
            password_confirm = input('Please confirm your password: ')
            validate_pw = r'^[a-zA-Z]+[@&#]\d+$'
            # Check if password is valid or not
            if self.password == password_confirm:
                if re.match(validate_pw, self.password):
                    self.user_list['password']=self.password  # Use self.user_list
                    break
                else:
                    print('You have entered the password in an invalid format. Please start again:')
        while True:
            self.dob = input('Please enter your Date of Birth (DD/MM/YYYY): ')
            # Check if date of birth is valid
            if re.search(r'\d{2}/\d{2}/\d{4}', self.dob):
                dob = self.dob.split('/')
                year_now = date.today().year
                if int(year_now - int(dob[-1])) >= 18:
                    self.user_list['dob']=self.dob  # Use self.user_list
                    break
                else:
                    print('You have entered the birthday in an invalid format. Please start again:')
        self.address = input("Would you like to Enter your Address?(Y/N)")
        # Uppercase input in case user enters lowercase type
        self.address_valid = self.address.upper()
        if self.address_valid == "Y":
            self.address = input("Please Enter your Address:")
        return [self.name, self.phone_number, self.password, self.dob]

class Customer(User):
    def __init__(self):
        super().__init__()

    def signin(self, user_info):
        login_id = input('Please Enter your Username (Mobile Number): ')
        password = input('Please Enter your password: ')
        # Check if login id and password are matched
        while user_info[1] != login_id or user_info[2] != password:
            # If not, let user enter again
            print("You entered wrong information")
            login_id = input('Please Enter your Username (Mobile Number): ')
            password = input('Please Enter your password: ')
        print("You have successfully Signed in")


class Restaurant(User):
    def __init__(self):
        super().__init__()

class Order:
    dine_in = []
    delivery = []
    pick_up = []
    all_order = []
    total_amount = 0
    Ddate = " "
    Pdate = " "
    Deldate = " "
    id = 1
    address = " "
    Delkm = 0

class DineInOrder(Order):
    
    
    def process_order(self):
        di_amount = 0
        
        while True:
            print("Enter 1 for Noodles   Price AUD 2")
            print("Enter 2 for Sandwich  Price AUD 4")
            print("Enter 3 for Dumpling  Price AUD 6")
            print("Enter 4 for Muffins   Price AUD 8")
            print("Enter 5 for Pasta     Price AUD 10")
            print("Enter 6 for Pizza     Price AUD 20") 
            print("Enter 7 for Coffee    Price AUD 2")
            print("Enter 8 for Colddrink Price AUD 4")
            print("Enter 9 for Shake     Price AUD 6")
            print("Enter 10 for Checkout:")
     
            di_menu=int(input())
            
            #Calculate price selected item
            if di_menu == 1:
                di_amount=di_amount+2
            elif di_menu == 2:
                di_amount=di_amount+4
            elif di_menu == 3:
                di_amount=di_amount+6
            elif di_menu == 4:
                di_amount=di_amount+8
            elif di_menu == 5:
                di_amount=di_amount+10
            elif di_menu==6:
                di_amount=di_amount+20
            elif di_menu==7:
                di_amount=di_amount+2
            elif di_menu==8:
                di_amount=di_amount+4
            elif di_menu==9:
                di_amount=di_amount+6
            elif di_menu==10:
                check = input(print("Please Enter Y to proceed to Checkout or Enter N to cancel the Order:"))
                #Uppercase input in case user enter lowercase type 
                valid_check=check.upper()
                
                if valid_check == "Y":
                    print("Your total payable amount is:", di_amount * 1.15, "including AUD", di_amount * 0.15, "for Service Charges")
                    DineInOrder.dineProc()  # Call dineProc method
                    
                    # Add order details to the 'dine_in' list 
                    Order.dine_in.append("A")                         # Add order identifier
                    Order.dine_in.append(Order.id)                    # Add order ID
                    Order.dine_in.append(Order.Ddate)                 # Add order date
                    Order.dine_in.append("Dine in")                   # Add order type
                    Order.dine_in.append(di_amount * 1.15)            # Add order amount with 15% service charge
        
                    # Add order details to the 'all_order' list        
                    Order.all_order.append("A")                       # Add order identifier
                    Order.all_order.append(Order.id)                  # Add order ID
                    Order.all_order.append(Order.Ddate)               # Add order date
                    Order.all_order.append("Dine in")                 # Add order type
                    Order.all_order.append(di_amount * 1.15)          # Add order amount with 15% service charge
                    Order.total_amount += di_amount * 1.15            # Update total amount with newly added order amount
                    
                    # Confirmation message with order ID that has been padded the order ID with leading zeros if necessary to ensure it is three digits long 
                    print("Your order has been confirmed, and your order id is ‘A" + str(Order.id).zfill(3) + "'")
                    
                    # Increment order ID for the next order
                    Order.id += 1
                    break 
                elif valid_check == "N":
                    print("Order is canceled")
            else: 
                print("Invalid number")

    
    def dineProc():
        # Prompt to let user enter booking information 
        Order.Ddate = input("Please enter the Date of Booking for Dine in: ")
        Dtime = input("Please enter the Time of Booking for Dine in: ")
        Dperson = input("Please enter the number of Persons: ")
        print("Thank you for entering the details, Your Booking is confirmed")


class OnlineOrder(Order):
    def __init__(self):
        super().__init__()

    def process_order(self):
        enter_onvalue = input('Please enter 1 for Self pickup.\nPlease enter 2 for Home delivery.\nPlease enter 3 to go to Previous menu. ')
        #Check if input is valid and move to according function
        if enter_onvalue == '1':
           self.pickup_menu()
        elif enter_onvalue == '2':
           self.delivery_menu()
        else:
            app = RestaurantApp()
            app.order_food()
    
    #Function to display pick-up menu and handle user selection
    def pickup_menu(self):
        pi_amount = 0
        pi_damount = 0
    
        while True:
            print("Enter 1 for Noodles   Price AUD 2")
            print("Enter 2 for Sandwich  Price AUD 4")
            print("Enter 3 for Dumpling  Price AUD 6")
            print("Enter 4 for Muffins   Price AUD 8")
            print("Enter 5 for Pasta     Price AUD 10")
            print("Enter 6 for Pizza     Price AUD 20")
            print("Enter 7 for Drinks Menu:") 
    
            pick_order = int(input())
            #Calculate food price 
            if pick_order == 1:
                pi_amount += 2
            elif pick_order == 2:
                pi_amount += 4
            elif pick_order == 3:
                pi_amount += 6
            elif pick_order == 4:
                pi_amount += 8
            elif pick_order == 5:
                pi_amount += 10
            elif pick_order == 6:
                pi_amount += 20
            elif pick_order == 7:
                while True:
                    print("Enter 1 for Coffee     Price AUD 2")
                    print("Enter 2 for Colddrink  Price AUD 4")
                    print("Enter 3 for Shake      Price AUD 6")
                    print("Enter 4 for Checkout:")
                    pick_drinkorder = int(input())
                    #Calculate drink price
                    if pick_drinkorder == 1:
                       pi_damount += 2
                    elif pick_drinkorder == 2:
                       pi_damount += 4
                    elif pick_drinkorder == 3:
                       pi_damount += 6
                    elif pick_drinkorder == 4:
                        checkout = input("Please Enter Y to proceed to Checkout or Enter N to cancel the Order:")
                        #Uppercase input in case user enter lowercase type
                        valid_checkout = checkout.upper()
                        #Check if input is valid and run 
                        if valid_checkout == 'Y':
                            pi_amount += pi_damount
                            print('Your total payable amount is:', pi_amount, "AUD")
                            self.pickProc()   #3questions
                        
                            Order.pick_up.append("A")
                            Order.pick_up.append(Order.id)
                            Order.pick_up.append(Order.Pdate)
                            Order.pick_up.append("Pick Up")
                            Order.pick_up.append(pi_amount)
    
                            Order.all_order.append("A")
                            Order.all_order.append(Order.id)
                            Order.all_order.append(Order.Pdate)
                            Order.all_order.append("Pick Up")
                            Order.all_order.append(pi_amount)
                           
    
                            Order.total_amount=pi_amount+Order.total_amount
                            print("Your order has been confirmed, and your order id is ‘A"+str(Order.id).zfill(3),"\'")
                            Order.id=Order.id+1
                            break
                        else:
                            print("Your order has been cancelled")
                            break
                    else:
                        print("Invalid number")
                break
            else:
                print("Invalid number")
                break
                    
                    
    def pickProc(self):
        Order.Pdate=input("Please enter the Date of Pick Up: " )
        Ptime=input("Please enter the Time of Pick Up: " )
        Pperson=input("Please enter the name of Person: " )
        print("Thank you for entering the details, Your Booking is confirmed")
                   
    def delivery_menu(self):
        di_amount = 0
        di_damount = 0
        user = User()
        while True:
            print("Enter 1 for Noodles   Price AUD 2")
            print("Enter 2 for Sandwich  Price AUD 4")
            print("Enter 3 for Dumpling  Price AUD 6")
            print("Enter 4 for Muffins   Price AUD 8")
            print("Enter 5 for Pasta     Price AUD 10")
            print("Enter 6 for Pizza     Price AUD 20")
            print("Enter 7 for Drinks Menu:") 
    
            delivery_order = int(input())
            
            if delivery_order == 7:
                break
            else:
                if delivery_order == 1:
                    di_amount += 2
                elif delivery_order == 2:
                    di_amount += 4
                elif delivery_order == 3:
                    di_amount += 6
                elif delivery_order == 4:
                    di_amount += 8
                elif delivery_order == 5:
                    di_amount += 10
                elif delivery_order == 6:
                    di_amount += 20
            
        while True:
                print("Enter 1 for Coffee     Price AUD 2")
                print("Enter 2 for Colddrink  Price AUD 4")
                print("Enter 3 for Shake      Price AUD 6")
                print("Enter 4 for Checkout:")
                delivery_drinkorder = int(input())
    
                if delivery_drinkorder == 4:
                    
                    if self.checkout()=="Y":
                        di_amount+=di_damount
                        print("Your total payble amount is:",di_amount," and there will be an additional charges for Delivery")
                        self.delProc()
                        if Order.Delkm>0 and Order.Delkm<=4:
                            di_amount=di_amount+3
                        elif Order.Delkm>4 and Order.Delkm<=6:
                            di_amount=di_amount+6
                        elif Order.Delkm>5 and Order.Delkm<=12:
                            di_amount=di_amount+10
                        elif Order.Delkm>12:
                            break
                        if user.address_valid=="N":
                            ad=input("You have not mentioned your address, while signing up. \nPlease enter Y if would like to enter your address.\nEnter N if you would like to select other mod of order.")
                            ad1=ad.upper()
                            if ad1=="Y":
                                ad2=input("Please enter your address")
                                Order.delivery.append("A")
                                Order.delivery.append(Order.id)
                                Order.delivery.append(Order.Deldate)
                                Order.delivery.append("Home Delivery")
                                Order.delivery.append(di_amount)

                                Order.all_order.append("A")
                                Order.all_order.append(Order.id)
                                Order.all_order.append(Order.Deldate)
                                Order.all_order.append("Home Delivery")
                                Order.all_order.append(di_amount)
                                Order.total_amount=di_amount+Order.total_amount
                                print("Your order has been confirmed, and your order id is ‘A"+str(Order.id).zfill(3),"\'")
                                Order.id=Order.id+1
                            elif ad1=="N":
                                Order.sub_menu1()
                            pass
                        else:
                                
                                Order.delivery.append("A")
                                Order.delivery.append(Order.id)
                                Order.delivery.append(Order.Deldate)
                                Order.delivery.append("Home Delivery")
                                Order.delivery.append(di_amount)

                                Order.all_order.append("A")
                                Order.all_order.append(Order.id)
                                Order.all_order.append(Order.Deldate)
                                Order.all_order.append("Home Delivery")
                                Order.all_order.append(di_amount)
                                Order.total_amount=di_amount+Order.total_amount
                                print("Your order has been confirmed, and your order id is ‘A"+str(Order.id).zfill(3),"\'")
                                Order.id=Order.id+1
                                pass
                        break
                    
                else:
                    if delivery_drinkorder == 1:
                       di_damount += 2
                    elif delivery_drinkorder == 2:
                       di_damount += 4
                    elif delivery_drinkorder == 3:
                       di_damount += 6
                
    def checkout(self):
        print("Please Enter Y to proceed to Checkout or Enter N to cancel the Order:")
        check_opt = input().upper()
        if check_opt == "Y":
            print("")
        elif check_opt == "N":
            print("Order is canceled")
            app = RestaurantApp()
            app.sub_menu1()  # Go back to the submenu of the main menu
        return check_opt
                   
                        

                        
    def delProc(self):
        Order.Deldate=input("Please enter the Date of Delivery: " )
        Deltime=input("Please enter the Time of Delivery: " )
        Order.Delkm=int(input("Please enter the Distance from the Restaurant: " ))
        if Order.Delkm>12:
            print("Distance from the restaurant is more than the applicable limits, you must be provided with the option to Pick up the Order.")
            self.pickup_menu()
        print("Thank you for entering the details, Your Booking is confirmed")

class Statistics:
    @staticmethod
    def display_statistics():
        print("Please Enter the Option to Print the Statistics. \n1-All in Dine Orders \n2-All Pick Up Orders \n3-All Deliveries \n4-All Orders(Descedingly) \n5-Total Amount Spent on All Orders \n6-To Go to Previous Menu")
       
        sEnter=int(input())

        if sEnter==1:
            i=0
            j=0
            while i<(len(Order.dine_in))/5:
                print(Order.dine_in[j]+str(Order.dine_in[j+1]).zfill(3),"\t",Order.dine_in[j+2],"\t",Order.dine_in[j+3],"\t",Order.dine_in[j+4])
                j=j+5
                i=i+1

        elif sEnter==2:
            i=0
            j=0
            while i<(len(Order.pick_up))/5:
                print(Order.pick_up[j]+str(Order.pick_up[j+1]).zfill(3),"\t",Order.pick_up[j+2],"\t",Order.pick_up[j+3],"\t",Order.pick_up[j+4])
                j=j+5
                i=i+1
        elif sEnter==3:
            i=0
            j=0
            while i<(len(Order.delivery))/5:
                print(Order.delivery[j]+str(Order.delivery[j+1]).zfill(3),"\t",Order.delivery[j+2],"\t",Order.delivery[j+3],"\t",Order.delivery[j+4])
                j=j+5
                i=i+1
        elif sEnter==4:
            i=0
            j=0
            while i<(len(Order.all_order))/5:
                print(Order.all_order[j]+str(Order.all_order[j+1]).zfill(3),"\t",Order.all_order[j+2],"\t",Order.all_order[j+3],"\t",Order.all_order[j+4])
                j=j+5
                i=i+1
        elif sEnter==5:
            print(Order.total_amount)
        elif sEnter==6:
            RestaurantApp.sub_menu1()

class RestaurantApp:
    def __init__(self):
        self.customer = Customer()  # Initialize the customer attribute here
        self.user_info = None  # Initialize user_info here

    def main_menu(self):
        while True:
            option = int(input("Please Enter 1 for Signup\nPlease Enter 2 for Signin\nPlease Enter 3 for Quit\n"))
            if option == 1:
                self.user_info = list(self.customer.signup())  # Convert to list
            elif option == 2:
                if self.user_info is None:
                    print('Please firstly sign up')
                    self.user_info = list(self.customer.signup())  # Convert to list
                else:
                    self.customer.signin(self.user_info)
                    self.sub_menu1()
            elif option == 3:
                print('Sign out successfully.')
                break

    def sub_menu1(self):
        enter_value = input(
            "Please enter 2.1 to Start Ordering\nPlease enter 2.2 to Print Statistics\nPlease enter 2.3 to Log out.")
        if enter_value == '2.1':
            self.order_food()
        elif enter_value == '2.2':
            Statistics.display_statistics()
        else:
            print('Logged out successfully.')
            self.sub_menu1()

    def order_food(self):
        order_type = input('Please enter 1 for Dine in.\nPlease enter 2 for Order Online.\nPlease enter 3 to go to Login page. ')
        if order_type == '1':
            dine_in_order = DineInOrder()
            dine_in_order.process_order()
        elif order_type == '2':
            online_order = OnlineOrder()
            online_order.process_order()
        else: 
            self.main_menu()


if __name__ == '__main__':
    app = RestaurantApp()
    app.main_menu()
```
