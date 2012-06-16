# define F_CPU 16000000L
#include <avr/io.h>
#include <math.h>
#include <util/delay.h>    // including the avr delay lib
#include <util/delay.h>		// Including the avr delay lib
#include "Mirf.cpp"
#include "usart.cpp"

Nrf24l nrf = Nrf24l();

int main(void) {
	uint8_t data[nrf.payload];
	uint8_t first_packets[nrf.payload];
	uint8_t aux,counter;
	
	USART_Init(MYUBRR);
	
	/*while(1){
		if(((UCSR0A & (1<<RXC0)))){
			uint8_t aux = UDR0;
			USART_Send_int(aux);
		}
		_delay_ms(10);
	}*/
	
	nrf.init();
	
	/*USART_Send_string("value before: ");
	
	nrf.readRegister(0x00,&aux,1);
	
	USART_Send_int(aux);
	
	USART_Send_string("\n");
	
	USART_Send_string("value after: ");
	nrf.readRegister(0x00,&aux,1);
	
	USART_Send_int(aux);*/
	
	/* Espera o embarcado responder */
	
	nrf.setRADDR((uint8_t *)"canel");
	nrf.config();
	
	nrf.ledHigh();
	//USART_Send_string("waiting\n");
	while(!nrf.dataReady());
	//USART_Send_string("pacote recebido..\n");
	
	nrf.getData(first_packets);
	
	for(int i=0;i<nrf.payload;i++){
		// Envia para a Serial o payload 
		USART_Send_int(first_packets[i]);
		data[i] = 0;
	}
	
	nrf.setTADDR((uint8_t *)"robot");
	
	//USART_Send_string("enviando..\n");
	nrf.send(data);
	
	while(nrf.isSending()); 
	
	//USART_Send_string("enviado..\n");
	
	// espera o 000
	if(!nrf.dataReady()) { // Chegou um pacote
	
		//USART_Send_string("recebido..\n");
		nrf.getData(data);
	
		for(int i = 0; i < nrf.payload; i++) {
			// Envia para a Serial o payload
			USART_Send_int(data[i]);
		}		
	
		//USART_Send_string("enviei para serial\n");
	}
	
	nrf.ledTg();
	
	//USART_Send_string("\n");
	
	counter = 0;
	
    while(1) {
		if(nrf.dataReady()) { // Chegou um pacote
		
			nrf.getData(data);
		
			//for(int i=0;i<nrf.payload;i++){
			//	USART_Send_int(data[i]);
			//}
			//USART_Send_string("porco\n");
		
		}
		else if(((UCSR0A & (1<<RXC0)))) { // Chegou um byte na Serial
		
			data[counter] = UDR0;
			counter++;
			
			if(counter == nrf.payload) {
				nrf.setTADDR((uint8_t *)"robot");
				nrf.send(data);
			
				//USART_Send_string("enviando..\n");
			
				while(nrf.isSending()); // espera enviar o pacote
			
				//USART_Send_string("enviado... Esperando\n");
			
				while(!nrf.dataReady()); // espera a resposta
			
				//USART_Send_string("recebido na manolagem \n");
			
				nrf.getData(data);
			
				for(int i = 0; i < nrf.payload; i++) {
					USART_Send_int(data[i]);
				}
				
				//USART_Send_string("\n\r");
				nrf.ledTg();
				counter = 0;
			}
		}
		
		_delay_ms(10);
    }
}
