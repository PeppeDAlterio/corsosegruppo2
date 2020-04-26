/**
  @page LIN UART inter board Communication IT example

  @verbatim
  *****************************************************************************************************
  * @file    readme.md 
  * @author  Rosa Nuzzo, Mara Morabito, Giuseppe D'Alterio, Fernando Di Costanzo 
  * @brief   Descrizione della comunicazione tra USART (LIN) di una stessa Board STM32F446 Nucleo-64
  *****************************************************************************************************

  @endverbatim

@par Descrizione

Questo esempio descrive una trasmissione tra UART in modalità di LIN con interruzioni si di una stessa
board STM32F446 Nucleo-64.

All'inizio del programma principale viene chiamata la funzione HAL_Init () per ripristinare tutte 
le periferiche, inizializzare l'interfaccia Flash e il systick, quindi la funzione SystemClock_Config() 
viene utilizzata per configurare il clock di sistema (SYSCLK) per funzionare a 168 MHz.

Per avviare la fase di trasmissione, è sufficiente premere il bottone USER che accenderà il LED verde.
Fatto ciò, il programma sceglierà randomicamente quale stringa trasmettere, ogni secondo, tra:
- ROSA NUZZO M63000917
- MARA MORABITO M63000910
- GIUSEPPE D'ALTERIO M63000880
- FERNANDO DI COSTANZO M63000925
I dati sono scambiati via USART1 dal pin PA9 verso USART6 sul pin PC7. Al fine di supportare lo scambio di
stringhe di lunghezza variabile, abbiamo stabilito un protocllo di invio tale per cui il trasmettitore
invia prima la size della stringa e poi la stringa.

Una volta ricevuta la stringa questa viene inoltrata ad USART2 per l'invio sul porto seriale COM collegato ad
un terminale esterno.

USART1 e USART6 è configurata come segue:
	- BaudRate: 115200 baud  
	- Word Length: 8 Bits (7 data bit + 1 parity bit)
	- One Stop Bit
	- Even parity
    - Transmit only / Receive only
    - Break Detect Length: 10 bits

@note L'istanza USARTx / UARTx utilizzata e le risorse associate possono essere aggiornate nel file 
	  "main.h" in base alla configurazione hardware utilizzata.

@note Quando la parity è abilitata, la parity calcolata viene inserita nella posizione MSB dei dati trasmessi.

@note Prestare attenzione quando si utilizza HAL_Delay(), questa funzione fornisce un ritardo accurato 
	  (in millisecondi) basato sulla variabile incrementata in SysTick ISR. Ciò implica che se HAL_Delay() viene 
	  chiamato da un processo ISR periferico, l'interrupt SysTick deve avere una priorità più alta (numericamente 
	  inferiore) dell'interrupt periferico. In caso contrario, il processo ISR del chiamante verrà bloccato. Per 
	  modificare la priorità di interrupt SysTick è necessario utilizzare la funzione HAL_NVIC_SetPriority().
	  

@par Directory contents 

  - Inc/stm32f4xx_hal_conf.h    HAL configuration file
  - Inc/stm32f4xx_it.h          IT interrupt handlers header file
  - Inc/main.h                  Main program header file  
  - Src/stm32f4xx_it.c          IT interrupt handlers
  - Src/main.c                  Main program
  - Src/stm32f4xx_hal_msp.c     HAL MSP module
  - Src/system_stm32f4xx.c      STM32F4xx system clock configuration file
  

@par Hardware and Software environment 
   
  - Questo esempio è stato testato con scheda STMicroelectronics STM32F446 Nucleo-64
	
  - Set-up
	- Collegare il pin PA9 (USART1_Tx) con PC7 (USART6_Rx)  
  
@par Come usarlo ?

Per far funzionare il programma, è necessario:
 - Apri la tua toolchain preferita
 - Rebuild tutti i file e carica l'immagine nella memoria di destinazione
 - Esegui l'esempio

