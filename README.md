# Yuridia-Elizondo

//2086179

#include <iostream>
#include <conio.h>		//Para el getline
#include <string.h>     //Para las cadenas de texto
#include <string>       
#include <fstream>      //Para crear el archivo txt
#include <stdlib.h>     //funcione new y delete (punteros)

using namespace std;

//definir las funciones
void AgendarCita();
void ModificarCita();
void EliminarCita();
void ListasVigentes();
void ArchivoTXT();

//Variables a usar durante todo el programa
int cantidadCitas;
int *hora, * minuto, * cantidadTratamiento;
float * precioUnitario, * total, * subtotal, *iva;
string * nombre, * nombreTratamiento, * descripcion;

//Función para agendar una cita
void AgendarCita()
{
	int contador = 0;
	cout << endl << "*AGENDAR CITAS :D*" << endl;
	cout << " Ingrese la cantidad de citas que va a dar de alta: " << endl;
	cin >> cantidadCitas;
	while (cantidadCitas < 1) {
		cout << "Error. Debe ingresar un numero valido. " << endl;
		cout << " Ingrese la cantidad de citas que va a dar de alta: " << endl;
		cin >> cantidadCitas;
	}
	//punteros
	nombre = new string[cantidadCitas];
	hora = new int[cantidadCitas];
	minuto = new int[cantidadCitas];
	nombreTratamiento = new string[cantidadCitas];
	descripcion = new string[cantidadCitas];
	cantidadTratamiento = new int[cantidadCitas];
	precioUnitario = new float[cantidadCitas];
	subtotal = new float[cantidadCitas];
	iva = new float[cantidadCitas];
	total = new float[cantidadCitas];

	for (int i = 0; i < cantidadCitas; i++)
	{
		char* validar;
		cout << endl << "Numero de registro: " << i + 1 << endl;
		cout << endl << "Inserte: " << endl;

		while (getchar() != '\n'); //se vacia el buffer o el espacio o cin.ignore
		//Caracteres especiales
		do {
			contador = 0; //contador por caracter, cuenta la separacion de la cadena de texto tipo string por caracteres
			cout << endl << "Nombre del paciente: " << endl;
			getline(cin, nombre[i]);
			//validar: variable para guardar los caracteres del string en un puntero tipo char del mismo tamaño que la cadena ingresada
			validar = new char[nombre[i].length() + 1];
			memmove(validar, nombre[i].c_str(), nombre[i].length()); //copia de la información del puntero string nombrePa a cada
			for (int c = 0; c < nombre[i].length(); c++) {		// espacio del puntero char correspondiente
				//Condicion del if dada por el codigo ASCII, cada caracter es evaluado dentro de un ciclo for para determinar si
				//alguno es un acento, signo, guion, etc
				if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
					//el contador suma si hay un caracter que no sea numerico sin acento
					contador = contador + 1;
					//cout << count << endl;
				}
			}
			if (contador > 0) { //si el contador suma aunque sea uno, se repetira el proceso de nuevo
				cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
			}
		} while (contador > 0);//se repite hasta que se ingrese una cadena valida

		cout << endl << "Hora del tratamiento. Ingrese por separado hora y minuto." << endl;
		cout << endl << "Ingrese la hora: " << endl;
		cin >> hora[i];
		cout << endl << "Ingrese el minuto: " << endl;
		cin >> minuto[i];
		while (hora[i] > 24 || hora[i] < 0 || minuto[i] > 60 || minuto[i] < 0) {
			cout << endl << "Error. Debe insertar numeros validos. " << endl;
			cout << endl << "Hora del tratamiento. Ingrese por separado hora y minuto." << endl;
			cout << endl << "Ingrese la hora: " << endl;
			cin >> hora[i];
			cout << endl << "Ingrese el minuto: " << endl;
			cin >> minuto[i];
		}
		if (hora[i] <= 24 && hora[i] >= 0 && minuto[i] < 60 && minuto[i] >= 0) {
			cout << endl << "La hora: " << hora[i] << ":" << minuto[i] << " es valida" << endl;
		}

		while (getchar() != '\n');
		contador = 0;
		do {
			contador = 0;
			cout << endl << "Nombre del tratamiento: " << endl;
			getline(cin, nombreTratamiento[i]);
			validar = new char[nombreTratamiento[i].length() + 1];
			memmove(validar, nombreTratamiento[i].c_str(), nombreTratamiento[i].length());
			for (int c = 0; c < nombreTratamiento[i].length(); c++) {
				if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
					contador = contador + 1;
					//cout << count << endl;
				}
			}
			if (contador > 0) {
				cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
			}
		} while (contador > 0);

		//while (getchar() != '\n');
		contador = 0;
		do {
			contador = 0;
			cout << endl << "Descripcion del tratamiento: " << endl;
			getline(cin, descripcion[i]);
			validar = new char[descripcion[i].length() + 1];
			memmove(validar, descripcion[i].c_str(), descripcion[i].length());
			for (int c = 0; c < descripcion[i].length(); c++) {
				if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
					contador = contador + 1;
					//cout << count << endl;
				}
			}
			if (contador > 0) {
				cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
			}
		} while (contador > 0);

		cout << endl << "Cantidad de tratamiento: " << endl;
		cin >> cantidadTratamiento[i];
		while (cantidadTratamiento[i] < 1) {
			cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
			cout << endl << "Cantidad de tratamiento: " << endl;
			cin >> cantidadTratamiento[i];
		}

		cout << endl << "Precio unitario: " << endl;
		cin >> precioUnitario[i];
		while (precioUnitario[i] <= 0) {
			cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
			cout << endl << "Precio unitario: " << endl;
			cin >> precioUnitario[i];
		}

		cout << endl << "Subtotal: " << endl;
		subtotal[i] = precioUnitario[i] * cantidadTratamiento[i];
		cout << "$ " << subtotal[i] << endl;

		cout << endl << "Iva: " << endl;
		iva[i] = precioUnitario[i] * cantidadTratamiento[i] * .16;
		cout << "$ " << iva[i] << endl;

		total[i] = subtotal[i] + iva[i];
		cout << endl << "Total: " << endl;
		cout << "$ " << total[i] << endl;
	}
}

