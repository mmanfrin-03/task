Le parti di codice scritte da me sono:
Riga 23...30 --> definizione nomi stati della FSM
Riga 69...96 --> dichiarazione funzione lettura e scrittura seriale
Riga 131...205 --> int main + while 1

Le altre righe di codice sono generate automaticamente dal IOC per avere le seguenti impostazioni:
	-USART2 asincrona con 9600 Bound, 8 bit + stop, nessuna parità
	-Timer3 non usato ma fa un togle ogni 0,5s (sarebbe stato usato per fare il timeout di 2,5s)
	-clock 86MHz

(Il main.c si trova sotto Manfrin task\Core\Src\main.c)

DESCRIZIONE CODICE:
INIT--> inizializzazione programma, definizioni variabili+USART on

CHECK_DEV --> invio il comando 0x22 ad un indirizzo 0x33 (inventato da me), se ricevo in risposta una versione, software e disp aspettati cambio stato (ABC123! Scelto da me come SW+versione+disp)

IDLE --> invio codice 0x20 e aspetto una risposta, se ricevo '1' allora la chiave è ok altrimenti lo continuo a richiedere

READ --> invio codice 0x70 e leggo il credito, il credito da ASCII viene convertito in un numero (ho ipotizzato che la cifra più piccola sia il centesimo) e valuto se è > o < di 10cent. Se è minore passo allo stato successivo altrimenti ritorno in IDLE

DEC --> invio comando prelievo e comando 0x3F, se ricevo un ack è ok altrimenti rinvio i comandi




NOTA: la lettura da seriale è bloccante, per semplicità l'ho tenuta così anche per fare un debug più semplice. Non sono presenti stati ERRORE, per semplicità ho ipotizzato che lo slave rispondesse sempre nei modi voluti.


DEBUG:
Collegando la scheda al Pc e tramite Putty ho simulato la comunicazione seriale (Putty non fornisce  i comandi in HEX ma in ASCII)