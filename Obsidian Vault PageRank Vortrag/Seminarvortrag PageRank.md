## Gliederung



## Finaler Inhalt



## Tobi-Notizen

==Habe mit DeepL mal ein Teil des Dokumentes übersetzt und das mit in die Repo getan==
==Schau mal in "Tobis Pagerank Ausarbeitung", wenn du meinen Stand sehen willst==
==Ich mache morgen vermutlich weiter damit den Text in "Vortrag Style"  zu übersetzen und an der Struktur zu arbeiten ==
==Was hältst du von meinen Anmerkungen bei 4.6 und ganz unten bei Vortrag Gliederung (Erste Ideen)?==


## Paul-Notizen

#### Kapitel 4.1: Summenformel des PageRank
$$r(P_i)=\sum_{P_j \in B_{P_i}}\frac{r(P_j)}{|P_j|}$$
- Der PageRank einer Seite $P_i$, auch mit $r(P_i)$ bezeichnet, ist die Summe der PageRanks aller Seiten die auf $P_i$ zeigen 
- Dabei ist $B_{P_i}$ die Menge der Seiten die auf $P_i$ zeigen und $|P_j|$ die Anzahl der der Verlinkungen die von $P_j$ ausgehen

- Problem dieser Formel ist, dass die PageRanks der Seiten die auf $P_i$ zeigen unbekannt sind
- Zur Lösung wurde ein iterativer Ansatz gewählt, bei dem angenommen wird das jede Seite am Anfang den gleichen PageRank besitzt (hier $\frac{1}{n}$, wobei n die Anzahl an Seiten in Googles "dex of the web" ist) und für jede Seite mithilfe der obigen Formel pro Iteration der PageRank bestimmt und der vorherige Wert damit ersetzt wird
$$r_{k+1}(P_i)=\sum_{P_j \in B_{P_i}}\frac{r_k(P_j)}{|P_j|}$$
- Ziel ist dass die Werte der PageRanks gegen einen stabilen Wert nach einer bestimmten Anzahl von Iterationen konvergieren

(Hier eventuell noch Schaubild von Graphen zeigen die Websiten repräsentieren und die Berechnung pro Iteration veranschaulichen)


#### Kapitel 4.2 Matrixdarstellung der Summengleichung

- Beide der im Kapitel 4.1 benutzten Formeln liefern pro Berechnung den PageRank von nur einer Seite, deshalb fokussiert sich dieses Kapitel auf die Vereinfachung dieser Berechnung durch Umschreiben in Form einer Matrix und der Verwendung eines PageRank Vektors

	![[Intelligente Systeme/PageRank/Screenshots/Pasted image 20231113113955.png]]

- Durch die Darstellung in einer $n \times n$ Matrix H entfällt $\sum$, weiterhin enthält der PageRank Vektor, ein $1 \times n$  Zeilenvektor ${\pi}^T$,welcher in jeder Iteration die PageRank Werte aller Seiten der Matrix enthält
- Die Werte der Matrix an der Stelle $H_{ij}$ ergeben sich aus $H_{ij}=1/|P_i|$, wenn es eine Verlinkung (Kante) vom Knoten i zu Knoten j gibt, ansonsten 0
- Die Nicht-Nullwerte, in dieser Matrix Wahrscheinlichkeiten, stehen in
	- der Zeile i für die ausgehenden Verlinkungen der Seite i
	- der Spalte i für die eingehenden Verlinkungen auf Seite i

