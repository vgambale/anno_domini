Anno Domini
===========

Anno domini è un gioco di carte: consiste nel collocare le carte dei giocatori in una linea temporale a seconda dell'evento.

Contenuto
=========
336 carte: sul fronte è descritto un evento, e sul retro l’anno in cui è accaduto.

Regole
======
* Ogni giocatore inizia con 7 carte in mano.
* Il gioco termina quando uno dei giocatori rimane senza carte in mano e si considera quindi il vincitore
* L'obiettivo del gioco è di posizionare in ordine cronologico le carte di eventi partendo da una carta di riferimento iniziale posta sul tavolo da gioco.
* Ogni turno di un giocatore è composto da due fasi: la prima fase è opzionale e permette di dubitare della sequenza di carte sul tavolo, secondoa invece è obbligatoria e costringe il giocate a posizionare una delle carte della sua mano sulla linea temporale del tavolo. 
* Nel caso in cui un giocatore dubita della sequenza ed ha ragione il giocatore del turno precedente prende 3 carte nella sua mano pescandole dal mazzo, altrimenti il giocatore che ha dubitato (erroneamente) prende 2 carte dal mazzo.
* Dopo un evento di dubbio vengono rivelate le date sulle carte presenti sul tavolo. Il tavolo viene quindi ripulito ed inizializzato con una nuova carta di riferimento. Il giocatore che ha dubitato correttamente inizia il turno, altrimenti inizia il giocatore successivo.
* Se un giocatore nel proprio turno ha una sola carta in mano e anche sul tavolo da gioco è presente solo una carta di riferimento, allora deve pescare una carta dal mazzo e posizionare le due le carte in mano sul tavolo; il giocatore nel turno successivo è obbligato a dubitare della sequenza (altrimenti il giocatore che è rimasto senza carte vince banalmente la partita).

Funzionamento generale del sistema:
1) Registrazione presso un server centrale della partita di un giocatore_0
2) Registrazione degli altri giocatore_1...giocatore_n (con n fissato all'inizio dal giocatore_0) e quindi inizio partita una volta raggiunto n.
3) La sequenza di arrivo dei giocatori determina i turni di gioco.
4) giocatore_0 inizia la partita mettendo sul tavolo l'evento iniziale (evento_0) preso dalla pila del mazzo e completa il suo turno buttando sul tavolo da gioco una carta della sua mano.

Architettura del sistema
-Server centrale di registrazione (scritto in python)
-I giocatori sono composti dal una parte di server (python) e da una client (linguaggi browser)
-

Messaggi scambiati:
-getGames: richiesta delle partite disponibili sul server (il server è in grado di gestire la creazione di più 				   partite)
-createGame: crea una nuova partita
-joinGame: messaggio di partecipazione ad una partita
-startGame: il server di registrazione invia un messaggio di inizio partita a tutti i partecipanti ad un game
			(msg broadcast)
			il giocatore che ha creato la partita e riceve una startGame crea un mazzo con la sua logica python.
			Questo mazzo viene inviato a tutti gli altri giocatori
-sendCards: fatta solo dal creator
-placeFirstCard: piazza la prima carte del gioco sul banco, operazione che viene effettuata da tutti i giocatori
				 della partita
-playCard: metodo per giocare una carta dalla mano di un giocatore, tutti i giocatori che ricevono la 						   playCard ricevono anche la lista di carte nel banco aggiornate
-sendToken: invia il token al giocatore del turno dopo
-sendDoubt: se il dubbio è fondato il giocatore precedente pesca 3 carte, altrimenti il giocatore che ha dubitato prende 2 carte. Quando il dubbio è fondato il giocatore che ha dubitato inizia il turno facendo playFirstCard e playCard, altrimenti queste due operazioni vengono fatte dal giocatore successivo.
-zeroCards: questo messaggio viene inviato dopo la playCard se il giocatore ha giocato la sua ultima carta. Il token viene passato in automatico. (Il token in questo caso può essere automaticamente settato a true in quanto zeroCards è msg in broadcast)