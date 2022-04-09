# Project-4: This program calculates how many people can ride an elevator with a weight limit of 1,100 lbs. The program lists the names and weight of the people who want to ride and determines who will fit. It then arranges the people in descending sequence by weight, and then ascending sequence by name. Finally, the program determines which method allows for the most people to ride the elevator.

# ![image](https://user-images.githubusercontent.com/56613527/162595456-f6049e2e-0d67-4cb7-acd2-64adfaaa8a62.png)

#include <iostream>
#include <fstream>
#include <iomanip>
#include <string>

using namespace std;

int howMany(int weights[], int n);

void printArrays(ofstream& outfile, string names[], int weights[], int count);
void selectionSortByWeights(string names[], int weights[], int n);
void bubbleSortByNames(string names[], int weights[], int n);

int main()
{
	ifstream infile;
	infile.open("input.txt");

	if (!infile)
	{
		cout << "Input file cannot be opened!" << endl;
		return 1;
	}

	ofstream outfile;
	outfile.open("output.txt");

	string names[12];
	int weights[12];

	int n = 0;
	string str;
	int wt;

	infile >> str;
	while (infile && n < 12)
	{
		infile >> wt;

		names[n] = str;
		weights[n] = wt;
		n++;

		infile >> str;
	}

	outfile << "Initial name and weight of people who want to ride the elevator:" << endl;
	printArrays(outfile, names, weights, n);

	int count1 = howMany(weights, n);

	outfile << "\nName and weight of people who got on the elevator:" << endl;
	printArrays(outfile, names, weights, count1);
	outfile << "Number of people who got on the elevator: " << count1 << endl;

	selectionSortByWeights(names, weights, n);

	outfile << "\n\nAfter rearranging the people in descending sequence by weight:" << endl;
	printArrays(outfile, names, weights, n);

	int count2 = howMany(weights, n);

	outfile << "\nName and weight of people who got on the elevator:" << endl;
	printArrays(outfile, names, weights, count2);
	outfile << "Number of people who got on the elevator: " << count2 << endl;

	bubbleSortByNames(names, weights, n);

	outfile << "\n\nAfter rearranging the people in ascending sequence by name:" << endl;
	printArrays(outfile, names, weights, n);

	int count3 = howMany(weights, n);

	outfile << "\nName and weight of people who got on the elevator:" << endl;
	printArrays(outfile, names, weights, count3);
	outfile << "Number of people who got on the elevator: " << count3 << endl << endl << endl;

	if (count1 >= count2 && count1 >= count3)
		outfile << "Initial (before sorting) method allowed the most people to ride the elevator." << endl;
	else if (count2 >= count1 && count2 >= count3)
		outfile << "Descending sequence by weight method allowed the most people to ride the elevator." << endl;
	else
		outfile << "Ascending sequence by name method allowed the most people to ride the elevator." << endl;
	outfile << endl;

	return 0;
};

int howMany(int weights[], int n)
{
	int loadLimit = 1100;
	int count = 0;
	int total = 0;

	while (total < loadLimit)
	{
		total += weights[count];
		count++;
	}

	if (total > loadLimit)
	{
		count--;
		total -= weights[count];
	}

	return count;
}

void printArrays(ofstream& outfile, string names[], int weights[], int count)
{
	int totalWeight = 0;

	outfile << "-------------------" << endl;
	outfile << left << setw(10) << "NAME" << right << setw(8) << "WEIGHT" << endl;
	outfile << "-------------------" << endl;
	for (int i = 0; i < count; i++)
	{
		outfile << left << setw(10) << names[i] << right << setw(8) << fixed << weights[i] << endl;
		totalWeight += weights[i];
	}

	outfile << "\nTotal weight of people: " << totalWeight << endl;
}

void selectionSortByWeights(string names[], int weights[], int n)
{
	for (int i = 0; i < n - 1; i++)
	{
		int minPos = i;

		for (int j = i + 1; j < n; j++)
		{
			if (weights[j] > weights[minPos])
			{
				minPos = j;
			}
		}

		if (minPos != i)
		{
			int tempWeight = weights[minPos];
			weights[minPos] = weights[i];
			weights[i] = tempWeight;


			string tempName = names[minPos];
			names[minPos] = names[i];
			names[i] = tempName;
		}
	}
}

void bubbleSortByNames(string names[], int weights[], int n)
{
	for (int i = 0; i < n - 1; i++)
	{
		for (int j = 0; j < n - i - 1; j++)
		{
			if (names[j].compare(names[j + 1]) > 0)
			{
				string tempName = names[j];
				names[j] = names[j + 1];
				names[j + 1] = tempName;

				int tempWeight = weights[j];
				weights[j] = weights[j + 1];
				weights[j + 1] = tempWeight;
			}
		}
	}
}
