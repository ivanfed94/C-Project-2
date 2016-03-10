# C-Project-2
//This program Requests the user inputs how much Hours they have worked and their pay rate. It then calculates their Total and then //take home pay after deductions of taxes and health insurance (if applicable).

#include <iostream>
#include <cmath>
#include <string>
using namespace std;


enum MaritalStatus{Single, Married};

//Function name: GetWorkingHours()
//The function prompts users to enter the working hours. 
//The function checks and returns valid input (10 <= hours <= 80)
int GetWorkingHours(int Hours_worked_per);

//Function name: GetWage()
//The function prompts users to enter wage.
//The function checks and returns valid input (10.00 < wage)
double GetWage(double Wage);

//Function name: GetMaritalStatus()
//The function prompts users to enter marital status.
//The function checks valid input: S, M, s or m and returns SINGLE or MARRIED
MaritalStatus GetMaritalStatus();

//Function name: CalculatePayment()
//The function gets working hours and wage as input parameters.
//The function calculates and return the total payment.
double CalculatePayment(int hours, double wage);

//Function name: CalculateTaxes()
//The function gets total payment as input parameter.
//The function calculates federal tax, state tax and social security tax
//Those tax values are output parameters
void CalculateTaxes(double payment, double& fedTax, double& stateTax, double& ssTax);

//Function name: CalculateInsurance()
//The function gets marital status as input parameter.
//The function calculates and returns deduction for insurance
double CalculateInsurance(MaritalStatus mStatus);

//Function name: PrintPayStub()
//The function gets working hours, pay rate and marital status as input parameters.
//The function calculates total payments, taxes, insurance.
//Finally, the function prints out the paystub
void PrintPayStub(int wHours, double payRate, MaritalStatus mStatus);

int Hours_checked_Pro(int Hours_checked_pur);//checks if the user entered a valid input for HoursWorked. Returning the correct value.

double Check_Wage(double Test);//Checks if the user entered a valid input. Looping until the user enters a correct value.
int Restart;
void main()
{
	do
	{
		cout.setf(ios::fixed);
		cout.setf(ios::showpoint);
		cout.precision(2);

		int Hours_worked;//variable to get the amount of hours worked
							//dummy number to call the function GetWorkingHours

		Hours_worked = GetWorkingHours(Hours_worked = 0);// calls the function GetWorkingHours and returns Hours_checked
															// making Hours_worked = Hours_checked

		double Pay_rate;//Variable for the amount per hour the user works
		Pay_rate = GetWage(Pay_rate = 0.0);//Calls the function GetWage and returns Wage_ch

		MaritalStatus mStatus;
		mStatus = GetMaritalStatus();//returns a enumeration value of single or married.
		if (mStatus == Single)
		{
			cout << "Single" << endl;
		}
		else if (mStatus == Married)
		{
			cout << "Married" << endl;
		}

		cout << "Your Payment Statement:" << endl << endl;

		cout << "Total Hours: " << Hours_worked << endl;
		if (Hours_worked > 40)
		{
			cout << "Overtime: " << Hours_worked - 40 << endl;
		}
		else
			cout << "Overtime: " << "0" << endl;

		cout << "Hourly Rate: $" << Pay_rate << endl << endl;


		double Total;
		Total = CalculatePayment(Hours_worked, Pay_rate);

		cout << "TOTAL EARNED: $" << Total << endl << endl;
		double fedtax, stateTax, ssTax;

		CalculateTaxes(Total, fedtax, stateTax, ssTax);

		double M_cal;

		M_cal = CalculateInsurance(mStatus);

		cout << M_cal << endl;


		PrintPayStub(Hours_worked, Pay_rate, mStatus);

		do
		{
			cout << "Do you want to continue: Please enter an 'N' or 'n' for NO & 'Y' or 'y' for YES: ";
			char Continue;
			cin >> Continue;
			if (Continue == 'n' || Continue == 'N')
				Restart = 1;
			else if (Continue == 'y' || Continue == 'Y')
			{
				Restart = 0;
			}
			else
				Restart = 3;
		} while (Restart > 1 || Restart < 0);




	} while (Restart == 0);

}

int GetWorkingHours(int Hours_worked_per)
{
	cout << "Please enter the amount of Hours worked this week: Restricted to 10-80 HRS: ";
	cin >> Hours_worked_per;
	int Hours_checked;
	Hours_checked = Hours_checked_Pro(Hours_worked_per);

	return Hours_checked;
}
int Hours_checked_Pro(int Hours_chekced_pur)//checks if the user entered a valid input. Returning the correct value.
{
	
	if (Hours_chekced_pur <= 80 && Hours_chekced_pur >= 10)
	{
		return Hours_chekced_pur;
	}
	else
	{
		while (Hours_chekced_pur < 10 || Hours_chekced_pur > 80)
		{
			cout << "Wrong number please enter a number between 10 - 80: ";
			cin >> Hours_chekced_pur;
		} 

		return Hours_chekced_pur;
	}
}