- Der Zeilenvektor ${\pi}^{(k)T}$ ist der PageRank Vektor in der k-ten Iteration mit welchen sich die zuvor verwendete Formel in Kapitel 4.1 weiter vereinfachen lässt zu:
$${\pi}^{(k+1)T}={\pi}^{(k)T}H$$
- Wichtige Beobachtungen für die Entwicklung des PageRank Modells:
	- Jede Iteration der vorangehenden Gleichung durchläuft eine Vektor-Matrix-Multiplikation, welche in der Regel $O(n^2)$ Berechnungen benötigt (hierbei ist n ist die Größe der Matrix H)
	- H ist eine spärliche Matrix, bzw. hat sehr viele Nullelemente, da die meisten Seiten nur auf eine Handvoll anderer Seiten verlinken. Solche Matrizen haben viele Vorteile:
		- minimale Speicherung notwendig, da meistens nur die Nicht-Nullelemente gespeichert werden müssen
		- Vektor-Matrix-Multiplikation einer spärlichen Matrix braucht deutlich weniger Berechnungen als $O(n^2)$ 
			- Im Detail $O(nnz(H))$ Berechnungen, wo $nnz(H)$ die Anzahl an von Nicht-Nullelementen in H ist
			- Schätzungen zeigen dass jede Seite im Schnitt 10 Verlinkungen hat, d.h. H hat 10n Nicht-Nullwerte im Vergleich zu einer dichten Matrix mit $n^2$ Nicht-Nullwerten
			- Fazit: Vektor-Matrix-Multiplikation hat reduzierten Aufwand von nur $O(n)$ Berechnungen
		- Einfache Gleichung, da es sich im Prinzip nur um eine klassische Potenzfunktion angewandt auf H handelt
		- H sieht nach einer stochastischen Übergangsmatrix für eine Markow-Kette aus. Die hier sogenannten "dangling nodes", also die Knoten ohne ausgehende Verlinkungen, erzeugen 0-Zeile in der Matrix, alle anderen Zeilen die keine "dangling nodes" sind, erzeugen stochastische Zeilen. Deshalb wird H als substochastisch bezeichnet. (substochastisch = Matrizen mit Einträgen zwischen 0 und 1 deren Zeilen-(Spalten)summe kleiner als 1 ist)


#### 4.3 Probleme der iterativen Herangehensweise

- Die in Kapitel 4.2 benutzte Formel zur Berechnung des Zeilenvektors ${\pi}^{(k)T}$ wirft viele Fragen auf, diese werden mit der Herangehensweise von Brin und Page im folgenden versucht zu beantworten

- Ursprünglich wurde für den Anfang der Iteration ${\pi}^{(0)T} = 1/n e^T$ verwendet, wobei $e^T$ der Zeilenvektor aller 1-en ist
- Jedoch kam es hier zu verschiedenen Problemen, wie
	- sogenannten "rank sinks", welche mit jeder Iteration einen höheren PageRank erlangen und quasi nichts "abgeben"
		- Sprich wenn keine ausgehenden Verlinkungen von einem Knoten ausgehen, sondern nur eingehende Verlinkungen wird der PageRank an diesem Knoten immer größer
	- Ein weiteres Problem ist wenn einzelne Knoten besonders viel PageRank ansammeln, dass andere Knoten am Ende einen PageRank von 0 haben
		- Im Idealfall sollte der PageRank-Vektor aber immer positiv sein, also keine Nullwerte
	- Ein weiteres und letztes Problem sind Zyklen, also wenn eine Knoten nur auf einen anderen zeigt und genauso der andere Knoten zurück, dies hat zur Folge dass die Werte niemals konvergieren


#### 4.4 Markow-Ketten

- In den Kapitel 4.2 erwähnten Beobachtungen haben wir festgestellt, dass die Gleichung einer Potenzfunktion ähnelt, die auf eine Markow-Kette mit Übergangsmatrix H angewendet wird
	- sehr hilfreich da sich Theorie der Markow-Ketten auf PageRank-Problem anwenden lässt um wünschenswerte Ergebnisse und die Konvergenzeigenschaften sicherzustellen
- Wir wissen, dass die Potenzfunktion die auf eine Markow-Matrix P angewendet wird, für jeden Startvektor konvergiert zu einem eindeutigen positiven Vektor, der als stationärer Vektor bezeichnet wird
	- vorausgesetzt P ist stochastisch, irreduzibel (nicht zurückführbar) und aperiodisch (Aperiodizität plus Irreduzibilität impliziert Primitivität)
