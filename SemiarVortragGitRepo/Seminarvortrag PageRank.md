## Gliederung



## Finaler Inhalt



## Tobi-Notizen



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

	![[SeminarVortrag/Screenshots/Pasted image 20231113113955.png]]

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
	- Ein weiteres Problem