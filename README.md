# Registration-and-login-using-file-handling
import re
import os.path
import pandas as pd


def registration():
    Username = input("Create username : ")
    Password = input("Create password : ")
    val1= True
    val2= True
    # Password Validation
    SpecialSym = ['$', '@', '#', '%']
    if len(Password) < 5:
        print('length should be at least 5')
        val2 = False

    if len(Password) > 16:
        print('length should be not be greater than 16')
        val2 = False

    if not any(char.isdigit() for char in Password):
        print('Password should have at least one numeral')
        val2 = False

    if not any(char.isupper() for char in Password):
        print('Password should have at least one uppercase letter')
        val2 = False

    if not any(char.islower() for char in Password):
        print('Password should have at least one lowercase letter')
        val2 = False

    if not any(char in SpecialSym for char in Password):
        print("Password should have special character")
        val2 = False

    #Username Validation
    pattern = "^[a-zA-Z][a-zA-Z0-9-_]+@[a-z]+.[a-z]{1,3}$"
    if re.match(pattern, Username):
        val1 = True
    else:
        val1 = False
        print("Username pattern not matched")
    #file handling
    if val1 and val2:
        if (os.path.exists('Database.csv')):
            write(Username, Password)
        else:
            fileCreation()
            write(Username, Password)
        print("Registration Successfull")
    else:
        print("Invalid Username and Password")
    return

def write(User,Pass):
    Credential = {
        'Username': [User],
        'Password': [Pass]
    }
    df = pd.DataFrame(Credential)
    df.to_csv('Database.csv', mode='a', index=False, header=False)
    return

#File Creation
def fileCreation():
    Credential = {
        'Username': [],
        'Password': []
    }
    df = pd.DataFrame(Credential)
    df.to_csv('Database.csv', index=False, mode='w')
    return
#Login Validation
def Login():
    Username = input("Enter username : ")
    Password = input("Enter password : ")
    if (os.path.exists('Database.csv')):
        read(Username, Password)
    else:
        fileCreation()
        read(Username, Password)
    return

def read(User,Pass):
    df = pd.read_csv('Database.csv')
    Users_list = df['Username'].tolist()
    pass_list = df['Password'].tolist()
    # show the list
    if User in Users_list and Pass in pass_list:
        print("Login Successfull")
    else:
        print("Invalid Credentials")
    return

def forgotpassword():
    Username = input("Enter username : ")
    if (os.path.exists('Database.csv')):
        getpassword(Username)

    else:
        fileCreation()
        getpassword(Username)
    return

def getpassword(Username):
    df = pd.read_csv('Database.csv', index_col=None)
    Users_list = df['Username'].tolist()
    if Username in Users_list:
        Password = df.loc[df['Username'] == Username, 'Password']
        Pass = str(Password.tolist())
        print("Password is",Pass)
    else:
        print("Invalid Username please register")
    return


#Mainstarting
print("Choose option")
print("1. Register")
print("2. Login")
print("3. Forgot Password")
print("Enter number: ")
option=int(input())
if option == 1:
    registration()
elif option == 2:
    Login()
else:
    forgotpassword()
