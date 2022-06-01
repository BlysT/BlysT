#include <iostream>
#include <random>
#include <time.h>
#include <conio.h>
#include <string>
#include <sstream>
#include <fstream>

#define REDBACKGROUND "\033[1;41m"
#define RESET "\033[m"
#define TAB "\t"

using text = std::string;

const int CONST_GL_SIZE_ARRAY = 9;

class Cinema
{
private:
	int C_Row;
	int C_Col;
	bool C_Close;

public:
	void setValue(int row, int col, bool close)
	{
		this->C_Row = row;
		this->C_Col = col;
		this->C_Close = close;
	}

	void setRow(int row)
	{
		this->C_Row = row;
	}

	void setCol(int col)
	{
		this->C_Col = col;
	}

	void setBool(bool close)
	{
		this->C_Close = close;
	}

	bool* getC_Close()
	{
		return &C_Close;
	}

	int getC_Row()
	{
		return this->C_Row;
	}

	int getC_Col()
	{
		return this->C_Col;
	}
};

void ShowPlace(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY])
{
	for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
	{
		std::cout << std::endl;
		std::cout << i + 1 << " Row" << TAB;
		for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
		{
			if (*array[i][j].getC_Close() == true)
			{
				std::cout << REDBACKGROUND << "| X |" << RESET << TAB;
			}
			else
			{
				std::cout << "| O |" << TAB;
			}
		}
		std::cout << std::endl;
		std::cout << TAB;
		for (int col = 0; col < CONST_GL_SIZE_ARRAY; col++)
		{
			std::cout << "__" << col + 1 << "__" << TAB;
		}
		std::cout << std::endl;
	}
}

void Detail(text name)
{
	std::cout << std::endl;
	if (name == "Spider-man")
	{
		std::cout << "21st Century's Menahem Golan still actively immersed himself mounting \"his\" Spider-Man, sending the original \"Doc Ock\" script for production bids. In 1990, he contacted Canadian effects company Light and Motion Corporation regarding the visual effects, which in turn offered the stop-motion chores to Steven Archer" << std::endl;
	}
	else if (name == "Iron-man")
	{
		std::cout << "Tony Stark, who has inherited the defense contractor Stark Industries from his late father Howard Stark, is in war-torn Afghanistan with his friend and military liaison, Lieutenant colonel James Rhodes, to demonstrate the new \"Jericho\" missile. After the demonstration, the convoy is ambushed and Stark is critically wounded by a missile used by the attackers: one of his company's own. He is captured and imprisoned in a cave by a terrorist group called the Ten Rings. Yinsen, a fellow captive doctor, implants an electromagnet into Stark's chest to keep the shrapnel shards that wounded him from reaching his heart and killing him. Ten Rings leader Raza offers Stark freedom in exchange for building a Jericho missile for the group, but he and Yinsen know that Raza will not keep his word." << std::endl;
	}
	else if (name == "Matrix")	
	{
		std::cout << "Computer programmer Thomas Anderson, known by his hacking alias \"Neo\", is puzzled by repeated online encounters with the phrase \"the Matrix\". Trinity contacts him and tells him a man named Morpheus has the answers Neo seeks. A team of Agents and police, led by Agent Smith, arrives at Neo's workplace in search of him. Though Morpheus attempts to guide Neo to safety, Neo surrenders rather than risk a dangerous escape. The Agents attempt to coerce Neo into helping them locate Morpheus, who they claim is a terrorist. When Neo refuses, the Agents fuse his mouth shut and implant a robotic \"bug\" in his stomach. Neo wakes up from what he believes to be a nightmare. Soon after, Neo is taken by Trinity to meet Morpheus, and she removes the bug from Neo, indicating that the \"nightmare\" he experienced was apparently real." << std::endl;
	}
	else if (name == "Captain America")
	{
		std::cout << "In New York City, Steve Rogers is rejected for World War II military recruitment due to his various health and physical problems. While attending an exhibition of future technologies with his best friend, Sgt. James \"Bucky\" Barnes, Rogers again attempts to enlist. Overhearing Rogers' conversation with Barnes about representing his country in the war, Dr. Abraham Erskine allows Rogers to enlist. He is recruited into the Strategic Scientific Reserve as part of a \"super - soldier\" experiment under Erskine, Colonel Chester Phillips, and British agent Peggy Carter. Phillips is unconvinced by Erskine's claims that Rogers is the right person for the procedure but relents after seeing Rogers jump on a grenade to save his comrades, unaware that it is a test. The night before the treatment, Erskine reveals to Rogers that Schmidt underwent the procedure prematurely and suffered permanent side-effects." << std::endl;
	}
	else if (name == "THOR")
	{
		std::cout << "In 965 AD, Odin, king of Asgard, wages war against the Frost Giants of Jotunheim and their leader Laufey, to prevent them from conquering the Nine Realms, starting with Earth. The Asgardian warriors defeat the Frost Giants in Tonsberg, Norway, and seize the source of their power, the Casket of Ancient Winters." << std::endl;
	}
	char button = _getch();
}

