Anno Domini

Regole:
-336 carte con 2 attributi: evento storico e data
-7 carte per giocatore
-Vince il giocatore che rimane senza carte
-L'obiettivo del gioco è di posizionare in ordine cronologico le carte di eventi partendo da una carta di riferimento iniziale posta sul tavolo da gioco.
-Ad ogni turno il giocatore può dubitare della correttezza della sequenza delle carte sul tavolo, se ha ragione il giocatore precedente prende 3 carte nella sua mano pescandole dal mazzo, altrimenti il giocatore che ha dubitato (erroneamente) prende 2 carte dal mazzo
-A fronte del dubbio di un giocatore e del controllo di correttezza della sequenza, dato che vengono rivelate le date sulle carte, il tavolo da gioco viene ripulito e viene posta una nuova carta di riferimento.Se il giocatore ha dubitato bene inizia lui,altrimenti inizia il giocatore successivo
-Se un giocatore nel proprio turno ha una sola carta in mano e sul tavolo da gioco è presente solo una carta di riferimento allora deve pescare una carta dal mazzo e posizionare tutte e due le carte in mano sul tavolo; il giocatore nel turno successivo è obbligato a dubitare della sequenza (altrimenti il giocatore che è rimasto senza carte vince banalmente la partita)

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