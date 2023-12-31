# A231_SQIT3073 #ASS1
# WONG XIANG YI 286267

import sys 
import os 
os.system('cls')

# Function calculate monthly installment        
def cal_monthly_installment(principal, annual_interest, loan_term):
    monthly_interest_rate = (annual_interest/100)/12 #r
    monthly_loan_term = loan_term*12 #n
    numerator = monthly_interest_rate*((1+monthly_interest_rate)**monthly_loan_term)
    denominator = ((1+monthly_interest_rate)**monthly_loan_term)-1
    monthly_installment = principal*(numerator/denominator) #MI = P((r(1+r)^n)/((1+r)^n)-1)
    return monthly_installment 

# Function calculate Total payable amount
def cal_total_payable(monthly_installment, loan_term):
    total_payable = (monthly_installment*(loan_term*12))
    return total_payable

# Function DSR
def cal_DSR(monthly_installment, monthly_fin_com, monthly_income):
    DSR = ((monthly_installment+monthly_fin_com)/monthly_income)*100
    return DSR

# List to store items
cal = []

# Function Show loan results
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
    print(f"                    DSR: {DSR:.2f}%")

    if DSR<= 70:
        print("\nYou are eligible for the loan")
    else:
        print("\nYou are not eligible for the loan due to DSR smaller than 70%")
    print("-------------------------------------------------------------------")
# Append into list so that we can show in history later
    cal.append(
        {"Principal Loan Amount: RM": principal,
         "Annual Interest Rate (%): ": annual_interest,
         "Loan Term (years): ": loan_term,
         "Monthly Income: RM": monthly_income,
         "Other Monthly Financial Commitment: RM": monthly_fin_com,
         "Monthly Installment: RM": round(monthly_installment,2),
         "Total Payable Amount: RM": round(total_payable,2),
         "DSR (%): ": round(DSR,2),
         "Eligibility": DSR <= 70
         }
    )

# Function Show History
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

# Function Del previous calculation in list (Need to change to tuple since list is immutable)
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

# Function Menu
def menu():
    loop = 0
    loop1 = 1
    while loop < loop1:  #Give a true condition to loop infinity
        print("\nMENU:")
        print("1. Calculate Loan")
        print("2. History")
        print("3. Delete previous calculation")
        print("4. Exit")

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
    loop += 1

# Open Menu
menu()