bool EnterRowAndCol(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY], int& freeSeats)
{
	while (true)
	{
		int countPeople = 0;
		int buttonNum = 0;
		char button = ' ';

		std::cout << std::endl;
		std::cout << "Enter the number of people" << std::endl;
		std::cin >> countPeople;
		std::cin.clear();

		while (countPeople >= freeSeats)
		{
			std::cout << "The number of people entered is greater than the number of vacancies" << std::endl;
			std::cout << "Enter the number of people" << std::endl;
			std::cin >> countPeople;
			std::cin.clear();
		}

		int** placeInCinema = new int* [countPeople];
		for (int i = 0; i < countPeople; i++)
		{
			placeInCinema[i] = new int[2];
		}

		for (int i = 0; i < countPeople; i++)
		{
			for (int j = 0; j < 2; j++)
			{
				if (j == 0)
				{
					std::cout << "Enter Row: ";
					std::cin >> placeInCinema[i][j];
				}
				else
				{
					std::cout << "Enter Col: ";
					std::cin >> placeInCinema[i][j];
				}

				if ((j == 1 && placeInCinema[i][0] <= 0)
					|| (j == 1 && placeInCinema[i][1] <= 0)
					|| (j == 1 && placeInCinema[i][0] >= 10)
					|| (j == 1 && placeInCinema[i][1] >= 10))
				{
					std::cout << "This place is close" << std::endl;
					j = -1;
				}

				if (j == 1)
				{
					for (int x = 0; x < CONST_GL_SIZE_ARRAY; x++)
					{
						for (int y = 0; y < CONST_GL_SIZE_ARRAY; y++)
						{
							if (array[x][y].getC_Row() == placeInCinema[i][0]
								&& array[x][y].getC_Col() == placeInCinema[i][1]
								&& *array[x][y].getC_Close())
							{
								std::cout << "This place is close" << std::endl;
								j = -1;
							}
						}
					}
				}

				if (j == 1)
				{
					bool* place = array[placeInCinema[i][0] - 1][placeInCinema[i][1] - 1].getC_Close();
					*place = true;
				}
			}
			std::cout << std::endl;
		}

		while (button != 'Q')
		{
			system("CLS");
			int cost = 80;
			std::cout << TAB << TAB << TAB << "Row" << TAB << "Col" << std::endl;
			for (int i = 0; i < countPeople; i++)
			{
				std::cout << "Reserved seats: ";
				for (int j = 0; j < 2; j++)
				{
					std::cout << TAB << placeInCinema[i][j];
				}
				std::cout << std::endl;
			}

			std::cout << std::endl << "The final price: " << cost * countPeople << std::endl << std::endl;

			if (buttonNum == 0)
			{
				std::cout << REDBACKGROUND << "[ Pay ]" << RESET << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 1)
			{
				std::cout << "Pay" << std::endl;
				std::cout << REDBACKGROUND << "[ Cancel ]" << RESET << std::endl;
			}

			button = _getch();

			switch (button)
			{
			case 'w':
				if (buttonNum != 0)
				{
					buttonNum -= 1;
				}
				break;
			case 's':
				if (buttonNum != 1)
				{
					buttonNum += 1;
				}
				break;
			case ' ':
				if (buttonNum == 0)
				{
					for (int i = 0; i < countPeople; i++)
					{
						delete[] placeInCinema[i];
						placeInCinema[i] = nullptr;
					}
					return false;
				}
				else if (buttonNum == 1)
				{
					for (int i = 0; i < countPeople; i++)
					{
						bool* place = array[placeInCinema[i][0] - 1][placeInCinema[i][1] - 1].getC_Close();
						*place = false;
					}

					for (int i = 0; i < countPeople; i++)
					{
						delete[] placeInCinema[i];
						placeInCinema[i] = nullptr;
					}
					return true;
				}
				break;
			}
		}
	}
}

