---

---
---
# Allgemeines
## Seminarziele Allgemein
- Intensive Auseinandersetzung mit ausgewählten Themen
- Einblick in relevante Literatur und wissenschaftliche Texte
- Aufarbeitung und Präsentation eines Themas trainieren
- Beschäftigung mit theoretischer Informatik (Techniken und Beweise)

## Was macht eine gute Seminarleistung aus?

- ## Das Ziel des Vortrages ist es, das Thema so verständlich wie möglich zu vermitteln

- ## Dazu gehört folgendes:

- Einführung mit Motivation der Fragestellung
- Definition und Erklärung der relevanten Begriffe, illustrierende Beispiele
- Darstellung der zentralen Algorithmen, Beispiele wenn möglich (nicht zu komplex)
  
- Begründung der Korrektheit des Algorithmus, z.B. durch
	- vollständigen Beweis
	- sorgfältige Darstellung der Beweisidee
	- Kompromiss zwischen beidem

- Verständlichkeit und gutes Erklären ist das Hauptkriterium für die Benotung
	- Richtet Euch dabei vom Niveau her an die anderen Teilnehmer

- Die Darstellung in der Quelle ist nicht in jedem Fall die ideale. Es wird z.B. häufig nützlich sein, eigene Beispiele zu finden
	- Die sollten dann gut gewählt sein und nur so kompliziert wie nötig (Occam’s Razor)

- Es ist vollkommen akzeptabel, Teile des Materials wegzulassen, wenn der Vortrag sonst zu voll würde.
- So einfach wie möglich, so kompliziert wie nötig.
  
- ### Muss ich Beweise vorstellen?
- Es bietet sich an, 1-2 grundlegende Beweise oder zumindest deren Ideen und Strategien vorzustellen, weil nur so die Techniken verständlich werden

---

# Pagerank
## Thema:
- Definition und Herleitung der Google-Matrix 
- Zusammenhang zu Markovketten und linearer Algebra
- Effiziente Algorithmen zur Berechnung des Pagerank
- Pagerank hängt eng zusammen mit wichtigen Themen der theo. Informatik: 
	- Random Walks auf Graphen
	- stationäre Wahrscheinlkeiten
	- Markovketten

