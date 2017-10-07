#include <iostream>
#include <windows.h> // esta libreria pemite usar Sleep

#define CProductos 5
#define CLAVE "pedro"
using namespace std; 


// variables globales 

short cantidadProducto[CProductos];
short precioProducto[CProductos];
short saldo;
int saldoMaquina;
string nombreProducto[CProductos] ;
short menorPrecio;

// prototipos de funciones
bool inicializar_productos(void);
void inventario(void);
bool abrir(void);
bool disponibidadProducto(short);
bool recibirDinero(void);
bool retirarDinero(void);
bool verificarDevueltas(short);
bool entregarProducto(short);
bool verificarValorCompra(short);
// programa 

int main(int argc, char** argv) {
	
	string entrada = "vacia" ; // a para ingreesar saldo , S para salir , pedro clave personal
	short productoID=-1;
	if(inicializar_productos()){
		
		do{	
			do{
				system("cls");
				cout<<"EL SALDO ACTUAL ES : "<< saldo<<endl;				
				cout<<"Oprima la tecla : "<<endl;
				cout<<"A si quiere ingresar dinero a la maquina."<<endl;
				cout<<"C si quiere realizar una comprar"<<endl;
				cout<<"R si quiere retirar su saldo"<<endl;
				cin>>entrada;
			}while(entrada != "A" && entrada != "B"  && entrada != CLAVE && entrada != "R"  && entrada != "C");
			
			if(entrada == CLAVE) abrir();
			else if(entrada =="A" ) recibirDinero();
			else if(entrada =="R" ) retirarDinero();
			else if(entrada =="C" ) {
				
				do{
					system("CLS");
					inventario();
					cout<<"Ingrese el ID del producto que quiere comprar o presiones E para salir"<<endl;
					cin>>entrada;
				}while(entrada != "A1" && entrada != "A2"  && entrada != "A3" && entrada != "A4"  && entrada != "A5" && entrada != "E");
			
				if (entrada == "E")entrada = "vacia";
				else {
					if(entrada == "A1" ) productoID= 0;
					else if (entrada == "A2") productoID= 1;
					else if (entrada == "A3") productoID= 2;
					else if (entrada == "A4") productoID= 3;
					else if (entrada == "A5") productoID= 4;
					
					entregarProducto(productoID);
					entrada = "vacia";
					Sleep(4000);
				}
			}
			
		}while(entrada != "B");
		
		system(0);
	
	}	
	else{
		cout<<"No se pudo inicializar la maquina"<<endl;
	}

	return 0;
}

/*----------------------------1. Funcion Incializar Productos----------------------------

Esta funcion tiene como objetivo inicializar los precios, nombres y cantidades de los 
productos disponibles en la maquina , cuando el operador abra la maquina podr acceder a estos
valores returna true si se hace adecuadamente
----------------------------------------------------------------------------------------*/
bool inicializar_productos(){
	saldo = 0 ;
	menorPrecio=500; // precio banana
	saldoMaquina =4000;
	// producto 1	
	cantidadProducto[0] = 4;
	precioProducto[0] = 3000;
	nombreProducto[0] = "Ensalada Rusa";
	
	// producto 1		
	cantidadProducto[1] = 5;
	precioProducto[1] = 4200; 
	nombreProducto[1] = "Ensalada Cesar";
	
	// producto 1	
	cantidadProducto[2] = 0;
	precioProducto[2] = 5800; 
	nombreProducto[2] ="Ensalada de Frutas";
	
	// producto 1	
	cantidadProducto[3] = 7;
	precioProducto[3] = 800; 
	nombreProducto[3] = "Manzana";
		
	// producto 1	
	cantidadProducto[4] = 9;
	precioProducto[4] = 500;
	nombreProducto[4] = "Banana";
	
	return true;
	
}

/*-----------------------------2. Funcion  Inventario ------------------------------

El objetivo de la funcion inventario es mostrar el estado actual del inventario
de productos en la maquina con us espectivos identificciones

no retorna nada
--------------------------------------------------------------------------------*/
void inventario(){
	
	cout<< "ID Producto    I    Cantidad    I    Precio    I    Producto"<<endl;
	for(int i=0; i< CProductos; i++){
		cout<<"    A"<<i+1<<"              ";
				cout<<"    "<<cantidadProducto[i]<<"    ";
		cout<<"         "<<precioProducto[i]<<"      ";
		cout<<"    "<<nombreProducto[i]<<"    "<<endl;
				
	}
}

/* ------------------------------3.  Funcion Abrir   --------------------------------------
la funcion abrir tiene como objetivo abrir la maquina para que el provedor pueda llenar
 la maquina con nuevos productos ademas cuando se abre se puede acceder al codigo
 
 retorna true una vez sea cerrada la puerta
-----------------------------------------------------------------------------------------*/
// todavia en constuccion
bool abrir(){
	// variales locales 
	char estado = 1; // 1 abierto , 0 cerrado
	
	cout<<endl<<endl<<"ABRIENDO PUERTA"<<endl;
	Sleep(2500);		
	
	do{
		system("CLS");	
		cout<<"					PUERTA ABIERTA              " <<endl;
		cout<<"EL SALDO MAQUINA ACTUAL ES : "<< saldoMaquina<<endl;
		inventario();
		cout<<"Oprima :" <<endl<<"cualquier tecla si Desea continuar con la puerta abierta "<<endl;
		cout<<"0 si desea cerrar la puerta "<<endl;
		cin>> estado;
	}while(estado != '0');
	
	system("CLS");
	cout<<"					CERRADO PUERTA 				"<<endl;
	Sleep(2500);// pausa de mil milisegundos
	//pausa
	
	return true;
}