bool Films(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY], text name, int& freeSeats)
{
	int buttonNum = 0;
	while (true)
	{
		system("CLS");
		std::cout << TAB << TAB << TAB << name << std::endl;
		ShowPlace(array);

		std::cout << std::endl;
		if (buttonNum == 0)
		{
			std::cout << REDBACKGROUND << "Choose place" << RESET << std::endl;
			std::cout << "Details" << std::endl;
			std::cout << "Back" << std::endl;
		}
		else if (buttonNum == 1)
		{
			std::cout << "Choose place" << std::endl;
			std::cout << REDBACKGROUND << "Details" << RESET << std::endl;
			std::cout << "Back" << std::endl;
		}
		else if (buttonNum == 2)
		{
			std::cout << "Choose place" << std::endl;
			std::cout << "Details" << std::endl;
			std::cout << REDBACKGROUND << "Back" << RESET << std::endl;
		}

		char button = _getch();

		switch (button)
		{
		case 'w':
			if (buttonNum != 0)
			{
				buttonNum -= 1;
			}
			break;
		case 's':
			if (buttonNum != 2)
			{
				buttonNum += 1;
			}
			break;
		case ' ':
			if (buttonNum == 0)
			{
				bool menu = EnterRowAndCol(array, freeSeats);
				if (!menu)
				{
					return menu;
				}
			}
			else if (buttonNum == 1)
			{
				Detail(name);
			}
			else if (buttonNum == 2)
			{
				return true;
			}
			break;
		}
	}
}

int freeSpaceInCinema(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY])
{
	int freeSpace = 0;
	for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
	{
		for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
		{
			if (*array[i][j].getC_Close() == false)
			{
				freeSpace++;
			}
		}
	}
	return freeSpace;
}

void randomSeat(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY])
{
	for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
	{
		for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
		{
			int randomsimbol = rand() % 2;
			if (randomsimbol == 1)
			{
				array[i][j].setValue(i + 1, j + 1, randomsimbol);
			}
			else
			{
				array[i][j].setValue(i + 1, j + 1, randomsimbol);
			}
		}
	}
}

