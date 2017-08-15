#include <iostream>
#include <string>
#include <fstream>

using namespace std;

const long MEMORY_SIZE = 3000;
const long FILE_SIZE = 3000;

long value(string theString)
{
	int sign = 1;
	long resault = 0;
	string th = theString;
	if(th[0] == '-')
		sign = -1;
	else
		resault = th[0] - '0';
	for(int i = 1; i < th.size() ; i++)
	{
		resault *= 10;
		resault += th[i] - '0';
	}
	return resault * sign;
}

////////////////////////////////////// main function
int main(int argc, char** argv)
{
	long long memory[MEMORY_SIZE];
	int reg[10];
	ifstream codefile;
	codefile.open("code.fcode");
	string code[FILE_SIZE];
	string command;
	long a, b, c;
	long filesize = 0;
	
	//load code into computer memory.
	while(!codefile.eof())
		codefile >> code[ filesize++ ];
		
	//interprete the code.
	for(long i = 0; i < filesize-1; i++)
	{
		command = code[i];
		//nop for no operation.
		if(command == "nop")
			continue;
		
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
			c = value(code[++i]);
			memory[a] = memory[b] + memory[c];
		}
		
		//product for multiplying. product a b c --> a = b * c			
		else if(command == "product")
		{
			a = value(code[++i]);
			b = value(code[++i]);
			c = value(code[++i]);
			memory[a] = memory[b] * memory[c];
		}
		
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
			cout << code[++i];
		}
		
				
		else
			cout << "undefined command: " << command << endl;
	}
	
	cout << endl;	
	codefile.close();
	return 0;
}

