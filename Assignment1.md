# A231_SQIT3073 #ASS1
# WONG XIANG YI 286267

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

# Show Loan Result
def all_info(principal, annual_interest, loan_term, monthly_fin_com, monthly_income):
    monthly_installment = cal_monthly_installment(principal, annual_interest, loan_term)
    total_payable = cal_total_payable(monthly_installment, loan_term)
    DSR = cal_DSR(monthly_installment, monthly_fin_com, monthly_income)

    print(f"\nLoan Result: Princpal Loan Amount: RM {principal}")
    print(f"             Annual Interest Rate: {annual_interest}%")
    print(f"             Loan Term: {loan_term} years")
    print(f"             Monthly Income: RM {monthly_income}")
    print(f"             Other Monthly Commitments: RM {monthly_fin_com}")
    # output in 2dp
    print(f"             Monthly Instalment: RM {monthly_installment:.2f}")
    print(f"             Total Payment: RM {total_payable:.2f}")
    print(f"             DSR: {DSR:.2f}%")

    if DSR<= 70:
        print("\nYou are eligible for the loan")
        print("-------------------------------------------------------------------")
    else:
        print("\nYou are not eligible for the loan due to DSR smaller than 70%")
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
         "DSR (%): ": round(DSR,2),
         "Eligibility": DSR <= 70
         }
    )

# Show History
def all_cal():
    print("\nAll History:")
    number = 1
    for cal1 in cal:
        print(f"\nHistory {number}:")
        for key, value in cal1.items():
            print(f"{key}: {value}")
        number += 1
    print("-------------------------------------------------------------------")

# Menu
def menu():
    loop = 0
    loop1 = 1
    while loop < loop1: 
        print("\nMENU:")
        print("1. Calculate Loan")
        print("2. History")
        print("3. Exit")

        select = input("Enter (1/2/3): ")

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
            exit = str(input("\nAll history will be clear after exit the program. \nYes, exit. = 1 \nNo = 0 \nEnter (0/1):  "))
            if "1" in  exit:
                print("Bye.")
                sys.exit()
            elif "0" in exit: 
                menu()
            else:
                print("Invalid input. Please enter again.")
                menu()
        else:
            print("Invalid. Please enter 1, 2 or 3.")
    loop += 1

menu()