/*----------------------------4. disponibidad Producto----------------------------------------

Esta funcion verifica si la cantidad de cualquier producto es mayor a cero en base a una
entrada n
retorna true si la cantidad es mayor que cero, y false en caso contrario

-----------------------------------------------------------------------------------------*/
bool disponibidadProducto(short n ){
	return cantidadProducto[n]>0;
}

/*-----------------------------5. recibir   Dinero------------------------------------------------

Esta funcion tiene como objetivo recibir el dinero ya sean billetes o monedas
retorna true si el saldo aumenta y false si el saldo es cero
----------------------------------------------------------------------------------------------*/

bool recibirDinero(){
	
	// variables 
	short opcion = 0, entrada;
	short saldoPasado=saldo;
	
	// mensaje base
	do{
		system("CLS");	
		cout<< "ingrese :"<<endl<<"1. si va insertar billete"<<endl<<"2. si son monedas"<<endl;
		cout<<"3. si quiere salir y recuperar su saldo"<<endl;
		cin>>opcion;
 	}while(opcion<=0 || opcion>3 );
	
	if(opcion==1){
		system("CLS");	
		cout<< "ingrese un billete de 1000, 2000 o 5000 Pesos"<<endl;
		cin>>entrada;
		
		switch(entrada){
			case 1000 : verificarDevueltas(1000);
				break;
			case 2000 : verificarDevueltas(2000);
				break;
			case 5000 : verificarDevueltas(5000);
				break;
			default : cout <<"billete no valido"<<endl;				
		}
		
	}
	else if(opcion==2){
		system("CLS");		
		cout<< "ingrese una moneda de 50, 100, 200, 500 o de 1000 Pesos"<<endl;
		cin>>entrada;
		
		switch(entrada){
			case 1000 : verificarDevueltas(1000);
				break;
			case 500 : verificarDevueltas(500);
				break;
			case 200 : verificarDevueltas(200);
				break;
			case 100 : verificarDevueltas(100);
				break;
			case 50 : verificarDevueltas(50);
				break;
			default : cout <<"Moneda no valida"<<endl;				
		}
	}
	else if(opcion==3){
		
		if(retirarDinero()){
			cout<<"Fin de la transaccion"<<endl;
		}
	}
	
	if(saldo >= saldoPasado || (saldoPasado == 0 && saldo==0 )) return true;
	else return false;
}


/*---------------------------6. Retirar Dinero------------------------------------------------

Esta funcion tiene como objetivo recibir el dinero ya sean billetes o monedas, y deja el saldo en cero
returna true siempre y cuando no ocurra nada extraordinario
----------------------------------------------------------------------------------------------*/
bool retirarDinero(){
	system("CLS");	
	cout<<endl<<endl<<endl<<"encender motor para sacar saldo de :"<<saldo<<endl<<endl<<endl<<endl;
	Sleep(1000);// pausa de mil milisegundos
	saldo=0;
	return true;
}

/*-----------------------------7. Verificar Devueltas------------------------------------------------

Esta funcion tiene como objetivo Vrificar si la maquina tiene Devueltas
retorna true si hay devueltas y false si no hay devultas
----------------------------------------------------------------------------------------------*/
bool verificarDevueltas(short entrada){
	
	if(entrada-menorPrecio >= saldoMaquina){
		cout<<"No Hay Devueltas ingrese dinero de menor denominacion"<<endl;
		cout<<"			MOTOR DEVOLVIENDO DINERO   "<<endl;
		Sleep(4000);// pausa de mil milisegundos
		return false;
	}
	saldo+=entrada;
	return true;
}

/*---------------------------8. Verificar Valor Compra------------------------------------------------

Esta funcion tiene como objetivo Verificar si con el saldo disponible se puede comprar producto
retorna true si es posible comprar y false si no alcanza el saldo
----------------------------------------------------------------------------------------------*/
bool verificarValorCompra(short entrada){
	return precioProducto[entrada]<=saldo;
}

/*----------------------------9. Entregar Producto------------------------------------------------

Esta funcion tiene como objetivo entregar el producto 
retorna true si hay devueltas y false si no hay devultas
----------------------------------------------------------------------------------------------*/
bool entregarProducto(short entrada){
	if(disponibidadProducto(entrada)){	
		if(verificarValorCompra( entrada )){
			system("CLS");	
			cout<<endl<<endl<<endl<<"encender motor para sacar : "<<nombreProducto[entrada]<<endl;
			Sleep(4000);
			cantidadProducto[entrada] -=1;
			saldoMaquina += precioProducto[entrada];
			saldo -= precioProducto[entrada];
			retirarDinero();
			return true;	
		}
		
		cout<<"!! Saldo Insuficiente !!"<<endl;
		return false;

	}
	cout<<"se agotaron las existencias de "<< nombreProducto[entrada]<<endl;
	return false;
}

