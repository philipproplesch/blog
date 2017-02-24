---
title: "SQL Server: Ausgesperrt!"
date: 2012-04-12
tags: sql server
---
Die Tage wollte ich eine neue Datenbank in meinem lokalen [SQL Server](http://www.microsoft.com/en-us/sqlserver/default.aspx) anlegen. Eigentlich keine wilde Sache: Management Studio auf, Verbindung mittels **Windows Authentication** herstellen, Datenbank anglegen, ggfs. Rechte vergeben, Management Studio zu. Fertig!

Dieses Mal erhielt ich jedoch beim Versuch die Datenbank anzulegen die Meldung: `CREATE DATABASE permission denied in database 'master'`.

![](/images/sql_no_permissions_1.png)

Grund für dieses Problem war die Tatsache, dass mein Windows-User – warum auch immer – nicht mehr Mitglied der Rolle **sysadmin** war.

Falls das Post-it mit dem Passwort des sa-Users nicht mehr am Monitor klebt, bleibt wahrscheinlich nur noch eine Neuinstallation. 

Alternativ könnte es auch mit den folgenden Schritten funktionieren:

#### Dienst des SQL Server beenden
![](/images/sql_no_permissions_2.png)

#### Instanz über die Konsole starten
![](/images/sql_no_permissions_3.png)

#### In einer weiteren Konsole eine Verbindung zum Server herstellen
![](/images/sql_no_permissions_4.png)

#### Den User zur oben genannten **sysadmin**-Rolle hinzufügen    
![](/images/sql_no_permissions_5.png)

#### Die Stored Procedure mittels **GO** ausführen
![](/images/sql_no_permissions_6.png)

Abschließend die Konsolen schließen, den Dienst wieder starten und die Datenbank wie gewohnt anlegen.