- Daher können die Konvergenzprobleme des PageRank, die von "sinks" und "cycles" verursacht werden, überwunden werden indem man H so abändert dass es eine Markow-Matrix mit den gewünschten Eigenschaften wird
- Markow-Eigenschaften mit Einfluss auf PageRank:
	- ein einziger positiver PageRank-Vektor existiert, wenn die Google-Matrix stochastisch und nicht reduzierbar ist
	- weiterhin gilt durch die Aperiodizität, dass die Potenzmethode konvergieren wird zum PageRank-Vektor, unabhängig vom Start-Vektor


#### 4.5 Frühe Anpassungen am Grundmodell

- Der Titel dieses Kapitels ist maßgebend dafür was Brin und Page in ihrem paper 1998 veröffentlicht haben, ohne den Begriff "Markow-Kette" überhaupt einmal verwendet zu haben
- Anstelle der Markow-Ketten und ihrer Eigenschaften, verwendeten sie den Begriff des "random surfers" um ihre Anpassungen zu beschreiben

 - Angenommen es gäbe einen web-surfer der zufällig der Hyperlink-Struktur des Internets folgt
	- Das heißt, sobald er auf einer Seite ankommt mit mehreren ausgehenden Verlinkungen, entscheidet er sich zufällig für einen, befindet sich nun auf einer neuen Seite und führt diesen Entscheidungsprozess auf unbestimmte Zeit fort 
	- Auf lange Sicht gesehen, steht die Zeit die der Surfer auf einer Seite verbringt unmittelbar im Zusammenhang damit wie wichtig eine diese insgesamt ist
		- Dies folgt daraus, dass der Surfer durch die zufällige Wahl von Hyperlinks sich häufig auf der einen Website, auf der er viel Zeit verbracht hat, immer wieder findet
		- Es gilt, Seiten die oft besucht wurden sind wichtig, da andere wichtige Seiten ausgehende Verlinkungen auf diese haben müssen
	- Allerdings gibt es auch Probleme für den Surfer
		- Er bleibt stecken an bereits erwähnten "Dangling Nodes", welches Verlinkungen zu beispielsweise pdf-Dateien, Bildern oder Tabellen sind, die keine weiteren Verlinkungen aufweisen

- Um dieses Problem zu lösen, wurde eine erste Anpassung vorgenommen, das "stochasticity adjustment", da die $0^T$ Reihen aus H ersetzt werden mit $1/ne^T$, was H stochastisch macht 
	- Resultat dieser Änderung ist, dass der Surfer beim Antreffen einer dangling node, nun zu jeder anderen Seite zufällig gelangen kann
- Für die kleine 6-Node Abbildung aus Kapitel 4.2, ist die stochastische Matrix genannt S:
	 ![[Pasted image 20240103151723.png]]
- Die stochastische Anpassung zeigt mathematisch das S entsteht aus einem "rank-one update" von H
	- Dieses ist: $S = H + a(1/ne^T)$, wobei $a_i=1$  wenn die Seite $i$ eine "dangling node" ist, sonst 0
	- Der Binär-Vektor a heißt "dangling node"-Vektor
	- S ist eine Kombination aus der originalen Hyperlink-Matrix H und einer "rank-one"-Matrix $1/nae^T$ 
- Die Anpassung garantiert, dass S stochastisch ist, genauso wie die Übergangswahrscheinlichkeitsmatrix einer Markow-Kette
	- Jedoch reicht dies nicht alleine um die Konvergenz-Eigenschaft zu garantieren (also dass ein positives $\pi^T$ existiert, zu welchem die Gleichung schnell konvergiert)

- Die zweite Anpassung, das sogenannte "primitivity adjustment", soll dafür sorgen dass die resultierende Matrix sowohl stochastisch als auch primitiv ist
	- eine primitive Matrix ist sowohl nicht reduzierbar, als auch aperiodisch 
	- Für den stationären Vektor der Kette (welcher in diesem Fall der PageRank-Vektor ist), gilt er muss existieren, eindeutig sein und durch eine einfache Potenziteration gefunden werden können
	- Brin und Page verwendeten auch hier den Surfer, um diese Markow-Eigenschaften zu beschreiben