void Export(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY], text name)
{
	std::ifstream fileExport;
	std::ofstream fileImport;
	std::stringstream ConvertIntToStr;
	text ImportNewData;
	text buffer;

	if (name == "Spider-man")
	{
		fileExport.open("Spider-man.txt");
		if (!fileExport.is_open())
		{
			// File is not open
			// Create File
			fileImport.open("Spider-man.txt", std::ios_base::trunc);
			randomSeat(array);
			for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
			{
				for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
				{
					ImportNewData += "\nRow: ";

					ConvertIntToStr << array[i][j].getC_Row();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nCol: ";

					ConvertIntToStr << array[i][j].getC_Col();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nClose: ";

					ConvertIntToStr << *array[i][j].getC_Close();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();
				}
			}
			fileImport << ImportNewData;
			ImportNewData.clear();
			fileImport.close();
		}
		fileExport.close();
		fileExport.open("Spider-man.txt");
		if (fileExport.is_open())
		{
			// File is open
			while (!fileExport.eof())
			{
				for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
				{
					for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
					{
						fileExport >> buffer;

						// Row
						fileExport >> buffer;
						array[i][j].setRow(stoi(buffer));
						fileExport >> buffer;

						// Col
						fileExport >> buffer;
						array[i][j].setCol(stoi(buffer));

						fileExport >> buffer;

						// Close
						fileExport >> buffer;
						array[i][j].setBool(stoi(buffer));
					}
				}
			}
		}
		fileExport.close();
	}
	else if (name == "Iron-man")
	{
		fileExport.open("Iron-man.txt");
		if (!fileExport.is_open())
		{
			// File is not open
			// Create File
			fileImport.open("Iron-man.txt", std::ios_base::trunc);
			randomSeat(array);
			for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
			{
				for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
				{
					ImportNewData += "\nRow: ";

					ConvertIntToStr << array[i][j].getC_Row();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nCol: ";

					ConvertIntToStr << array[i][j].getC_Col();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nClose: ";

					ConvertIntToStr << *array[i][j].getC_Close();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();
				}
			}
			fileImport << ImportNewData;
			ImportNewData.clear();
			fileImport.close();
		}
		fileExport.close();
		fileExport.open("Iron-man.txt");
		if (fileExport.is_open())
		{
			// File is open
			while (!fileExport.eof())
			{
				for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
				{
					for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
					{
						fileExport >> buffer;

						// Row
						fileExport >> buffer;
						array[i][j].setRow(stoi(buffer));
						fileExport >> buffer;

						// Col
						fileExport >> buffer;
						array[i][j].setCol(stoi(buffer));

						fileExport >> buffer;

						// Close
						fileExport >> buffer;
						array[i][j].setBool(stoi(buffer));
					}
				}
			}
		}
		fileExport.close();
	}
	else if (name == "Matrix")
	{
		fileExport.open("Matrix.txt");
		if (!fileExport.is_open())
		{
			// File is not open
			// Create File
			fileImport.open("Matrix.txt", std::ios_base::trunc);
			randomSeat(array);
			for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
			{
				for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
				{
					ImportNewData += "\nRow: ";

					ConvertIntToStr << array[i][j].getC_Row();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nCol: ";

					ConvertIntToStr << array[i][j].getC_Col();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nClose: ";

					ConvertIntToStr << *array[i][j].getC_Close();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();
				}
			}
			fileImport << ImportNewData;
			ImportNewData.clear();
			fileImport.close();
		}
		fileExport.close();
		fileExport.open("Matrix.txt");
		if (fileExport.is_open())
		{
			// File is open
			while (!fileExport.eof())
			{
				for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
				{
					for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
					{
						fileExport >> buffer;

						// Row
						fileExport >> buffer;
						array[i][j].setRow(stoi(buffer));
						fileExport >> buffer;

						// Col
						fileExport >> buffer;
						array[i][j].setCol(stoi(buffer));

						fileExport >> buffer;

						// Close
						fileExport >> buffer;
						array[i][j].setBool(stoi(buffer));
					}
				}
			}
		}
		fileExport.close();
	}
	else if (name == "Captain America")
	{
		fileExport.open("Captain America.txt");
		if (!fileExport.is_open())
		{
			// File is not open
			// Create File
			fileImport.open("Captain America.txt", std::ios_base::trunc);
			randomSeat(array);
			for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
			{
				for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
				{
					ImportNewData += "\nRow: ";

					ConvertIntToStr << array[i][j].getC_Row();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nCol: ";

					ConvertIntToStr << array[i][j].getC_Col();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nClose: ";

					ConvertIntToStr << *array[i][j].getC_Close();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();
				}
			}
			fileImport << ImportNewData;
			ImportNewData.clear();
			fileImport.close();
		}
		fileExport.close();
		fileExport.open("Captain America.txt");
		if (fileExport.is_open())
		{
			// File is open
			while (!fileExport.eof())
			{
				for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
				{
					for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
					{
						fileExport >> buffer;

						// Row
						fileExport >> buffer;
						array[i][j].setRow(stoi(buffer));
						fileExport >> buffer;

						// Col
						fileExport >> buffer;
						array[i][j].setCol(stoi(buffer));

						fileExport >> buffer;

						// Close
						fileExport >> buffer;
						array[i][j].setBool(stoi(buffer));
					}
				}
			}
		}
		fileExport.close();
	}
	else if (name == "THOR")
	{
		fileExport.open("THOR.txt");
		if (!fileExport.is_open())
		{
			// File is not open
			// Create File
			fileImport.open("THOR.txt", std::ios_base::trunc);
			randomSeat(array);
			for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
			{
				for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
				{
					ImportNewData += "\nRow: ";

					ConvertIntToStr << array[i][j].getC_Row();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nCol: ";

					ConvertIntToStr << array[i][j].getC_Col();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();

					ImportNewData += "\nClose: ";

					ConvertIntToStr << *array[i][j].getC_Close();
					ImportNewData += ConvertIntToStr.str();
					ConvertIntToStr.str(text());
					ConvertIntToStr.clear();
				}
			}
			fileImport << ImportNewData;
			ImportNewData.clear();
			fileImport.close();
		}
		fileExport.close();
		fileExport.open("THOR.txt");
		if (fileExport.is_open())
		{
			// File is open
			while (!fileExport.eof())
			{
				for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
				{
					for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
					{
						fileExport >> buffer;

						// Row
						fileExport >> buffer;
						array[i][j].setRow(stoi(buffer));
						fileExport >> buffer;

						// Col
						fileExport >> buffer;
						array[i][j].setCol(stoi(buffer));

						fileExport >> buffer;

						// Close
						fileExport >> buffer;
						array[i][j].setBool(stoi(buffer));
					}
				}
			}
		}
		fileExport.close();
	}
}

