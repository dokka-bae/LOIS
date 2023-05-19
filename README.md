# LOIS LAB_1
To use:

	#include <windows.h>
	#include <iostream>
	int main() {
		// Загрузка DLL библиотеки
		HMODULE SKNF_CHECKER_DLL = LoadLibrary("./SKNF_CHECKER.dll");
		if (!SKNF_CHECKER_DLL) {
			std::cerr << "Error loading DLL\n";
			return 1;
		}
		typedef bool (*function)(std::string, bool);
		function SKNF_VALIDATION = (function)GetProcAddress(SKNF_CHECKER_DLL, "SKNF_VALIDATION");
		if (!SKNF_VALIDATION) {
			std::cerr << "Wrong function name\n";
			return 1;
		}
		//std::string expression = "(!a\\/a\\/c)/\\(!a\\/!b\\/c)/\\(!a\\/!b\\/!c)";
		//std::string expression = "(b\\/a\\/c)/\\(!a\\/!(b)\\/c)/\\(!a\\/!b\\/!c)";
		//std::string expression = "(!a\\/a\\/c)/\\(!a\\/";
		std::string expression = "a";
		//std::string expression = "(b\\/!a)/\\(c\\/a)";
		bool is_sknf = SKNF_VALIDATION(expression, true);
		is_sknf ? std::cout << "It is a SKNF": std::cout << "It is not a SKNF";

		FreeLibrary(SKNF_CHECKER_DLL);

		return 0;
	}
To compile:
	
	g++ -o main.exe main.cpp -L. -lSKNF_CHECKER;./main.exe
