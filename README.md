
# **GUIDE** Wie generiert man in Linux eine Shared Library (.so): 

1) Objektdatei erstellen
2) Bibliothek erstellen


## Schritt 1:

Header(h) und Quelldatei(c/cpp) erstellen.

Beispiel: 

Header add.h:
```C++
     int add(int, int);
     int sub(int, int);
     int mul(int, int);
```

Quelldatei, hier im Beispiel als C++ Datei, add.cpp:
```C++
     
	int add(int a, int b) 
	    {
	        return a + b;
	    }
	int sub(int a, int b)
	    {
	        return a - b;
	    }
	int mul(int a, int b)
	    {
        	return a * b;
	    }
```
	    
Quell- und Header Datei werden, mit dem g++, zu einer Objektdatei compiliert.
```bash
 g++ -fPIC -c -Wall add.cpp
```
**-fPIC** muss für shared libraries immer angegben werden!

Über die gcc Option **-c** wird Linker mitgeteilt **NICHT** zu linken. Wird **-c** nicht hinzugefügt kommt es zur Fehlermeldung da es kein main Funktion gibt.

-> Add.o wurde erstellt

## Schritt 2:

Die Library wird so erstellt:
```bash
ld -shared add.o -o libadd.so
```
Über **ld** wird der Linker aufgerufen. Mit der Option **-shared** wird dem Linker mitgeteilt, eine shared Object mit dem Input File add.o zu linken
### Library linken

Der Linker schaut, beim folgenden Befehl, im Verzeichnis */usr/bin/ld* nach der Library.
```bash
g++ -Wall main.cpp -ladd
```
Man kann die Library ins angegebene Verzeichnis hinzufügen oder dem Linker über die Option **-L**  den Pfad mitgeben. 
```Bash
g++ -Wall -L/home/admin/demo -Wl,-rpath=/home/admin/demo/ main.cpp -lpal
```
Mit **-W,-rpath=/home/labadmin/demo/main.cpp** wird der Pfad angegeben,in die die Lib eingebettet wird. Somit kann der Loader sie bei Runtime finden.   

#### Linker Excursion im Bezug auf Libraries

Über Linker Option **-l** können Libs includiert werden:
```bash
-ladd (für libadd.so/.a )
```
Mit **-L** können Lib Verzeichnisse angegeben werden:
```Bash
-L/user../Demo/lib
```
 Über **-I** kann ein Verzeichnis für *include* Files angegeben werden welches durchsucht werden soll:
 ```bash
 -I/user.../lib
```
 
 
 
 