//Funcion para modificar cita

void ModificarCita() {
	char* validar;
	int j, op2, contador = 0;
	cout << "*MODIFICAR CITA :D*" << endl << "Ingrese el numero de registro que desea modificar: " << endl;
	cin >> j;
	while (j < 1) {
		cout << "Error. Debe ingresar una cantidad valida" << endl;
		cout << "Ingrese el numero de registro que desea modificar: " << endl;
		cin >> j;
	}
	j = j - 1;
	cout << "Ingrse: " << endl << " 1 para modificar Nombre \n 2 para modificar Hora \n 3 para modificar Nombre del tratamiento \n ";
	cout << "4 para modificar Descripcion \n 5 para modificar Cantidad de tratamiento \n";
	cout << " 6 para modificar Precio Unitario \n 7 para modificar Todo\n ";
	cin >> op2;
	switch (op2) {
		//Nombre del Paciente
	case 1:
		for (int i = j; i == j; i++) //Nada mas se entra al ciclo for una vez
		{
			while (getchar() != '\n');
			do {
				contador = 0;
				cout << endl << "Nombre del paciente: " << endl;
				getline(cin, nombre[i]);
				validar = new char[nombre[i].length() + 1];
				memmove(validar, nombre[i].c_str(), nombre[i].length());
				for (int c = 0; c < nombre[i].length(); c++) {		
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);
		}
		break;
		//Hora del tratamiento
	case 2:
		for (int i = j; i == j; i++)
		{
			cout << endl << "Inserte hora del tratamiento. Inserte por separado hora y minutos. " << endl;
			cout << endl << "Ingerte la hora: " << endl;
			cin >> hora[i];
			cout << endl << "Inserte el minuto: " << endl;
			cin >> minuto[i];
			while (hora[i] > 24 || hora[i] < 0 || minuto[i] > 60 || minuto[i] < 0) {
				cout << endl << "Error. Debe insertar numeros validos." << endl;
				cout << endl << "Hora del tratamiento. Ingrese por hora, minuto y segundo. " << endl;
				cout << endl << "Ingrese la hora: " << endl;
				cin >> hora[i];
				cout << endl << "Ingrese el minuto: " << endl;
				cin >> minuto[i];
			}
			if (hora[i] <= 24 && hora[i] >= 0 && minuto[i] < 60 && minuto[i] >= 0) {
				cout << endl << "La hora: " << hora[i] << ":" << minuto[i] << " es valida" << endl;
			}
		}
		break;
		//Nombre del tratamiento
	case 3:
		for (int i = j; i == j; i++) {
			while (getchar() != '\n');
			contador = 0;
			do {
				contador = 0;
				cout << endl << "Nombre del tratamiento: " << endl;
				getline(cin, nombreTratamiento[i]);
				validar = new char[nombreTratamiento[i].length() + 1];
				memmove(validar, nombreTratamiento[i].c_str(), nombreTratamiento[i].length());
				for (int c = 0; c < nombreTratamiento[i].length(); c++) {
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);
		}
		break;
		//Descripcion
	case 4:
		for (int i = j; i == j; i++) {
			contador = 0;
			do {
				while (getchar() != '\n');
				contador = 0;
				cout << endl << "Descripcion del tratamiento: " << endl;
				getline(cin, descripcion[i]);
				validar = new char[descripcion[i].length() + 1];
				memmove(validar, descripcion[i].c_str(), descripcion[i].length());
				for (int c = 0; c < descripcion[i].length(); c++) {
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);
		}
		break;
		//Cantidad de tratamiento
	case 5:
		for (int i = j; i == j; i++) {
			cout << endl << "Cantidad de tratamiento: " << endl;
			cin >> cantidadTratamiento[i];
			while (cantidadTratamiento[i] < 1) {
				cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
				cout << endl << "Cantidad de tratamiento: " << endl;
				cin >> cantidadTratamiento[i];
			}
			subtotal[i] = precioUnitario[i] * cantidadTratamiento[i];
			iva[i] = subtotal[i] * .16;
			total[i] = subtotal[i] + iva[i];
		}
		break;
		//Precio Unitario
	case 6:
		for (int i = j; i == j; i++) {
			cout << endl << "Precio unitario: " << endl;
			cin >> precioUnitario[i];
			while (precioUnitario[i] <= 0) {
				cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
				cout << endl << "Precio unitario: " << endl;
				cin >> precioUnitario[i];
			}
			subtotal[i] = precioUnitario[i] * cantidadTratamiento[i];
			iva[i] = precioUnitario[i] * cantidadTratamiento[i] * .16;
			total[i] = subtotal[i] + iva[i];
		}
		break;
		//Todo
	case 7:
		for (int i = j; i == j; i++) {
			cout << endl << "Ingrese: " << endl;

			cout << endl << "Numero de registro: " << i + 1 << endl;
			cout << endl << "Inserte: " << endl;

			while (getchar() != '\n');
			do {
				contador = 0;
				cout << endl << "Nombre del paciente: " << endl;
				getline(cin, nombre[i]);
				validar = new char[nombre[i].length() + 1];
				memmove(validar, nombre[i].c_str(), nombre[i].length());
				for (int c = 0; c < nombre[i].length(); c++) {
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);

			cout << endl << "Hora del tratamiento. Ingrese por separado hora y minuto." << endl;
			cout << endl << "Ingrese la hora: " << endl;
			cin >> hora[i];
			cout << endl << "Ingrese el minuto: " << endl;
			cin >> minuto[i];
			while (hora[i] > 24 || hora[i] < 0 || minuto[i] > 60 || minuto[i] < 0) {
				cout << endl << "Error. Debe insertar numeros validos. " << endl;
				cout << endl << "Hora del tratamiento. Ingrese por separado hora y minuto." << endl;
				cout << endl << "Ingrese la hora: " << endl;
				cin >> hora[i];
				cout << endl << "Ingrese el minuto: " << endl;
				cin >> minuto[i];
			}
			if (hora[i] <= 24 && hora[i] >= 0 && minuto[i] < 60 && minuto[i] >= 0) {
				cout << endl << "La hora: " << hora[i] << ":" << minuto[i] << " es valida" << endl;
			}

			while (getchar() != '\n');
			contador = 0;
			do {
				contador = 0;
				cout << endl << "Nombre del tratamiento: " << endl;
				getline(cin, nombreTratamiento[i]);
				validar = new char[nombreTratamiento[i].length() + 1];
				memmove(validar, nombreTratamiento[i].c_str(), nombreTratamiento[i].length());
				for (int c = 0; c < nombreTratamiento[i].length(); c++) {
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);

			//while (getchar() != '\n');
			contador = 0;
			do {
				contador = 0;
				cout << endl << "Descripcion del tratamiento: " << endl;
				getline(cin, descripcion[i]);
				validar = new char[descripcion[i].length() + 1];
				memmove(validar, descripcion[i].c_str(), descripcion[i].length());
				for (int c = 0; c < descripcion[i].length(); c++) {
					if ((validar[c] < 32) || (validar[c] > 32 && validar[c] < 65) || (validar[c] > 90 && validar[c] < 97) || (validar[c] > 122)) {
						contador = contador + 1;
						//cout << count << endl;
					}
				}
				if (contador > 0) {
					cout << endl << "No se permiten caracteres especiales como los acentos." << endl;
				}
			} while (contador > 0);

			cout << endl << "Cantidad de tratamiento: " << endl;
			cin >> cantidadTratamiento[i];
			while (cantidadTratamiento[i] < 1) {
				cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
				cout << endl << "Cantidad de tratamiento: " << endl;
				cin >> cantidadTratamiento[i];
			}

			cout << endl << "Precio unitario: " << endl;
			cin >> precioUnitario[i];
			while (precioUnitario[i] <= 0) {
				cout << endl << "Error. Debe insertar una cantidad valida. " << endl;
				cout << endl << "Precio unitario: " << endl;
				cin >> precioUnitario[i];
			}

			cout << endl << "Subtotal: " << endl;
			subtotal[i] = precioUnitario[i] * cantidadTratamiento[i];
			cout << "$ " << subtotal[i] << endl;

			cout << endl << "Iva: " << endl;
			iva[i] = precioUnitario[i] * cantidadTratamiento[i] * .16;
			cout << "$ " << iva[i] << endl;

			total[i] = subtotal[i] + iva[i];
			cout << endl << "Total: " << endl;
			cout << "$ " << total[i] << endl;
		}
		break;
	default:
		cout << endl << "ERROR. Debe ingresar un número válido" << endl;
		break;
	}

}

//Funcion para eliminar citas

void EliminarCita()
{
	int j;
	cout << "*ELIMINAR CITA D:*" << endl;
	cout << "Inserte el numero de registro a eliminar: " << endl;
	cin >> j;
	while (j < 1) {
		cout << "Error. Debe ingresar una cantidad valida" << endl;
		cout << "Ingrese el numero de registro que desea modificar: " << endl;
		cin >> j;
	}
	j = j - 1;
	for (int i = j; i == j; i++)
	{
		cout << endl << "Eliminando registro numero: " << j + 1 << endl;

		nombre[i] = "...";
		hora[i] = 0;
		minuto[i] = 0;
		nombreTratamiento[i] = "...";
		descripcion[i] = "...";
		cantidadTratamiento[i] = 0;
		precioUnitario[i] = 0;
		subtotal[i] = 0;
		iva[i] = 0;
		total[i] = 0;
	}
}

//Función para mostrar lista de citas

void ListasVigentes() {
	cout << "*LISTA DE CITAS :D*" << endl;
	for (int i = 0; i < cantidadCitas; i++)
	{
		cout << endl << "Numero de registro: " << i + 1 << endl;

		cout << endl << "Nombre del paciente: " << endl;
		cout << nombre[i];

		cout << endl << "Hora del tratamiento: " << endl;
		cout << hora[i] << ":" << minuto[i] << " horas.";

		cout << endl << "Nombre del tratamiento:" << endl;
		cout << nombreTratamiento[i];

		cout << endl << "Descripcion del tratamiento: " << endl;
		cout << descripcion[i];

		cout << endl << "Cantidad de tratamiento: " << endl;
		cout << cantidadTratamiento[i];

		cout << endl << "Precio unitario: " << endl;
		cout << precioUnitario[i];

		cout << endl << "Subtotal: " << endl;
		cout << subtotal[i];

		cout << endl << "Iva: " << endl;
		cout << iva[i];

		cout << endl << "Total: " << endl;
		cout << total[i] << endl;
	}
}

//Funcion para salir y crear el archivo

void ArchivoTXT()
{
	ofstream archivo; //clase ifstream para trabajar en archivos
	string InfoCit; //Nombre del archivo
	int textoint;
	float textofloat;
	string textostring;

	InfoCit = "InfoCit.txt";

	archivo.open(InfoCit.c_str(), ios::out);

	if (archivo.fail())
	{
		cout << "ERROR. No se pudo crear el archivo" << endl;
		exit(1);
	}

	archivo << "\t" << "\t" << "\t" << "- LISTA -" << "\n";
	archivo << "NUMERO DE REGISTRO" << "\t";
	archivo << "NOMBRE DEL PACIENTE" << "\t";
	archivo << "HORA" << "\t" << "\t" << "\t";
	archivo << "NOMBRE DEL TRATAMIENTO" << "\t" << "\t";
	archivo << "DESCRIPCION DEL TRATAMIENTO" << "\t" << "\t";
	archivo << "CANTIDAD DE TRATAMIENTO" << "\t" << "\t";
	archivo << "PRECIO UNITARIO" << "\t" << "\t";
	archivo << "SUBTOTAL" << "\t";
	archivo << "IVA" << "\t" << "\t";
	archivo << "TOTAL" << "\n";

	for (int i = 0; i < cantidadCitas; i++)
	{
		textoint = i + 1;
		archivo << textoint << "\t" << "\t" << "\t";

		textostring = nombre[i];
		archivo << textostring << "\t" << "\t";

		textoint = hora[i];
		archivo << textoint << ":";

		textoint = minuto[i];
		archivo << textoint << " horas \t" << "\t";

		textostring = nombreTratamiento[i];
		archivo << textostring << "\t" << "\t" << "\t" << "\t";

		textostring = descripcion[i];
		archivo << textostring << "\t" << "\t" << "\t" << "\t";

		textoint = cantidadTratamiento[i];
		archivo << textoint << "\t " << "\t" << "\t" << "\t";

		textofloat = precioUnitario[i];
		archivo << textofloat << "\t " << "\t" << "\t";

		textofloat = subtotal[i];
		archivo << textofloat << "\t " << "\t ";

		textofloat = iva[i];
		archivo << textofloat << "\t " << "\t";

		textofloat = total[i];
		archivo << textofloat << "\t " << "\t" << "\n";
	}

	archivo.close();
}

//Menu principal

int main()
{
	int op;
	//Interfaz menu
	cout << " *BIENVENIDO :D*" << endl;

	cout << "Inserte:" << endl;
	cout << "1. Agendar cita." << endl;
	cout << "2. Modificar cita. " << endl;
	cout << "3. Eliminar cita. " << endl;
	cout << "4. Lista de citas vigentes." << endl;
	cout << "5. Limpiar pantalla." << endl;
	cout << "6. Salir." << endl;
	cin >> op;

	switch (op) {

		//Agendar cita
	case 1:
		AgendarCita(); //Funcion de tipo void
		return main();
		break;
		//Modificar cita
	case 2:
		ModificarCita();
		return main();
		break;
		//Eliminar Cita
	case 3:
		EliminarCita();
		return main();
		break;
		//Lista de Citas
	case 4:
		ListasVigentes();
		return main();
		break;
		//Limpiar Pantalla
	case 5:
		system("cls");
		return main();
		break;
		//Salir
	case 6:
		ArchivoTXT();
		break;
		//Información no válida
	default:
		cout << "ERROR. Debe ingresar un número válido" << endl;
		return main();
		break;
	}
}