## Inhalt Kapitel 14 - The Mathematics of Google's PageRank
- 4.1 THE ORIGINAL SUMMATION FORMULA FOR PAGERANK
	- Eine Seite ist wichtig, wenn andere wichtige Seiten auf sie verweisen.
		- Übersetzt als mathematische Gleichung bedeutet das, dass der PageRank-Wichtigkeitswert eigentlich die stationären Werte einer riesigen Markov-Kette sind und folglich durch die Markov-Theorie viele interessante Eigenschaften des PageRank-Modells erklärt werden können.
	- PageRank kommt von bibliometrics research (die Analyse der Zitationsstruktur unter wisschenschaftlichen Arbeiten)
		- Pi = PageRank
		- r(Pi) = Summe der PageRanks aller Seiten, die auf Pi verweisen.
		- B Pi = die Menge der Seiten, die auf Pi verweisen (Backlinks)
		- |Pj| = Anzahl der Outlinks
		- ![[Pasted image 20231113134135.png]]
		- Das Problem bei dieser Gleichung ist, dass die r(Pj) Werte, die PR der Seiten, die auf die Seite Pi verlinken, nicht bekannt sind.
			- Erklären was das bedeutet!
		- Zur Problemlösung wurde iteratives Verfahren von Brin und Page eingeführt. Also alle Seiten zu beginn gleichen Rank (1/Seiten im Google-Index des Web's)
	- Überleitung von Formel 4.1.1 zu 4.1.2 erklären und dann die Iterations am Graphen durchgehen um Verständnis aufzubauen
		- Nun wird die Regel in Gleichung (4.1.1) befolgt, um r(Pi ) für jede Seite Pi im Index zu berechnen. Die Regel in Gleichung (4.1.1) wird nacheinander angewandt, wobei die Werte der vorherigen Iteration in r(Pj ) eingesetzt werden.
		- ![[Pasted image 20231113143027.png]]
		- ![[Pasted image 20231113143445.png]]
- 4.2 MATRIXDARSTELLUNG DER SUMMENGLEICHUNGEN
	- Aufzeigen, warum Matrizen besser zur Berechnung sind (Nicht jede Seite einzeln berechnet werden muss).
	- Um das Summenzeichen los zu werden, wird der PageRank mit Hilfe von Matrizen errechnet.
	- Dafür wird bei jeder Iteration eines PageRank-Vektor berechnet, der einen einzigen 1×n-Vektor verwendet, um die PageRank-Werte für alle Seiten im Index zu speichern. 
	- Zu diesem Zweck führen wir eine n×n-Matrix H und einen 1×n-Zeilenvektor πT ein. 
		- Die Matrix H ist eine zeilennormierte Hyperlink-Matrix mit Hij = 1/|Pi |, wenn es einen Link von Knoten i zu Knoten j gibt, und ansonsten 0.
		- Obwohl H die gleiche Nicht-Null-Struktur hat wie die binäre Adjazenzmatrix für den Graphen, sind ihre Nicht-Null-Elemente Wahrscheinlichkeiten.
	- ![[Pasted image 20231113144119.png]]
	- 
	- Überleitung zu 4.2.1 herstellen
		- Die Nicht-Null-Elemente der Zeile i entsprechen den verlinkenden Seiten der Seite i, während die Nicht-Null-Elemente der Spalte i den verlinkenden Seiten der Seite i entsprechen. Wir führen nun einen Zeilenvektor π(k)T ein, der den PageRank-Vektor in der Iteration kth darstellt. Unter Verwendung dieser Matrixnotation kann Gleichung(4.1.2) kompakt geschrieben werden als: 
		- Iterative Matrixgleichung (4.2.1)![[Pasted image 20231113144509.png]]
	- Zeigen das 4.1 und 4.2.1 das gleiche Ergebnis hat
	- 4 unmittelbare Beobachtungen aus 4.2.1:
		1. Jede Iteration von Gleichung (4.2.1) beinhaltet eine Vektor-Matrix-Multiplikation, die im Allgemeinen O(n^2 )-Rechnungen erfordert, wobei n die Größe der quadratischen Matrix H ist.
		2. H ist eine sehr spärliche Matrix (ein großer Teil ihrer Elemente ist 0), da die meisten Webseiten nur auf eine Handvoll anderer Seiten verweisen. Spärliche Matrizen, wie die in Abbildung 4.2 gezeigte, sind aus mehreren Gründen willkommen. Erstens benötigen sie nur minimalen Speicherplatz, da spärliche Speichersysteme, die nur die Nicht-Null-Elemente der Matrix und deren Lage [145], existieren. Zweitens erfordert die Vektor-Matrix-Multiplikation mit einer spärlichen Matrix viel weniger Aufwand als die O(n^2) dichte Berechnung. In der Tat erfordert sie O(nnz(H)), wobei nnz(H) die Anzahl der Nicht-Nullen in H ist. Schätzungen zeigen, dass die durchschnittliche Webseite etwa 10 Outlinks hat, was bedeutet, dass H etwa 10n Nonzeros hat, im Gegensatz zu den n2 Nonzeros in einer vollständig dichten Matrix. Dies bedeutet, dass die Vektor-Matrix-Multiplikation von Gleichung (4.2.1) auf O(n) Aufwand reduziert wird.
		3. Der iterative Prozess der Gleichung (4.2.1) ist ein einfacher linearer stationärer Prozess, wie er in den meisten Vorlesungen zur numerischen Analyse untersucht wird [82, 127]. Tatsächlich handelt es sich um die klassische Potenzmethode, die auf H angewendet wird
		4. H sieht einer stochastischen bergangswahrscheinlichkeitsmatrix für eine Markov-Kette sehr ähnlich. Die "Dangling Nodes" des Netzes, d. h. die Knoten ohne "Outlinks", bilden 0 Zeilen in der Matrix. Alle anderen Zeilen, die den nichtverknüpften Knoten entsprechen, bilden stochastische Zeilen. Daher wird H als substochastisch bezeichnet.
- 4.3 PROBLEME MIT DEM ITERATIVEN PROZESS
	- Welche Probleme entstehen dadurch? (Offene) Fragen stellen?
		- Wird sich dieser iterative Prozess endlos fortsetzen oder wird er konvergieren?
		- Wird sie zu etwas konvergieren, das im Zusammenhang mit dem PageRank-Problem sinnvoll ist?
		- Wenn es irgendwann konvergiert, wie lange ist "irgendwann"? Das heißt, wie viele    Iterationen können wir bis zur Konvergenz erwarten?
		- Unter welchen Umständen oder Eigenschaften von H ist die Konvergenz garantiert?
		- Welche Fehler haben Brin und Page anfangs gemacht und wie behoben?
			- Brin und Page begannen den Iterationsprozess ursprünglich mit ![[Pasted image 20231113154605.png]]
			- e^T = Zeilenvektor aller 1en
				- Dadurch ergaben sich mehrere Probleme, wenn sie Gleichung (4.2.1) mit diesem Ausgangsvektor verwendeten.:
					- Zum Beispiel gibt es das Problem der "Rank Sinks", also der Seiten, die bei jeder Iteration mehr und mehr PageRank anhäufen, die Punkte monopolisieren und sich weigern zu teilen. (Diese Verschwörung kann böswillig oder unabsichtlich sein. z.B.: Suchmaschinenoptimierung oder  Linkfarmen)
						- Hier Knoten 3 Rank Sinks ![[Pasted image 20231113154827.png]]
					- Ein weiteres Problem sind Zyklen
						- ![[Pasted image 20231113155033.png]]
						- 1 verweist nur auf Seite 2 und umgekehrt, wodurch eine Endlosschleife oder ein Zyklus entsteht.
						- Konvergiert also nie!
					- Negative Vektoren auch ein Problem
						- Wieso eigentlich?
- 4.4 EIN WENIG MARKOV-KETTEN-THEORIE
	- Zur Lösung der Probleme brauchen wir Markov-Theorie
	- In den Beobachtungen 3 und 4 haben wir festgestellt, dass Gleichung (4.2.1) der Potenzmethode ähnelt, die auf eine Markov-Kette mit der Übergangswahrscheinlichkeitsmatrix H angewendet wird. 
		- Diese Beobachtungen sind sehr hilfreich, denn die Theorie der Markov-Ketten ist gut entwickelt,2 und sehr gut auf das PageRank-Problem anwendbar.
	- Damit können wir Änderungen vornehmen, die zur Konvergenz führen
		- Insbesondere wissen wir, dass die Potenzmethode, die auf eine Markov-Matrix P angewandt wird, für jeden beliebigen Startvektor zu einem einzigen positiven Vektor konvergiert, der als stationärer Vektor bezeichnet wird, solange P stochastisch, irreduzibel und aperiodisch ist.
		- Daher können die PageRank-Konvergenzprobleme, die durch Senken und Zyklen verursacht werden, überwunden werden, wenn H leicht modifiziert wird, so dass es eine Markov-Matrix mit diesen gewünschten Eigenschaften ist.
- 4.5 FRÜHE ANPASSUNGEN DES GRUNDMODELLS
	- Tatsächlich haben Brin und Page genau das getan. Sie beschreiben ihre Anpassungen am grundlegenden PageRank-Modell in ihren Originalarbeiten von 1998. Interessant ist, dass in keinem ihrer Papiere der Begriff "Markov-Kette" vorkommt, nicht ein einziges Mal. Auch wenn sie sich dessen 1998 nicht bewusst waren, wissen sie jetzt sicherlich, dass ihr ursprüngliches Modell mit Markov-Ketten in Verbindung steht, da Markov-Ketten-Forscher begeistert und stetig auf den PageRank-Zug aufgesprungen sind und eifrig an dem arbeiten, was manche die große Anwendung von Markov-Ketten nennen.
	- Random Surfer
		- Stellen Sie sich einen Websurfer vor, der wahllos der Hyperlinkstruktur des Webs folgt. Das heißt, wenn er auf eine Seite mit mehreren Links stößt, wählt er einen zufällig aus, verlinkt zu dieser neuen Seite und setzt diesen Prozess der zufälligen Entscheidung unbegrenzt fort. Langfristig ist der Anteil der Zeit, die der zufällige Surfer auf einer bestimmten Seite verbringt, ein Maß für die relative Bedeutung dieser Seite. Wenn er einen großen Teil seiner Zeit auf einer bestimmten Seite verbringt, dann muss er beim zufälligen Verfolgen der Hyperlink-Struktur des Webs immer wieder auf diese Seite zurückgekehrt sein. Seiten, die er oft besucht, müssen wichtig sein, denn auf sie müssen andere wichtige Seiten verweisen.
		- Leider stößt dieser zufällige Surfer auf einige Probleme. Er bleibt immer dann hängen, wenn er einen hängenden Knotenpunkt betritt
			- Und im Web gibt es jede Menge solcher Knoten, z. B. PDF-Dateien, Bilddateien, Datentabellen usw.
		- Zur Lösung dieses Problems war die erste Anpassung die Stochastizitätsanpassung
			- heißt so, weil die 0^T Zeilen von H durch 1/n e^T ersetzt werden, wodurch H stochastisch wird. Infolgedessen kann der zufällige Surfer, nachdem er einen Dangling-Knoten eingegeben hat, nun einen Hyperlink zu einer beliebigen Seite setzen.
				- Diese stochastische Matrix nennt sich S
				- ![[Pasted image 20231113163139.png]]
				- Die mathematische Formulierung dieses Stochastik-Fixes zeigt, dass S aus einer Rang-1-Aktualisierung von H entsteht, d. h. S = H + a(1/n e^T ), wobei ai = 1 ist, wenn die Seite i ein "Dangling Node" ist, und sonst 0.
				- Der binäre Vektor a wird als "Dangling Node"-Vektor bezeichnet. S ist eine Kombination aus der rohen ursprünglichen Hyperlink-Matrix H und einer Rang-Eins-Matrix 1/n ae^T.
				- Diese Anpassung garantiert, dass S stochastisch ist und somit die Übergangswahrscheinlichkeitsmatrix für eine Markov-Kette darstellt.
		- Allerdings löst allerdings noch nicht das Konvergenzproblem (Das ein eindeutiges positives Ergebnis π^T existiert und die Gleichung 4.2.1 schnell dahin konvergiert)
			- Deswegen wurde außerdem die Primitivitätsanpassung vorgenommen. 
				- Eine primitive Matrix ist sowohl irreduzibel als auch aperiodisch. Der stationäre Vektor der Kette (in diesem Fall der PageRank-Vektor) existiert also,  ist eindeutig und kann durch eine einfache Potenziteration gefunden werden.
				- Brin und Page verwenden erneut den Random Surfer, um diese Markov-Eigenschaften zu beschreiben.
					- Das Argument der Zufallssurfer für die Primitivitätsanpassung lautet folgendermaßen: 
						- Es stimmt zwar, dass Surfer der Hyperlink-Struktur des Webs folgen, aber manchmal wird ihnen langweilig und sie verlassen die Hyperlink-Methode des Surfens, indem sie ein neues Ziel in die URL-Zeile des Browsers eingeben. 
					- Um diesen Vorgang mathematisch zu modellieren, erfanden Brin und Page eine neue Matrix G (Google-Matrix), die wie folgt lautet:
						- ![[Pasted image 20231113171056.png]]
							- α ist ein Skalar zwischen 0 und 1
		- Die Primitivitätsanpassung hat mehrere Konsequenzen:
			- G ist stochastisch. Es ist die konvexe Kombination der beiden stochastischen Matrizen S und E = 1/n ee^T .
				- Was bedeutet das?
			- G ist irreduzibel. Jede Seite ist direkt mit jeder anderen Seite verbunden, so dass die Irreduzibilität trivialerweise erzwungen ist.
			- G ist aperiodisch. Die Selbstschleifen (G (unten ü) > 0 für alle i) erzeugen Aperiodizität.
				- Erklären
			- G ist primitiv, weil G^k > 0 für einige k. (Tatsächlich gilt dies für k = 1.) Dies impliziert, dass ein eindeutiges positives πT existiert, und die auf G angewandte Potenzmethode konvergiert garantiert zu diesem Vektor.
			- G ist komplett dicht, was aus rechnerischer Sicht sehr schlecht ist. Glücklicherweise kann G als eine Rang-Eins-Aktualisierung der sehr spärlichen Hyperlink-Matrix H geschrieben werden.
			- G ist insofern künstlich, als die rohe Hyperlink-Matrix H zweimal verändert wurde, um die gewünschten Konvergenzeigenschaften zu erreichen. Einen stationären Vektor (also einen PageRank-Vektor) gibt es für H nicht, also haben Brin und Page kreativ gemogelt, um ihr gewünschtes Ergebnis zu erreichen. Für das zweifach modifizierte G gibt es einen eindeutigen PageRank-Vektor, und wie sich herausstellt, ist dieser Vektor bemerkenswert gut darin, Webseiten einen globalen Bedeutungswert zu geben.
	- Begriffsübersicht:
		- ![[Pasted image 20231113171708.png]]
		- Noch übersetzen!
	- ![[Pasted image 20231113171738.png]]
- 4.6BERECHNUNG DES PAGERANK-VEKTORS
	- Mit Eigenvektore-Methode oder linearen homogenen Systemen
		- Matlab’s eig command to solve for πT , then normalized the result (by dividing the vector by its sum) to get the PageRank vector. However, for a web-sized matrix like Google’s, this will not do. Other more advanced and computationally efficient methods must be used. Of course, πT is the stationary vector of a Markov chain with transition matrix G
		- However, the specific features of the PageRank matrix G make one numerical method, the power method, the clear favorite
			- The power method is one of the oldest and simplest iterative methods for finding the dominant eigenvalue and eigenvector of a matrix. Therefore, it can be used to find the stationary vector of a Markov chain. (The stationary vector is simply the dominant left-hand eigenvector of the Markov matrix.)
			- However, the power method is known for its tortoise-like speed. Of the available iterative methods (Gauss-Seidel, Jacobi, restarted GMRES, BICGSTAB, etc.), the power method is generally the slowest. So why did Brin and Page choose a method known for its sluggishness?
				- First, the power method is simple and therefore easy to implement and also is storage-friendly:
					- The vector-matrix multiplications (π(k)T H) are executed on the extremely sparse H, and S and G are never formed or stored, only their rank-one components, a and e, are needed. Recall that each vector-matrix multiplication is O(n) since H has about 10 nonzeros per row. This is probably the main reason for Brin and Page’s use of the power method in 1998
			- But why is the Power method still the predominant method in PageRank research papers today, and why have most improvements been novel modifications to the PageRank power method, rather than experiments with other methods?
				- The power method, like many other iterative methods, is matrix-free, which is a term that refers to the storage and handling of the coefficient matrix
					- No manipulation of the matrix is done
				- Modifying and storing elements of the Google matrix is not feasible. Even though H is very sparse, its enormous size and lack of structure preclude the use of direct methods.
				- The last reason for using the power method to compute the PageRank vector con- cerns the number of iterations it requires. Brin and Page reported in their 1998 papers, and others have confirmed, that only 50-100 power iterations are needed before the iterates have converged, giving a satisfactory approximation to the exact PageRank vector.
			- The next logical question is: why does the power method applied to G require only about 50 iterations to converge?
				- The answer comes from the theory of Markov chains. In general, the asymptotic rate of convergence of the power method
	- ASIDE: Search Engine Optimization
	- ASIDE: How Do Search Engines Make Money?
- 4.7 THEOREM AND PROOF FOR SPECTRUM OF THE GOOGLE MATRIX
	- Verstehen und Erklären können, vielleicht nicht im Detail, aber die Grundidee bzw. Strategie

## Vortrag Gliederung (Erste Ideen)
1. Einstieg mit dem Quote von 1. Seite Kapitel 14
2. Grober Überblick welches Problem PageRank löst und warum die Erfinder es brauchten. Anschließend noch wofür andere PageRank Modell nutzen, maybe auch mit anderer Verwendung als Google es hat.
3. Erklärung PageRank erst in Worten, an Beispiel mit 4 Bekannten Websites und einem erklärenden Graphen dazu. Simpler Einstieg
4. Von Ausgangsformel des Pageranks aufbauend anfangen und dann bis heutigen Pagerank und zwischendurch die mathematischen Erklärungen, wie Markov Chain, einstreuen.