void Import(Cinema array[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY], text name)
{
	std::ofstream fileImport;
	std::stringstream ConvertIntToStr;
	text ImportNewData;

	if (name == "Spider-man")
	{
		fileImport.open("Spider-man.txt", std::ios_base::trunc);
		for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
		{
			for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
			{
				ImportNewData += "\nRow: ";

				ConvertIntToStr << array[i][j].getC_Row();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nCol: ";

				ConvertIntToStr << array[i][j].getC_Col();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nClose: ";

				ConvertIntToStr << *array[i][j].getC_Close();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();
			}
		}
		fileImport << ImportNewData;
		ImportNewData.clear();
		fileImport.close();
	}
	else if (name == "Iron-man")
	{
		fileImport.open("Iron-man.txt", std::ios_base::trunc);
		for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
		{
			for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
			{
				ImportNewData += "\nRow: ";

				ConvertIntToStr << array[i][j].getC_Row();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nCol: ";

				ConvertIntToStr << array[i][j].getC_Col();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nClose: ";

				ConvertIntToStr << *array[i][j].getC_Close();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();
			}
		}
		fileImport << ImportNewData;
		ImportNewData.clear();
		fileImport.close();
	}
	else if (name == "Matrix")
	{
		fileImport.open("Matrix.txt", std::ios_base::trunc);
		for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
		{
			for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
			{
				ImportNewData += "\nRow: ";

				ConvertIntToStr << array[i][j].getC_Row();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nCol: ";

				ConvertIntToStr << array[i][j].getC_Col();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nClose: ";

				ConvertIntToStr << *array[i][j].getC_Close();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();
			}
		}
		fileImport << ImportNewData;
		ImportNewData.clear();
		fileImport.close();
	}
	else if (name == "Captain America")
	{
		fileImport.open("Captain America.txt", std::ios_base::trunc);
		for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
		{
			for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
			{
				ImportNewData += "\nRow: ";

				ConvertIntToStr << array[i][j].getC_Row();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nCol: ";

				ConvertIntToStr << array[i][j].getC_Col();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nClose: ";

				ConvertIntToStr << *array[i][j].getC_Close();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();
			}
		}
		fileImport << ImportNewData;
		ImportNewData.clear();
		fileImport.close();
	}
	else if (name == "THOR")
	{
		fileImport.open("THOR.txt", std::ios_base::trunc);
		for (int i = 0; i < CONST_GL_SIZE_ARRAY; i++)
		{
			for (int j = 0; j < CONST_GL_SIZE_ARRAY; j++)
			{
				ImportNewData += "\nRow: ";

				ConvertIntToStr << array[i][j].getC_Row();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nCol: ";

				ConvertIntToStr << array[i][j].getC_Col();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();

				ImportNewData += "\nClose: ";

				ConvertIntToStr << *array[i][j].getC_Close();
				ImportNewData += ConvertIntToStr.str();
				ConvertIntToStr.str(text());
				ConvertIntToStr.clear();
			}
		}
		fileImport << ImportNewData;
		ImportNewData.clear();
		fileImport.close();
	}
}

