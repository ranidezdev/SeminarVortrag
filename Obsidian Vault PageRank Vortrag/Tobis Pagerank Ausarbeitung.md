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
	- Das PageRank-Problem kann auf zwei Arten erklärt werden:
		- 1.  Lösen Sie das folgende Eigenvektorproblem für π^T .
			- ![[Pasted image 20231115195118.png]]
			- Im ersten System besteht das Ziel darin, den normalisierten dominanten linken Eigenvektor von G zu finden, der dem dominanten Eigenwert λ1 = 1 entspricht.
		- 2. Lösen Sie das folgende lineare homogene System für π^T .
			- ![[Pasted image 20231115195203.png]]
			- m zweiten System besteht das Ziel darin, den normalisierten linken Nullvektor von I - G zu finden.
	- Für beide Systeme gilt die Normalisierungsgleichung πT e = 1, die sicherstellt, dass πT ein Wahrscheinlichkeitsvektor ist
	- An sich es dadurch zu berechnen, dass man den Vektor durch seine Summe dividiert, aber für riesig große Webmatrizen nicht ausreichend.
		- Dafür müssen fortschrittlichere und rechenintensivere Methoden verwendet werden
	- Natürlich ist π^T der stationäre Vektor einer Markov-Kette mit der Übergangsmatrix G, und es wurde viel über die Berechnung des stationären Vektors für eine allgemeine Markov-Kette geforscht.
	- Dies machte eine numerische Methode, genauer die Potenz-Methode, zur besseren Wahl und wurde deshalb für die Google Matrix zum klaren Favoriten
	- Wieso ist die Potenzmethode in diesem Fall besser fragt ihr euch?
		- Ihr habt mir das vermutlich alle direkt geglaubt, aber die Potenzmethode ist eigentlich für ihre Langsamkeit beim finden von stationären Vektoren im Vergleich zu Gauß-Seidel, Jacobi, etc. bekannt.
		- Also was hat Brin und Page damals dazu gebracht, trotzdem diese Methode zu wählen?
			- Erstens die Einfachheit der Methode. Die Implementierung und Programmierung ist sehr unkompliziert.
			- Doch ein weitaus größerer Vorteil für ihr Vorhaben ist, dass sich die Matrix G durch die angewandte Potenzmethode (Gleichung (4.5.1)) tatsächlich in Form des sehr dünn besetzten H ausgedrückt werden.
			  ![[Pasted image 20231115202114.png]]
			-  Die Vektor-Matrix-Multiplikationen (π^((k)T) H) werden mit dem extrem dünn besetzten H durchgeführt, und S und G werden nie gebildet oder gespeichert, sondern nur ihre Rang- Eins-Komponenten, a und e, werden benötigt.
			- Es sei daran erinnert, dass jede Vektor- Matrix-Multiplikation O(n) ist, da H etwa 10 Nullen pro Zeile hat. Dies ist wahrscheinlich der Hauptgrund dafür, dass Brin und Page im Jahr 1998 die Potenzmethode verwendeten.
		- Aber warum ist die Power-Methode auch heute noch die vorherrschende Methode in PageRank-Forschungsarbeiten, und warum waren die meisten Verbesserungen neuartige Modifikationen der PageRank-Power-Methode und nicht Experimente mit anderen Methoden?
			- Weitere Vorteile des Algorithmus:
				- Die Potenzmethode ist matrixfrei
					- wie viele andere iterative Methoden, ist sie matrixfrei, ein Begriff, der sich auf die Speicherung und Handhabung der Koeffizientenmatrix bezieht. Bei matrixfreien Methoden wird auf die Koeffizientenmatrix nur über die Vektor-Matrix- Multiplikationsroutine zugegriffen. Es findet keine Manipulation der Matrix statt. 
					- Dies steht im Gegensatz zu direkten Methoden, bei denen Elemente der Matrix bei jedem Schritt manipuliert werden. 
					- Ändern und Speichern von Elementen der Google Matrix ist nicht machbar.
					- Auch wenn H sehr spärlich ist, schließen seine enorme Größe und sein Mangel an Struktur die Verwendung direkter Methoden aus. Stattdessen werden matrixfreie Methoden, wie die Klasse der iterativen Methoden, bevorzugt.
				- Die Potenzmethode ist auch speicherfreundlich
					- Neben der dünnbesetzten Matrix H und dem Knotenvektor a muss nur ein Vektor, der aktuelle Iterus π^((k)T) , gespeichert werden. Dieser Vektor ist vollständig dicht, d. h. es müssen n reelle Zahlen gespeichert werden
					- Bei Google ist n = 8,1 Milliarden, so dass man die sparsame Mentalität des Unternehmens verstehen kann, wenn es um die Speicherung geht
					- Andere iterative Methoden, wie GMRES oder BICGSTAB, sind zwar schneller, erfordern aber die Speicherung mehrerer Vektoren. Ein neu gestartetes GMRES(10) erfordert beispielsweise bei jeder Iteration die Speicherung von 10 Vektoren der Länge n, was dem Speicherbedarf der gesamten H-Matrix entspricht, da nnz(H) ≈ 10n
				- Der letzte Grund für die Verwendung der Power-Methode zur Berechnung des PageRank-Vektors und den finde ich persönlich am interessantesten, ist die Anzahl der erforderlichen Iterationen. 
					- Brin und Page berichteten in ihren Veröffentlichungen von 1998, und andere haben dies bestätigt, dass nur 50-100 Power-Iterationen erforderlich sind, bevor die Iterate konvergieren und eine zufriedenstellende Annäherung an den exakten PageRank-Vektor liefern.
					- Es sei daran erinnert, dass jede Iteration der Potenzmethode O(n) Aufwand erfordert, weil H so spärlich ist. 
					- Daher ist es schwer, eine Methode zu finden, die 50 O(n)-Power-Iterationen übertreffen kann. Algorithmen, deren Laufzeit und Rechenaufwand linear (oder sublinear) mit der Problemgröße sind, sind sehr schnell und selten.
			- Aber warum benötigt die auf G angewandte Potenzmethode nur etwa 50 Iterationen, um zu konvergieren?
				-  Die Antwort ergibt sich aus der Theorie der Markov-Ketten. Im Allgemeinen hängt die asymptotische Konvergenzrate der auf eine Matrix angewandten Potenzmethode vom Verhältnis der beiden größten Eigenwerte ab, die mit λ1 und λ2 bezeichnet werden. 
				- Genauer gesagt ist die asymptotische Konvergenzrate die Rate, bei der  ![[Pasted image 20231115203400.png]] Für stochastische Matrizen wie G ist λ1 = 1, so dass |λ2 | die Konvergenz bestimmt.
				- Da G ebenfalls primitiv ist, ist ![[Pasted image 20231115203434.png]] 
				- Im Allgemeinen erfordert die numerische Bestimmung von λ2 für eine Matrix einen Rechenaufwand, den man nicht aufwenden möchte, nur um eine Schätzung der asymptotischen Konvergenzrate zu erhalten. Glücklicherweise ist es für das PageRank-Problem einfach zu zeigen, dass, wenn die jeweiligen Spektren ![[Pasted image 20231115203534.png]] gilt ![[Pasted image 20231115203551.png]]
				- Außerdem ist es aufgrund der Linkstruktur des Webs sehr wahrscheinlich, dass ![[Pasted image 20231115203611.png]] oder zumindest ungefähr 1 was bedeutet, dass ![[Pasted image 20231115203635.png]] ist.
				- Folglich erklärt der konvexe Kombinationsparameter α die berichtete Konvergenz nach nur 50 Iterationen. In ihren Papieren verwenden die Google-Gründer Brin und Page α = .85, und beim letzten Bericht ist dies immer noch der von Google verwendete Wert. α50 = .8550 ≈ .000296, was bedeutet, dass man bei der 50. Iteration eine Genauigkeit von etwa 2-3 Stellen im ungefähren PageRank-Vektor erwarten kann.
	- Wir können nun also positive Festhalten, dass:
		- mit den Anpassungen der Stochastizität und der Primitivität, die auf G angewandte Potenzmethode garantiert zu einem einzigen positiven Vektor  konvergiert, dem PageRank-Vektor, unabhängig vom Startvektor.
		- Der resultierende PageRank-Vektor positiv ist, und es dadurch keine unerwünschten Gleichstände bei 0 mehr gibt. 
		- Außerdem müssen um PageRank-Werte mit einer Genauigkeit von ungefähr τ Stellen zu erzeugen, nur -τ /log10 α Iterationen  abgeschlossen werden.
		- ==Das sollten wir wahrscheinlich mit Bildern in der PowerPoint erklären. Also die Unterschiede die entstehen würden, wenn man das mit anderen Mehtoden versuchen würde==
		- ==Wenn Zeit über, dann über "Wie verdienen Suchmachinen ihr Geld" oder Suchmaschinen-Optimierung reden==