double GetWage(double Wages)
{
	cout << "Please enter the rate which you get paid: ";
	cin >> Wages;
	double Wage_ch;
	Wage_ch = Check_Wage(Wages);
	return Wage_ch;
}

double Check_Wage(double test)
{
	if (test >= 10.0)
	{
		return test;
	}
	else
	{
		while (test < 10.0)
		{
			cout << "invalid input. Please enter a pay rate greater than 10.0 hrs: ";
			cin >> test;
		}
		return test;
	}

}

MaritalStatus GetMaritalStatus()
{
	cout << "Please enter your marriage status:'S' or 's' = Single || 'M' or 'm' = Married: ";
	char M_status;

	cin >> M_status;
	if (M_status != 'm' && M_status != 's')
	{
		while (M_status != 'm' && M_status != 's')
		{
			cout << "Invalid Input please only enter a single letter:s or m: ";
			cin >> M_status;
			if (M_status == 'S' || M_status == 's')
			{
				return Single;
			}
			else if (M_status == 'M' || M_status == 'm')
				return Married;
		}

	}
	else if (M_status == 'S' || M_status == 's')
	{
		return Single;
	}
	else if (M_status == 'M' || M_status == 'm')
		return Married;
	
}
double CalculatePayment(int hours, double wage)
{
	double Pay = 0;
	double Overtime, overtime_rate = 1.5;
	Overtime = hours - 40;
	if (hours <= 40)
	{
		Pay = hours * wage;
	}
	else
	{
		Pay = ((hours - Overtime) * wage) + (Overtime * (overtime_rate * wage));
	}
	
	return Pay;
}
void CalculateTaxes(double payment, double& fedTax, double& stateTax, double& ssTax)
{
	
	const double ss_rate = .06;


	cout << "Social Security Tax: $";
	ssTax = ss_rate * payment;
	cout << ssTax << endl;
	if (payment > 1000.00)
	{
		const double federal_income_rate_rich = .1, state_income_rate_rich = .04;

		fedTax = federal_income_rate_rich * payment;
		stateTax = state_income_rate_rich * payment;
	}
	else
	{
		const double federal_income_poor = .07, state_income_poor = .03;

		fedTax = federal_income_poor * payment;
		stateTax = state_income_poor * payment;
	}
	cout << "Federal Income Tax: $";
	cout << fedTax << endl;
	cout << "State Income Tax: $";
	cout << stateTax << endl;
}

double CalculateInsurance(MaritalStatus mStatus)
{
	double H_I;
	cout << "Health Insurance: $";
	if (mStatus == Married)
	{
		const double Health_I = 50.0;
		H_I = Health_I;
	}

	else
	{
		const double Health_s = 0.0;
		H_I = Health_s;
	}
	
	
	
	return H_I;


}
void PrintPayStub(int wHours, double payRate, MaritalStatus mStatus)
{
	cout << "------------------------------------" << endl;
	cout << "TAKE HOME PAY: $";
	double Take_home;
	double Total_pay;
	int over;
	const double srate = .06;
	if (wHours >= 40)
	{
		over = wHours - 40;
		Total_pay = (payRate * 40) + ((over)*(1.5 * payRate));
		if (Total_pay > 1000.00)
		{
			const double fedtax = 0.10, statetax = 0.04;
			double fedTax, stateTax, ss_rate;

			if (mStatus == Single)
			{
				double H_2;
				H_2 = 0;
				Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
				cout << Take_home << endl;

			}
			else if (mStatus == Married)
			{
				double H_2;
				H_2 = 50;
				Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
				cout << Take_home << endl;

			}
			else;

		}
		else if (Total_pay < 1000.0)
		{
			const double fedtax = 0.07, statetax = 0.03;
			double fedTax, stateTax, ss_rate;

			if (mStatus == Single)
			{
				double H_2;
				H_2 = 0;
				Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
				cout << Take_home << endl;

			}
			else if (mStatus == Married)
			{
				double H_2;
				H_2 = 50;
				Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);

				cout << Take_home << endl;

			}
		}
		
	}
	else if (wHours <= 40)
	{

		Total_pay = payRate * wHours;
		cout << endl << Total_pay << endl;
	if (Total_pay > 1000.00)
	{
		const double fedtax = 0.10, statetax = 0.04;
		double fedTax, stateTax,ss_rate;
		
		if (mStatus == Single)
		{
			double H_2;
			H_2 = 0;
			Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
			cout << Take_home << endl;
			
		}
		else if (mStatus == Married)
		{
			double H_2;
			H_2 = 50;
			Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
			cout << Take_home << endl;
		
		}
		else;
		
	}
	else if (Total_pay < 1000.0)
	{
		const double fedtax = 0.07, statetax = 0.03;
		double fedTax, stateTax, ss_rate;
		
		if (mStatus == Single)
		{
			double H_2;
			H_2 = 0;
			Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);
			cout << Take_home << endl;

		}
		else if (mStatus == Married)
		{
			double H_2;
			H_2 = 50;
			Take_home = Total_pay - ((fedtax * Total_pay) + (statetax * Total_pay) + (srate * Total_pay) + H_2);

			cout << Take_home << endl;

		}
	}
	}
	
	
}
	
