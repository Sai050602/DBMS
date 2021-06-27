#include<iostream>
#include<fstream>
#include<iomanip>
#include<stdlib.h>

using namespace std;

class Student
{
	int admno;
	string fn,ln,course,sec;

	public:
		void getData();
		void showData();
		int getAdmno(){return admno;}
}s;

void Student::getData()
{
	cout<<"\n\nEnter Student Details:\n";
	cout<<"Enter Admission No.: "; cin>>admno;
	cout<<"Enter First Name   : "; cin>>fn;
	cout<<"Enter Last Name    : "; cin>>ln;
	cout<<"Enter Course Name  : "; cin>>course;
	cout<<"Enter Section      : "; cin>>sec;
}

void Student::showData()
{
	cout<<"\n\nStudent Details...\n";
	cout<<"Admission No. : "<<admno<<endl;
	cout<<"First Name    : "<<fn<<endl;
	cout<<"Last Name     : "<<ln<<endl;
	cout<<"Course Name   : "<<course<<endl;
	cout<<"Section       : "<<sec<<endl;
	cout<<endl;
}

void addData()
{
	ofstream fout;
	fout.open("Students.dat");
	s.getData();
	fout.write((char*)&s,sizeof(s));
	fout.close();
	cout<<"\n\nData Successfully Saved to File....\n";
}

void displayData()
{
	ifstream fin;
	fin.open("Students.dat",ios::in|ios::binary);
	while(fin.read((char*)&s,sizeof(s)))
	{
		s.showData();
	}
	fin.close();
	cout<<"\n\nData Reading from File Successfully Done....\n";
}

void searchData()
{
	int n, flag=0;
	ifstream fin;
	fin.open("Students.dat");
	cout<<"Enter Admission Number you want to search : ";
	cin>>n;

	while(fin.read((char*)&s,sizeof(s)))
	{
		if(n==s.getAdmno())
		{
			cout<<"The Details of Admission No. "<<n<<" shown herewith:\n";
			s.showData();
			flag++;
		}
	}
	fin.close();
	if(flag==0)
		cout<<"The Admission No. "<<n<<" not found....\n\n";
	cout<<"\n\nData Reading from File Successfully Done....\n";
}

void deleteData()
{
	int n, flag=0;
	ifstream fin;
	ofstream fout,tout;

	fin.open("Students.dat");
	fout.open("TempStud.dat");
	tout.open("TrashStud.dat");

	cout<<"Enter Admission Number you want to move to Trash : ";
	cin>>n;

	while(fin.read((char*)&s,sizeof(s)))
	{
		if(n==s.getAdmno())
		{
			cout<<"The Following Admission No. "<<n<<" has been moved to Trash:\n";
			s.showData();
			tout.write((char*)&s,sizeof(s));
			flag++;
		}
		else
		{
			fout.write((char*)&s,sizeof(s));
		}
	}
	fout.close();
	tout.close();
	fin.close();
	if(flag==0)
		cout<<"The Admission No. "<<n<<" not found....\n\n";
	remove("Students.dat");
	rename("tempStud.dat","Students.dat");
}

void getTrash()
{
	ifstream fin;
	fin.open("TrashStud.dat");
	while(fin.read((char*)&s,sizeof(s)))
	{
		s.showData();
	}
	fin.close();
	cout<<"\n\nData Reading from Trash File Successfully Done....\n";
}

void modifyData()
{
	int n, flag=0, pos;
	fstream fio;

	fio.open("Students.dat");

	cout<<"Enter Admission Number you want to Modify : ";
	cin>>n;

	while(fio.read((char*)&s,sizeof(s)))
	{
		pos=fio.tellg();
		if(n==s.getAdmno())
		{
			cout<<"The Following Admission No. "<<n<<" will be modified with new data:\n";
			s.showData();
			cout<<"\n\nNow Enter the New Details....\n";
			s.getData();
			fio.seekg(pos-sizeof(s));
			fio.write((char*)&s,sizeof(s));
			flag++;
		}
	}
	fio.close();

	if(flag==0)
		cout<<"The Admission No. "<<n<<" not found....\n\n";
}

void project()
{
	int ch;
	do
	{
		system("cls");
		cout<<"...............STUDENT DATABASE MANAGEMENT SYSTEM..............\n";
		cout<<"======================================================\n";
		cout<<"0. Exit from Program\n";
		cout<<"1. Write Data to File\n";
		cout<<"2. Read Data From File\n";
		cout<<"3. Search Data From File\n";
		cout<<"4. Delete Data From File\n";
		cout<<"5. Get Deleted Records from Trash file\n";
		cout<<"6. Modify Data in File\n";
		cout<<"Enter your choice  : ";
		cin>>ch;
		system("cls");
		switch(ch)
		{
			case 1: addData(); break;
			case 2: displayData(); break;
			case 3: searchData(); break;
			case 4: deleteData(); break;
			case 5: getTrash(); break;
			case 6: modifyData(); break;
		}
		system("pause");
	}while(ch);
}

int main()
{
	project();
}
