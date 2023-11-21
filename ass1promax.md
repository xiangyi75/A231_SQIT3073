# A231_SQIT3073
# Wong Xiang Yi 286267

# this is promax version hahahah 

import sys
import os
os.system('cls')

# All function           
def cal_monthly_installment(principal, annual_interest, loan_term):
    monthly_interest_rate = (annual_interest/100)/12 #r
    monthly_loan_term = loan_term*12 #n
    numerator = monthly_interest_rate*((1+monthly_interest_rate)**monthly_loan_term)
    denominator = ((1+monthly_interest_rate)**monthly_loan_term)-1
    monthly_installment = principal*(numerator/denominator) #MI = P((r(1+r)^n)/((1+r)^n)-1)
    return monthly_installment 

def cal_total_payable(monthly_installment, loan_term):
    total_payable = (monthly_installment*(loan_term*12))
    return total_payable

def cal_DSR(monthly_installment, monthly_fin_com, monthly_income):
    DSR = ((monthly_installment+monthly_fin_com)/monthly_income)*100
    return DSR

cal = []
DSR_threshold = 70
# Show loan results
def all_info(principal, annual_interest, loan_term, monthly_fin_com, monthly_income):
    monthly_installment = cal_monthly_installment(principal, annual_interest, loan_term)
    total_payable = cal_total_payable(monthly_installment, loan_term)
    DSR = cal_DSR(monthly_installment, monthly_fin_com, monthly_income)

    print(f"\nLoan Detail: Princpal Loan Amount: RM {principal}")
    print(f"             Annual Interest Rate: {annual_interest}%")
    print(f"             Loan Term: {loan_term} years")
    print(f"             Monthly Income: RM {monthly_income}")
    print(f"             Other Monthly Commitments: RM {monthly_fin_com}")
    # output in 2dp
    print(f"\nCalculation Result: Monthly Instalment: RM {monthly_installment:.2f}")
    print(f"                    Total Payment: RM {total_payable:.2f}")
    print(f"                    DSR ({DSR_threshold}%): {DSR:.2f}%")

    if DSR <= DSR_threshold:
        print("\nYou are eligible for the loan")
    else:
        print(f"\nYou are not eligible for the loan due to DSR greater than {DSR_threshold}")
    print("-------------------------------------------------------------------")
    
    # append into list so that we can show in history later
    cal.append(
        {"Principal Loan Amount: RM": principal,
         "Annual Interest Rate (%): ": annual_interest,
         "Loan Term (years): ": loan_term,
         "Monthly Income: RM": monthly_income,
         "Other Monthly Financial Commitment: RM": monthly_fin_com,
         "Monthly Installment: RM": round(monthly_installment,2),
         "Total Payable Amount: RM": round(total_payable,2),
         "DSR: ": round(DSR,2),
         "Eligibility": DSR <= DSR_threshold
         }
    )

# Modify threshold
def change_threshold():
    global DSR_threshold
    try:
        new_threshold = float(input("\nEnter the new DSR threshold: "))
        DSR_threshold = new_threshold
        print(f"DSR threshold set to {DSR_threshold}%")
    except ValueError:
        print("Invalid input.")
      

# Show History
def all_cal():
    if not cal:
        print("\nNo history.")
    else:
        print("\nAll History:")
        number = 1
        for cal1 in cal:
            print(f"\nHistory {number}:")
            for key, value in cal1.items():
                print(f"{key}: {value}")
            number += 1
    print("-------------------------------------------------------------------")

# Del previous calculation in list
def del_cal():
    global cal
    if not cal:
        print("\nNo history to delete.")
    else:
        dlt = input("Sure to delete the previous calculation? (1 = yes, 0 = no): ")
        if dlt == "1":
            cal_tuple = tuple(cal)
            if len(cal_tuple) > 0:
                cal_tuple = cal_tuple[:-1]
                cal = list(cal_tuple)
                print("\nDeleted the last history.")
            else:
                menu()
        elif dlt == "0":
            menu()
        else:
            print("Invalid input.")
            menu()
    print("-------------------------------------------------------------------")


#System
def menu():
        print("\nMENU:")
        print("1. Calculate Loan")
        print("2. History")
        print("3. Delete previous calculation")
        print("4. Modify threshold")
        print("5. Exit")

        select = input("Enter (1/2/3/4): ")

        if select == "1":
            try:
                principal = float(input("\nEnter the principal loan amount: "))
                annual_interest = float(input("Enter the annual interest rate: "))
                loan_term = int(input("Enter the loan term in years: "))
                monthly_income = float(input("Enter the applicant's monthly income: "))
                monthly_fin_com = float(input("Enter any other monthly financial commitments: "))
                    
                all_info(principal, annual_interest, loan_term, monthly_fin_com, monthly_income)
            except:
                 print("Invalid input. Please enter again.")
                 print("-------------------------------------------------------------------")
        elif select == "2":
            all_cal()
        elif select == "3":
            del_cal()
        elif select == "4":
            change_threshold()
        elif select == "5":
            exit = input("\nSure to exit? All history will be delete. (1 = yes, 0 = no): ")
            if exit == "1":
                print("Bye.")
                sys.exit()
            elif exit == "0": 
                menu()
            else:
                print("Invalid input.")
                menu()
        else:
            print("Invalid. Please enter 1, 2, 3 or 4.")
    

# Open Menu
print("Welcome to the DSR calculator! Are you wondering your eligibility to the loan, no worry we will give accurate calculation to you.")
welcome = (input("Enter 1 to open menu: ")) 
while welcome == "1":
    menu()
    
    a = 0
    b = 1
    while a < b:
        a += 1

else:
    print("Invalid input.")