int main()
{
	srand(time(0));

	Cinema cinemaSiteSpiderMan[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY];
	Cinema cinemaSiteIronMan[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY];
	Cinema cinemaSiteMatrix[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY];
	Cinema cinemaSiteCaptainAmerica[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY];
	Cinema cinemaSiteTHOR[CONST_GL_SIZE_ARRAY][CONST_GL_SIZE_ARRAY];

	// file function
	Export(cinemaSiteSpiderMan, "Spider-man");
	Export(cinemaSiteIronMan, "Iron-man");
	Export(cinemaSiteMatrix, "Matrix");
	Export(cinemaSiteCaptainAmerica, "Captain America");
	Export(cinemaSiteTHOR, "THOR");

	int freeSpaceSpiderMan = freeSpaceInCinema(cinemaSiteSpiderMan);
	int freeSpaceIronMan = freeSpaceInCinema(cinemaSiteIronMan);
	int freeSpaceMatrix = freeSpaceInCinema(cinemaSiteMatrix);
	int freeSpaceCaptainAmerica = freeSpaceInCinema(cinemaSiteCaptainAmerica);
	int freeSpaceTHOR = freeSpaceInCinema(cinemaSiteTHOR);

	char button;
	int buttonNum = 0;
	int bottomButton = 5;
	int menuButton = 1;
	bool filmEnter = false;

	while (true)
	{
		if (!filmEnter)
		{
			system("CLS");
			std::cout << TAB << "~ Menu ~" << std::endl;
			if (buttonNum == 0)
			{
				std::cout << REDBACKGROUND << "Films" << RESET << std::endl;
				std::cout << "Exit" << std::endl;
			}
			else if (buttonNum == 1)
			{
				std::cout << "Films" << std::endl;
				std::cout << REDBACKGROUND << "Exit" << RESET << std::endl;
			}

			button = _getch();

			switch (button)
			{
			case 'w':
				if (buttonNum == 0)
				{
					buttonNum = 0;
				}
				else
				{
					buttonNum -= 1;
				}
				break;
			case 's':
				if (buttonNum == menuButton)
				{
					buttonNum = menuButton;
				}
				else
				{
					buttonNum += 1;
				}
				break;
			case ' ':
				if (buttonNum == 0)
				{
					filmEnter = true;
				}
				else
				{
					return 0;
				}
				break;
			}
		}

		if (filmEnter)
		{
			system("CLS");
			std::cout << "~ FILMS ~" << TAB << "Free Space" << std::endl;
			if (buttonNum == 0)
			{
				std::cout << REDBACKGROUND << "Spider-man" << RESET << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << "Iron-man" << TAB << freeSpaceIronMan << std::endl;
				std::cout << "Matrix" << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << "Captain America" << TAB << freeSpaceCaptainAmerica << std::endl;
				std::cout << "THOR" << TAB << TAB << freeSpaceTHOR << std::endl << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 1)
			{
				std::cout << "Spider-man" << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << REDBACKGROUND << "Iron-man" << RESET << TAB << freeSpaceIronMan << std::endl;
				std::cout << "Matrix" << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << "Captain America" << TAB << freeSpaceCaptainAmerica << std::endl;
				std::cout << "THOR" << TAB << TAB << freeSpaceTHOR << std::endl << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 2)
			{
				std::cout << "Spider-man" << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << "Iron-man" << TAB << freeSpaceIronMan << std::endl;
				std::cout << REDBACKGROUND << "Matrix" << RESET << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << "Captain America" << TAB << freeSpaceCaptainAmerica << std::endl;
				std::cout << "THOR" << TAB << TAB << freeSpaceTHOR << std::endl << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 3)
			{
				std::cout << "Spider-man" << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << "Iron-man" << TAB << freeSpaceIronMan << std::endl;
				std::cout << "Matrix" << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << REDBACKGROUND << "Captain America" << TAB << RESET << freeSpaceCaptainAmerica << std::endl;
				std::cout << "THOR" << TAB << TAB << freeSpaceTHOR << std::endl << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 4)
			{
				std::cout << "Spider-man" << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << "Iron-man" << TAB << freeSpaceIronMan << std::endl;
				std::cout << "Matrix" << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << "Captain America" << TAB << freeSpaceCaptainAmerica << std::endl;
				std::cout << REDBACKGROUND << "THOR" << RESET << TAB << TAB << freeSpaceTHOR << std::endl << std::endl;
				std::cout << "Cancel" << std::endl;
			}
			else if (buttonNum == 5)
			{
				std::cout << "Spider-man" << TAB << freeSpaceSpiderMan << std::endl;
				std::cout << "Iron-man" << TAB << freeSpaceIronMan << std::endl;
				std::cout << "Matrix" << TAB << TAB << freeSpaceMatrix << std::endl;
				std::cout << "Captain America" << TAB << freeSpaceCaptainAmerica << std::endl;
				std::cout << "THOR" << TAB << TAB << freeSpaceTHOR << std::endl  << std::endl;
				std::cout << REDBACKGROUND << "Cancel" << RESET << std::endl;
			}

			button = _getch();

			switch (button)
			{
			case 'w':
				if (buttonNum == 0)
				{
					buttonNum = 0;
				}
				else
				{
					buttonNum -= 1;
				}
				break;
			case 's':
				if (buttonNum == bottomButton)
				{
					buttonNum = bottomButton;
				}
				else
				{
					buttonNum += 1;
				}
				break;
			case ' ':
				if (buttonNum == 0)
				{
					// Spider-man
					buttonNum = 0;
					filmEnter = Films(cinemaSiteSpiderMan, "Spider-man", freeSpaceSpiderMan);
					if (!filmEnter)
					{
						Import(cinemaSiteSpiderMan, "Spider-man");
					}
				}
				else if (buttonNum == 1)
				{
					// Iron-man
					buttonNum = 0;
					filmEnter = Films(cinemaSiteIronMan, "Iron-man", freeSpaceIronMan);
					if (!filmEnter)
					{
						Import(cinemaSiteIronMan, "Iron-man");
					}
				}
				else if (buttonNum == 2)
				{
					// Matrix
					buttonNum = 0;
					filmEnter = Films(cinemaSiteMatrix, "Matrix", freeSpaceMatrix);
					if (!filmEnter)
					{
						Import(cinemaSiteMatrix, "Matrix");
					}
				}
				else if (buttonNum == 3)
				{
					// Captain America
					buttonNum = 0;
					filmEnter = Films(cinemaSiteCaptainAmerica, "Captain America", freeSpaceCaptainAmerica);
					if (!filmEnter)
					{
						Import(cinemaSiteCaptainAmerica, "Captain America");
					}
				}
				else if (buttonNum == 4)
				{
					// THOR
					buttonNum = 0;
					filmEnter = Films(cinemaSiteTHOR, "THOR", freeSpaceTHOR);
					if (!filmEnter)
					{
						Import(cinemaSiteTHOR, "THOR");
					}
				}
				else
				{
					// Cancel
					buttonNum = 0;
					filmEnter = false;
				}
			}
		}
	}
}
