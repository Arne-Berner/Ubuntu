### Hotke  
alt+cmd+f4-6 wechselt zwischen virtuellen termina  

### ls   
erste Spalte Zugriffsrechte + ist es eine regular file oder directory   
zweite Spalte wie viele links (symbolllinks "shortcuts" und hardlinks)  
dritte Spalte: wer hat die Spalte erstellt  
fünfte Spalte: Wie groß ist die File  
sechste Spalte: Timestamps -> in linux gibt es immer 3(4) timestamps (last modified "Wann wurde etwas geändert", last status change "Was ist an der Filepermission verändert wurden", last acessed "wann wurde es aufgerufen", birth time "Wann wurde es erstellt")  

### . und   
. ist current directory  
.. ist parent directoy  
~ ist $HOME 
cd ohne etwas ist meistens auch $HOME  

### tee, touch und tail/cut  
tee zeigt schickt etwas an das Terminal zum einsehen, aber auch in die angegebene Datei  

### cmd parameter und pipe operator |
cmd 1>success.txt schickt den output an success, wenn es funktioniert hat (wegen der 1), man kann auch cmd >success.txt stattdessen schreiben  
cmd 2>/dev/Null schickt den output in den "Trash", ohne recovery, wenn die Aktion nicht funktioniert hat  
cmd 1>success.txt 2>failed.txt kann man auch so kombinieren.  
cmd 1>>success.txt macht einen append anstatt zu ersetzen.  
cmd 1>f1 2>&1 schickt beides zu success. &1 ist also der stream für success. Um beides zu f1 zu schicken kann man auch cmd &>f1 machen  
für die meisten cmds ist der standart output pts(zahl in der man ist)  
der | operator sendet den output stream eines Programms als input an das Programm hinter |  
nutzt man cat f1 | cat und der Inhalt von f1 ist f2, dann wird f2 auf dem Bildschirm ausgegeben. Benutzt man aber cat f1 | xargs cat dann wird der Inhalt von f2 ausgegeben  
cat \`cat f1\` bedeutet, dass cat (inhalt von f1) ausgeführt wird. 
cat ```$(cat f1)``` bedeutet, dass gleiche wie cat \`cat f1\`  
Mit ; kann man commands hintereinander ausführen. Also sowas wie update ; upgrade  
Mit & kann man commands gleichzeitig ausführen. also sowas wie xclock & xlock  
Mit && wird der zweite command ausgeführt, wenn der erste succeeded. also dateee && xclock executed xclock nicht  
Mit || wird das zweite command ausgeführt, wenn das erste failed. also dateee || xclock wird nur xclock ausführen  
Man kann sich && als und vorstellen und || als or vorstellen  
cat f3 | more oder cat f3 | less lässt einen oben in einer file anfangen und search benutzen. Head und tail -n4 lässt einen die letzten 4 zeilen sehen.  

### kill
kill -n PID (n ist die art des tötens, 15 ist aufs x drücken und 9 ist schlimm)
killall name (hiermit kann man nach namen töten, alle mit diesem namen werden getötet)
pkill name (hier wird jeder prozess getötet, welcher name beinhaltet)

### commands on the flY (cat and cut and sort)
cat gibt aus, was man in der datei hat  
cut kann genutzt werden, um die Spalten anzuzeigen. Also cat /etc/passwd | cut -f2,3 (die zweite und dritte spalte) -d: (durch : getrennt)  
cat /etc/passwd| head-15 | sort -k4 (column4 ) -t: (durch : getrennt) -n (nach zahlen)
cat filename | uniq gibt nur einzigeartige Wörter aus mit -d gibt duplicates an  
cat f5 | tr  ia XY (i wird durch X und a durch Y ersetzt)
cat /dev/urandom | tr -dc \!0-9a-zA-Z | head -c 10 erstellt ein random passwort mit 10 zeichen. 
paste names.txt lastnames.txt -d" " pasted die zeilen der ersten datei in die zeilen der zweiten datei mit dem delimiter " " und gibt sie auf dem stream aus. (sie werden nicht wirklich in die datei kopiert)
join names.txt lastnames.txt (joined die zeilen, aber löscht die Wörter raus, die gedoppelt sind)

### grep & sed
grep nimmt einen input und wendet darauf eine regular expression drauf an.(um ein wort zu suchen)  
mit -e kann man in grep mehrere expressions hintereinander anwenden  
-F ist ohne ^. usw. zu ersetzen  
-c zählt die vorkommenden wörter
-v nimmt die inverse
sed nimmt einen stream und baut es im output stream quasi noch um. Es ist sehr mächtig? und funktioniert ähnlich wie vim. Man kann z.b. die 4 spalte mit sed '4d' löschen. 's/:/Canberk/3' ersetzt jedes dritte : durch Canberk. Nutzt man g anstatt von 3, wird jedes : ersetzt. nutzt man 3g wird ab dem dritten vorkommen jedes : ersetzt.
mit 's/.*/\U&/' ändert man alle lines zu uppercase. \U ist uppercase und & ist alles was geändert werden soll. Also in dem beispiel alles was in .\* fällt.  
mit '3,8s/nologin/Arne/' wird nur in der 3. bis 8. Zeile login mit arne ersetzt.
mit '3,8!s/nologin/Arne/' werden alle login ausser der 3. bis 8. zeile mit arne ersetzt
mit -E kann man sowas wie .{5} benutzen, um 5 beliebige buchstaben einzusetzen.
```bash set -n '$=' ``` = gibt die linenumber aus. Also gibt die expression die letzte linenumber aus und somit die anzahl der lines.


### Rechnen
expr 4+5 berechnet 4+5  
aber auch $[4+5] oder $((4+5))funktioniert auch  
bc ist der basic calculator. Damit kann man auch schnell arbeiten.

### Kopieren und ausschneiden
mit cp kann man files oder directories kopieren.
Will man ein directory kopieren muss man jedoch -r benutzen.  
-i sorgt dafür, dass files nicht einfach überschrieben werden, sondern vorher nachgefragt wird.  
-v zeigt an, was er gerade macht. Also falls man sehr viel kopiert, sieht man, ob es abgestürzt ist.  
Mit mv kann man dinge ausschneiden. Bei mv braucht man nicht -r angeben.  
-p preserve permissions (ls in user bin nach idw ordner kopiert) bleiben die user permissions gleich  
-f überschreibt einfach bei copy andere Dateien mit dem gleichen Namen  
man kann auch /usr/bin/ls (also das ls command aus usr/bin) unter einem anderen namen in ein anderes Directory kopieren.  
mit which ls kann man herausfinden, wo ls wirklich liegt

### Variablen
PATH = /bla/bla:\$PATH setzt den ordner vor path  
PATH = \$PATH:/bla/bla setzt den ordner hinter path (jeweils immer in den Path odner)  
printenv gibt alle Umgebungsvariablen raus
export macht eine lokale Variable zu einer Umgebungsvariable  
export -n entfernt die Variable wieder aus den Umgebungsvariablen, sie bleibt aber als lokale Variable bestehen.  
lokale Variablen löscht man mit unset (löscht auch aus Umgebungsvariablen)  
mit bash kann man einen "Kindprozess" starten, dort werden nur die Umgebungsvariablen übernommen  
shellset gibt dir lokale und umgebungsvariablen  
\$PS1 gibt den wert aus, der vor $ steht  
\\w ist current directory  

expr length "\$a" gibt die länge eines strings an
```bash
for i in *
do
a=${i#*, }  // der string i wird ab dem ersten , ausgegeben und in a gespeichert  
b=${i%%,*} // der string wird bis zum letzten , ausgegeben und in b gespeichert  
c=${i%,*} // der string wird bis zum ersten , ausgegeben und in c gespeichert  
d=${i##,*} // der string wird ab dem letzten , ausgegeben und in d gespeichert
```

### Permissions
ls -la gibt sowas aus  
drwxr-x-rw- bedeutet, dass es ein directory ist, r steht für read (lesen, inhalt ansehen), write steht für schreibrechte (files createn zum Beispiel im Directory), x steht für execute, also in das directory reingehen oder dateien ausführen.  
chmod 753 directoryname gibt mir die rechte, wie sie oben beschrieben sind.  
aber auch chmod u=rwx g=rx o=rw (u ist user, g ist group - Ausser der user selbst!, o ist other)  
Wenn ich eine file executable machen will, kann ich chmod +x filename machen. Soll sie nichtmehr executable sein kann ich -x nutzen. Das geht für alle rechte. Soll nur der user x bekommen ist es u+x.  
das stickybit gibt an, dass nur die owner löschen können. es sieht so aus: drwxrwxrwt (es ist das t). Es kann über chmod +t filename gesetzt werden.
das SUID Set user Identification setzt drwsr-xr-x. Damit kann der user, der nur dieses Programm executen darf, die files auf die dieses programm zugreift mit dem owner recht der datei ausführen.  


### regex
. bedeuted ein charakter
.* bedeutet alle charakter
[0-9]* bedeutet es ist eine zahlenfolge mit beliebig vielen zahlen

### time variables
\\@ ist am/pm 
\\t ist 24 stunden
\\T ist 12 stunden
nur in der \$PS1 variable


### terminal emulation und virtual termin  
mit strg alt f1-9 kann man virtual terminals aufrufen. Im Ersten oder zweiten dieser virtual Terminals ist meistens die GUI. Macht man einen Terminal in der GUI auf, ist es eine terminal emulation. In der Funktionalität unterscheiden sie sich nicht und sie können miteinander kommunizieren. (schließlich sind sie nur die shell zum kerne  

### copy pas  
ctrl + insert (copy)  
shift + insert (paste)  
oder  
auswählen (automatisch copy)  
mittlere Maustaste (paste)  

### Text bearbeiten
[a,b,c] ersetzt einen Charakter durch einen dieser charaktere (xor)  
[^a] bedeutet dass der charakter nicht ovrkommt  
* Joker für mehrere Characters (mehrere oder null)  
? Joker für einen Character   
touch {a,b,c}.txt ist das gleiche wie touch a.txt b.txt c.txt  
{1..15} ist eine range von 1 bis 15  
{1..15..2} is eine range von 1 bis 15 mit zweier Schritten  

### tail nutzen um logs zu lesen
tail -f filename

### count words
cat f3 | wc -l (lines) -w(words) -c(characterbytes) -m(character count)

### Mounting
Mounting kann man sich vorstellen, als würde man den usb stick (zum Beispiel) an ein Verzeichnis andocken. Man kann nicht direkt auf dne USB zugreifen, ohne dass er an ein Verzeichnis geheftet ist. (Wie ein Raumschiff, was an der Raumstation andocken muss.). Zu beachten ist dabei, dass die einzelnen devices sda, sdb, sdc (werden alphabetisch hochgezählt) nicht angeschlossen werden, sondern die logischen devices sda1 sda2 usw. (also partitionen auf der festplatte z.B.)

### packages installieren etc.
apt-cache search peter (sucht in deinem cache nach den möglichen packages)
apt-file search peter (sucht peter im remote repository)

### vim
keine schreibrechte? :w !sudo tee %

### Automate Tasks
mit at now + 3 days kann ich sagen, dass etwas in 3 tagen ausgeführt werden soll.  
in at muss ich dann angeben, wo ich das hinsenden möchte. Also z.b. ```bash echo "hi!">/dev/pts/4 ``` um es auf die konsole zu printen.
atq zeigt die prozesse und atrm kann die nummer bei atq nutzen, um den prozess zu löschen. at ist also für Zeitlich abgegrenzte Prozesse  
batch hingegen wartet, bis das System nichtmehr "busy" ist.  
in cron oder crontab kann man automatisierte tasks haben, die sich immer wiederholen. in der spool file steht unten sowas wie m h dom mon usw.  
Dort kann man 5 * * * etc eingeben, für die jeweiligen felder (also ausserhalb des Kommentars). crontab guru ist da eine gute quelle.   

### Komprimieren und verpacken
eine Tar (ein Archive) zu erstellen, macht eine Datei erstmal größer. Man kann dort einfach mehrere Directories reinpacken. So könnte man Dateien leicht schicken.  
gzip komprimiert die tar file dann, sodass sie meistens kleiner ist, als die Files für sich.  
gzip -v filename.tar zeigt die komprimierung durch v mit an  
gunzip nutzt man, um sie wieder zu öffnen.
tar cf filename erstellt eine tar file
tar xf filename extrahiert eine tar file
tar tf filename zeigt den content an
tar rf filename fügt eine datei der tar hinzu

### Shellscript
\#! ist shebang und gibt den interpreter an. Also z.b. das etwas mit bash übersetzt werden soll.


### mögliche Fehler
Wenn man etwas mit | sed verändert in einer file und es zurück an die file schickt, wird der Inhalt gelöscht, weil man etwas an eine "busy file" schickt. Es ist da besser sed -i zu benutzen und am ende die file als bestimmungsort anzugeben. Am besten nutzt man -iBACKUPNAME ,damit das original auch noch im original gespeichert wird.

### fragen  
was ist \$(cat file.txt)$ als Beispiel gewese  
Können die einzelnen Terminals miteinander kommunzieren, weil sie das über den kernel tun? oder bauen sie untereinander eine verbindung au  


Wie kann man direkt von der Ausgabe dinge nehmen? Gibt es da vim aktionen?  
Wie könnte ich die Episoden sortieren?


### Script Ideen
Dateien mit Datum erstellen. 
```bash
$(date +%Y%m)
``` 
zum beispiel im namen.