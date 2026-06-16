#include <iostream>
#include <fstream>
#include <string>
using namespace std;

class BankAccount
{
private:
    int accountNumber;
    string name;
    double balance;

public:
    BankAccount()
    {
        accountNumber = 0;
        name = "";
        balance = 0;
    }

    void createAccount()
    {
        cout << "\nEnter Account Number: ";
        cin >> accountNumber;

        cin.ignore();
        cout << "Enter Customer Name: ";
        getline(cin, name);

        cout << "Enter Initial Deposit: ";
        cin >> balance;

        saveToFile();
        cout << "\nAccount Created Successfully!\n";
    }

    void saveToFile()
    {
        ofstream file("accounts.txt", ios::app);

        file << accountNumber << "|"
             << name << "|"
             << balance << endl;

        file.close();
    }

    void displayAccount()
    {
        cout << "\nAccount Number : " << accountNumber;
        cout << "\nCustomer Name  : " << name;
        cout << "\nBalance        : $" << balance << endl;
    }

    static void checkBalance(int accNo)
    {
        ifstream file("accounts.txt");
        string line;

        while (getline(file, line))
        {
            size_t pos1 = line.find("|");
            size_t pos2 = line.rfind("|");

            int acc = stoi(line.substr(0, pos1));
            string name = line.substr(pos1 + 1, pos2 - pos1 - 1);
            double balance = stod(line.substr(pos2 + 1));

            if (acc == accNo)
            {
                cout << "\nAccount Found";
                cout << "\nName: " << name;
                cout << "\nBalance: $" << balance << endl;
                file.close();
                return;
            }
        }

        cout << "\nAccount Not Found!\n";
        file.close();
    }

    static void deposit(int accNo, double amount)
    {
        ifstream file("accounts.txt");
        ofstream temp("temp.txt");

        string line;
        bool found = false;

        while (getline(file, line))
        {
            size_t pos1 = line.find("|");
            size_t pos2 = line.rfind("|");

            int acc = stoi(line.substr(0, pos1));
            string name = line.substr(pos1 + 1, pos2 - pos1 - 1);
            double balance = stod(line.substr(pos2 + 1));

            if (acc == accNo)
            {
                balance += amount;
                found = true;
            }

            temp << acc << "|" << name << "|" << balance << endl;
        }

        file.close();
        temp.close();

        remove("accounts.txt");
        rename("temp.txt", "accounts.txt");

        if (found)
            cout << "\nDeposit Successful!\n";
        else
            cout << "\nAccount Not Found!\n";
    }

    static void withdraw(int accNo, double amount)
    {
        ifstream file("accounts.txt");
        ofstream temp("temp.txt");

        string line;
        bool found = false;

        while (getline(file, line))
        {
            size_t pos1 = line.find("|");
            size_t pos2 = line.rfind("|");

            int acc = stoi(line.substr(0, pos1));
            string name = line.substr(pos1 + 1, pos2 - pos1 - 1);
            double balance = stod(line.substr(pos2 + 1));

            if (acc == accNo)
            {
                found = true;

                if (balance >= amount)
                {
                    balance -= amount;
                    cout << "\nWithdrawal Successful!\n";
                }
                else
                {
                    cout << "\nInsufficient Balance!\n";
                }
            }

            temp << acc << "|" << name << "|" << balance << endl;
        }

        file.close();
        temp.close();

        remove("accounts.txt");
        rename("temp.txt", "accounts.txt");

        if (!found)
            cout << "\nAccount Not Found!\n";
    }
};

int main()
{
    int choice, accNo;
    double amount;

    while (true)
    {
        cout << "\n================================";
        cout << "\n BANK MANAGEMENT SYSTEM";
        cout << "\n================================";
        cout << "\n1. Create Account";
        cout << "\n2. Deposit";
        cout << "\n3. Withdraw";
        cout << "\n4. Check Balance";
        cout << "\n5. Exit";
        cout << "\nEnter Choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
        {
            BankAccount acc;
            acc.createAccount();
            break;
        }

        case 2:
            cout << "\nEnter Account Number: ";
            cin >> accNo;
            cout << "Enter Deposit Amount: ";
            cin >> amount;
            BankAccount::deposit(accNo, amount);
            break;

        case 3:
            cout << "\nEnter Account Number: ";
            cin >> accNo;
            cout << "Enter Withdrawal Amount: ";
            cin >> amount;
            BankAccount::withdraw(accNo, amount);
            break;

        case 4:
            cout << "\nEnter Account Number: ";
            cin >> accNo;
            BankAccount::checkBalance(accNo);
            break;

        case 5:
            cout << "\nThank You!\n";
            return 0;

        default:
            cout << "\nInvalid Choice!\n";
        }
    }

    return 0;
}