# DataRecorder

## Definitionen
- **Rohdate**: So, wie ein Signal am Sensor Eingang empfangen wird (**mA** oder **V**).
- **Messdate**: Daten mit physikalischen Einheiten (z.B. °C)

## Einleitung
Das Ziel dieses Projekts ist es einen Messdaten-Recorder mit einer Strommessung im 4-20 **mA** Bereich zu entwickeln. Die Rohdatenerfassung  soll nach der Projektspezifikation über 24 **h** lang mit einem (konfigurierbarem) Erfassungsintervall von 1 **s** bis mehreren **min** erfolgen. Als Erfassungshardware wird der Yocto-4-20mA-Rx [9] von der Schweizer Firma Yoctopuc [8] verwendet. Die Rohdaten werden über eine konfigurierbare lineare Funktion in physikalische Messwerte umgerechnet. Die Messwerte sollen neben der Darstellung im graphischen User Interface (GUI) auch auf der Harddisk als CSV File abgespeichert werden. Im GUI werden auch die aktuellen Roh- und Messwerte (und Min/Max/Einheit) nummerisch dargestellt.

Im zweiten Schritt soll im Anschluss an die Arbeitsprobe das Projekt so erweitert werden, dass aufgezeichnete Messwertdaten die als CSV Datei gelesen werden und über ein Yoctopuc[8] Emulator Modul [10] mit einem Strombereich von 4-20 **mA** ausgegeben werden. Der Verlauf der physikalischen Ausgabewerte soll im selben GUI, auf einem anderen Tab dargestellt werden.

Das Programm soll eine konfigurierbare Anzahl Messsensoren als Empfänger unterstützen und später - bei der Realisierung des Emulators - die Transmittermodule. Als Konfigurationsdatei wird ein XML Format verwendet, in dem die zu verwendenden Sensoren und Transmitter persistent abgespeichert werden.

## Architektur (grob skizziert, noch zu verbessern)
Das 'main.py' ist der Eintrittspunkt zum Programm. Es lädt die persistente Konfiguration aus dem 'configuration.xml'. Darin ist die letze Verwendung des Programms abgelegt (Datenrate, Folder für CVS Datenfile, Erfassungsdauer) und wird daher auch bei Programm Ende mit den aktuellen Settings gespeichert. Mit den konfigurierten Sensoren werden die Sensoren im 'sensor.py' instanziiert. Der Zugriff auf die Sensoren wird in dieser Klasse gekapselt. Dann von 'main.py' das Graphische User Interface aufgesetzt ('gui.py') und im Konstruktor die Konfiguration und die Sensoren übergeben.

Als Spielfeld zum Kennenlernen der Sensoren und der Herstellerbibliotheke wurde ein Stand Alone script `producer.py` das hardcodiert in einer definierten Datenrate ein bestimmte Anzahl (24x60= 1440 = 1 Messpunkt pro Minute) Messwerte erfasst und in ein CVS File gespeichert.
 
![Model View Controller Aufbau](./mvc.png)

## Graphic User Interface (GUI)
Das Graphic User Interface (GUI) ist in der Datei `gui.py` implementiert. Als Bibliotheke wird PyQt [11] verwendet. Diese Biblioteke stellt die Funktionalität zur Verfügung um ein graphisches Fenster plattformunabnhänig zu implementieren. Das hat `DataRecorder` steht in der Titelbar. Es folgt eine Menubar mit den Einträgen `File` und `About` .

Im folgenden werden als Tabs sowohl ein Recorder als auch eine Emulatorgrafik (Optional) dargestellt. Im Erfassungstab wird die aktuelle Erfassungszeit und die aktuelle Erfassungsrate dargestellt. Wird dieser Wert geändert, wird daraus wieder ein `configuration.xml` erstellt und abgespeichert. In einer Legende können die erfassenden Sensoren selektiert werden, die dann in °C in der Grafik des Tabs dargestellt werden.  Die Grafik ist in einem Sliding Fenster dargestellt um den interessierten Bereich der Daten darzustellen.

## Konfiguration
![Xml Konfiguration](./xmlConfig.png)
## Referenzen

## 
1. „GitHub Flavored Markdown Spec“. https://github.github.com/gfm/#example-14 (zugegriffen 15. Januar 2023).
2. J. M. Willman, Beginning PyQt: A Hands-on Approach to GUI Programming with PyQt6, 2nd ed. Apress, 2022.
3. B. Okken, „Python Testing with Pytest: Simple, Rapid, Effective, and Scalable: Simple, Rapid, Effective, and Scalable : Okken, Brian: Amazon.de: Bücher“, 2022. 
4. E. Gamma, R. Helm, R. E. Johnson, und J. Vlissides, Design Patterns. Elements of Reusable Object-Oriented Software., 1st ed., Reprint Edition. Reading, Mass: Prentice Hall, 1997.
5. M. Fitzpatrick, „Create GUI Applications with Python & Qt5 (5th Edition, PyQt5): The hands-on guide to making apps with Python : Fitzpatrick, Dr Martin.
6. D. Beazley, Python Essential Reference, 4. Aufl. Upper Saddle River, NJ: Addison-Wesley Professional, 2009.
7. D. Bader, Python-Tricks: Praktische Tipps für Fortgeschrittene, 1. Aufl. Heidelberg: dpunkt.verlag GmbH, 2018.
8. [yoctopuc](https://www.yoctopuce.com/)
9. [Yocto-4-20mA-Rx](https://www.yoctopuce.com/EN/products/usb-electrical-interfaces/yocto-4-20ma-rx)
10. [Yocto-4-20mA-Tx](https://www.yoctopuce.com/EN/products/usb-electrical-interfaces/yocto-4-20ma-tx)
11. [PyQt](https://www.qt.io/)
12. [1] M. Summerfield, Rapid GUI Programming with Python and Qt: The Definitive Guide to PyQt Programming, 1. Aufl. Pearson, 2007.

Source library can be downloaded  from <https://www.yoctopuce.com/FR/downloads/YoctoLib.python.52382.zip> and unzip in a folder above this folder and rename it to yoctolib_python.
