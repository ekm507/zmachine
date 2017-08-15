#include <iostream>
#include <string>
#include <fstream>
#include <map>

using namespace std;

//size of machines initial memory.
const long MEMORY_SIZE = 3000;

//size of machines FLASH memory.
const long FILE_SIZE = 3000;

const long STACK_SIZE = 1000;



//a map for containing variable names.
map<string, long long> variables;

//a map for tagging program parts.
map<string, long long> lables;


//convert string to plain number.
long value(string theString)
{
	//number's sign (+ or -)
	int sign = 1;
	
	//the number(abs value.)
	long resault = 0;
	
	string th = theString;
	if(th[0] == '-')
		sign = -1;
	else if(th[0] == '%' || th[0] == '#')
		resault = 0;
	else if(th[0] <= 'z' && th[0] >= 'A' || th[0] == '_')
	{
		return variables.find(th)->second;
	}
	else
		resault = th[0] - '0';
	for(int i = 1; i < th.size() ; i++)
	{
		resault *= 10;
		resault += th[i] - '0';
	}
	return resault * sign;
}


////////////////////////MAIN FUNCTION////////////////////
int main(int argc, char** argv)
{
	//works as machine's RAM.
	long long memory[MEMORY_SIZE];
	
	long long address_stack[STACK_SIZE];
	long long stack_last = 0;
	
	//works as CPU's register.
	int reg[10];
	
	
	//initial program to boot from.
	ifstream codefile;
	codefile.open("code.fcode");
	
	string code[FILE_SIZE];
	string command;
	string command_var;
	long a, b, c;
	long filesize = 0;
	
	//load code into computer memory.
	while(!codefile.eof())
		codefile >> code[ filesize++ ];
	
	//set label map and variables map.
	for(long long i = 0; i < filesize-1; i++)
	{
		command = code[i];
		if(command == "label")
		{
			lables[ code[i+1] ] = i;
		}
		
		else if(command == "var")
		{
			variables[ code[i+1] ] = value(code[i+2]);
		}
		
	}
	
	//interprete the code.
	for(long long i = 0; i < filesize-1; i++)
	{
		command = code[i];
		//nop for no operation.
		if(command == "nop")
			continue;
		
		////////////////////MATHEMATICAL COMMANDS////////////////
		
		//set . set a b --> a = value(b)
		else if(command == "set")
		{
			a = value(code[++i]);		
			b = value(code[++i]);
			memory[a] = b;
		}

		//mov for copying. mov a b --> a = b.
		else if(command == "mov")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[b];
		}
		
		//add for adding. add a b c --> a = b + c			
		else if(command == "add")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] + memory[b];
		}
		
		//xor
		else if(command == "xor")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] xor memory[b];
		}
		
		
		//and
		else if(command == "and")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] and memory[b];
		}
		
		
		//or
		else if(command == "or")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] or memory[b];
		}
		
		
		//product for multiplying. product a b c --> a = b * c			
		else if(command == "product")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] * memory[b];
		}
		
		
		//divide
		else if(command == "divide")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] / memory[b];
		}
		
		
		//mod
		else if(command == "mod")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			memory[a] = memory[a] % memory[b];
		}
		
		
		
		else if(command == "var")
			i+=2;
			
		else if(command == "lable")
			i++;
			
		////////////////////CONTROL FLOW COMMANDS////////////////
		//jump between code lines. jump a --> go a lines forward.
		else if(command == "jump")
		{
			a = value(code[++i]);
			i += a;
			i--;
			continue;
		}
		
		//jump if last equality test was true.
		else if(command == "je")
		{	
			a = value(code[++i]);
			if(reg[1])
			{
				i += a;
				i--;
			}
			continue;
		}
		
		
		//jump if last equality test was false.
		else if(command == "jne")
		{	
			a = value(code[++i]);
			if(!reg[1])
			{
				i += a;
				i--;
			}
			continue;
		}
		
		
		//test if two memory stages have equal numbers.
		else if(command == "equality")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			if(memory[a] == memory[b])
				reg[1] = 1;
			else
				reg[1] = 0;
		}
		
		////////////////////MARKDOWN COMMANDS///////////////////////
		else if(command == "<")
		{
			while(command != ">")
			{
				command = code[++i];
			}
			continue;
		}
		////////////////////DEBUGGING COMMANDS//////////////////////
		
		//print for debugging. print value of a memory stage.
		else if(command == "print")
		{
			a = value(code[++i]);
			cout << memory[a] << endl;
		}
		
		//oupput a message. for debgging.
		else if(command == "write")
		{
			cout << code[++i] << endl;
		}
		
		//outut a message. for debugging.(without endl)
		else if(command == "write2")
		{
			command = code[++i];
			if(command == "\"")
			{
				command = code[++i];
				while(command != "\"")
				{
					cout << command <<" ";
					command = code[++i];
				}
			}
			else
				cout << command;
			cout << endl;
			continue;
		}
		
		else if(command == "get")
		{
			a = value(code[++i]);
			cin >> memory[a];
		}
				
		else
			cout << "undefined command: " << command << endl;
	}
	
	cout << endl;	
	codefile.close();
	return 0;
}




/*
A method






*/