- 4.7 THEOREM AND PROOF FOR SPECTRUM OF THE GOOGLE MATRIX
	- Verstehen und Erklären können, vielleicht nicht im Detail, aber die Grundidee bzw. Strategie
	- ==Oder wir zeigen das nur so wie "hey, falls du daran interessiert bist, hier der Beweis."==
	- ![[Pasted image 20231115204609.png]]
 

## Vortrag Gliederung (Erste Ideen)
==4.5 und 4.6 sollten wir wahrscheinlich am meisten Zeit widmen==
1. Einstieg mit dem Quote von 1. Seite Kapitel 14
	1. ==Wir könnten auch Versuchen den Vortrag anhand der 6 Fragen aus 4.3 zu gestalten um die Leute mit den Fragen vielleicht bisschen am Ball zu halten==
2. Grober Überblick welches Problem PageRank löst und warum die Erfinder es brauchten. Anschließend noch wofür andere PageRank Modell nutzen, maybe auch mit anderer Verwendung als Google es hat.
3. Erklärung PageRank erst in Worten, an Beispiel mit 4 Bekannten Websites und einem erklärenden Graphen dazu. Simpler Einstieg
4. Von Ausgangsformel des Pageranks aufbauend anfangen und dann bis heutigen Pagerank und zwischendurch die mathematischen Erklärungen, wie Markov Chain, einstreuen.