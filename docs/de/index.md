---
layout: index
title: Postgres.app – der schnellste Weg zu PostgreSQL am Mac
---

<header>
	<hgroup>
	  	<h1 itemprop="name">Postgres.app</h1>
	  	<h2 itemprop="description">Der schnellste Weg zu PostgreSQL am Mac</h2>
		<ul class="buttons">
			<li><a href="{{ site.downloadLocation }}">Download</a></li>
			<li><a href="documentation/">Dokumentation</a></li>
			<li>
				<a href="{{ site.github.repository_url }}">Github <span class="note">{{ site.github.public_repositories[0].stargazers_count }} stars</span></a>
			</li>
		</ul>
	</hgroup>
</header>


Postgres.app enthält eine vollständige Installation von PostgreSQL die keine Wünsche offen lässt.
Im Paket sind außerdem populäre Erweiterungen wie zB [PostGIS](http://postgis.net) für Geo-Daten und [plv8](https://github.com/plv8/plv8) für Javascript enthalten.


Postgres.app hat ein innovatives user interface und ist über ein eigenes Statusmenü steuerbar.
Die umständliche und komplizierte Steuerung über das Terminal entfällt somit,
für fortgeschrittene User_innen wurden jedoch sämtliche [Command Line Tools](/de/documentation/cli-tools.html) und Header Dateien inkludiert.

Postgres.app ist mit einer auto-update Funktion ausgestattet, somit erhältst du sofort alle Updates und Bugfixes.

Die aktuelle Version erfordert macOS 10.10 oder neuer und wird mit den PostgreSQL Versionen 9.5 und 9.6 ausgeliefert. Ältere Versionen können [hier](/de/documentation/older-versions.html) heruntergeladen werden.



Installation
-----------------------

<ul class="instructions">
	<li>
		<p>Download &nbsp; ➜ &nbsp; In Applications Ordner bewegen &nbsp; ➜ &nbsp; Doppelklick</p>
		<p class="subdued">Postgres.app kann nur geöffnet werden, wenn du sie zuvor in den Applications Ordner bewegt hast. (Sonst wird folgende Warnmeldung angezeigt: "unidentified developer")</p>
	</li>
	<li>
		<p>Klicke auf "Initialize" um einen neuen Server zu erstellen</p>
	</li>
	<li>
		<p>Optional: Um die Command Line Tools nutzen zu können, musst du deinen <tt>$PATH</tt> konfigurieren:</p>
		<pre>echo /Applications/Postgres.app/Contents/Versions/latest/bin | sudo tee /etc/paths.d/postgresapp</pre>
	</li>
</ul>

Fertig! Auf deinem Mac läuft nun ein PostgreSQL Server mit diesen Einstellungen:

<table class="settings">
	<tr>
		<td>Host</td>
		<td>localhost</td>
	</tr>
	<tr>
		<td>Port</td>
		<td>5432</td>
	</tr>
	<tr>
		<td>User</td>
		<td class="light">dein System User Name</td>
	</tr>
	<tr>
		<td>Database</td>
		<td class="light">gleich wie User</td>
	</tr>
	<tr>
		<td>Passwort</td>
		<td class="light">keines</td>
	</tr>
	<tr>
		<td>Connection URL</td>
		<td>postgresql://localhost</td>
	</tr>
</table>

Um eine Verbindung zu einer Datenbank herzustellen, doppelklicke auf das gewünschte Datenbank-Symbol.
Wenn du dich mittels psql direkt aus dem terminal verbinden möchtest, gib `psql` ein.
Eine Liste mit graphischen Datenbank-Clients findest du im folgenden Abschnitt. 

WICHTIG: Diese Anleitung setzt voraus, dass du PostgreSQL zuvor noch nicht auf deinem Mac installiert hast.
Folge [dieser Anleitung](/de/documentation/install.html#upgrading), wenn du PostgreSQL schon einmal installiert hast (zB via Postgres.app, homebrew, oder EnterpriseDB),


Graphische Clients
-----------------

Neben dem Terminal-Befehl `psql` gibt es noch eine Reihe graphischer Client-Apps, mit denen du dich mit deiner PostgreSQL Datenbank verbinden kannst.
Zwei populäre Apps sind:

<ul class="clients">
	<li id="pgadmin"><a href="https://www.pgadmin.org">pgAdmin 4</a></li>
	<li id="postico"><a href="https://eggerapps.at/postico/">Postico</a></li>
</ul>

[pgAdmin 4](https://www.pgadmin.org) ist ein open source PostgreSQL cross-plattform client, welcher nahezu alle Features von PostgreSQL unterstützt.
Der einzige Nachteil ist das etwas altmodische UI, welches nicht die Erwartungen einer modernen und nativen Mac App erfüllt.

[Postico](https://eggerapps.at/postico/) ist eine sehr moderne und innovative Mac App.
Es wurde von den selben Personen entwickelt, die auch Postgres.app betreuen.
Wir haben bei der Entwicklung von Postico sehr viel Zeit investiert um dir bei der Verwendung von PostgreSQL viel Freude zu bereiten.
Dennoch kann Postico leider nicht den vollen Funktionsumfang von pgAdmin bieten und wird im Gegensatz dazu als kommerzielle Version vertrieben.

Abgesehen von diesen beiden Optionen findest du in der Dokumentation [eine Liste mit zahlreichen anderen Mac Apps für PostgreSQL](/de/documentation/gui-tools.html).


Verbindung zum Server
--------------

Nachdem dein PostgreSQL Server aufgesetzt wurde möchtest du vielleicht eine Verbindung von deiner Applikation aus herstellen.
Die folgende Liste enthält einige Code-Beispiele der populärsten Programmiersprachen und Frameworks:

<dl class="connect-info">
	<dt class="active" onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">PHP</dt>
	<dd>
			<p>
				Stelle zunächst sicher, dass deine PHP-Version PostgreSQL unterstützt.
				Da die in macOS integrierte PHP-Version PostgreSQL nicht unterstützt, empfehlen wir <a href="https://www.mamp.info">MAMP</a>.
				Dieses kostenlose Programmpaket enthält neben den aktuellen PHP-Version auch viele Erweiterungen und ist sehr einfach zu installieren.
			</p>
			<p>
				Verbindung mittels PDO (objektorientiert):
			</p>
			<pre>&lt;?php
$db = new PDO('pgsql:host=localhost');
$statement = $db->prepare("SELECT datname FROM pg_database");
$statement->execute();
while ($row = $statement->fetch()) {
    echo "&lt;p>" . htmlspecialchars($row["datname"]) . "&lt;/p>\n";
}
?></pre>
			<p>
				Verbindung mittels <tt>pg_connect()</tt> (prozedural):
			</p>
			<pre>&lt;?php
$conn = pg_connect("postgresql://localhost");
$result = pg_query($conn, "SELECT datname FROM pg_database");
while ($row = pg_fetch_row($result)) {
    echo "&lt;p>" . htmlspecialchars($row[0]) . "&lt;/p>\n";
}
?></pre>
	</dd>
	
	<dt onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">Python</dt>
	<dd>
		<p>
			Um via Python mit einem PostgreSQL verbinden zu können muss zunächst die psycopg2 library installiert werden:
		</p>
		<pre>
pip install psycopg2
		</pre>
		<h3>Django</h3>
		<p>Füge einen Eintrag zu DATABASES in settings.py hinzu:</p>
		<pre>
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql_psycopg2",
        "NAME": "[YOUR_DATABASE_NAME]",
        "USER": "[YOUR_USER_NAME]",
        "PASSWORD": "",
        "HOST": "localhost",
        "PORT": "",
    }
}
		</pre>
		<h3>Flask</h3>
		<p>Wenn du die <a href="https://packages.python.org/Flask-SQLAlchemy/">Flask-SQLAlchemy</a> Extension verwendest kannst du die Library folgendermaßen einbinden:</p>
		<pre>
from flask import Flask
from flask.ext.sqlalchemy import SQLAlchemy
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'postgresql://localhost/[YOUR_DATABASE_NAME]'
db = SQLAlchemy(app)
		</pre>
		<h3>SQLAlchemy</h3>
		<pre>
from sqlalchemy import create_engine
engine = create_engine('postgresql://localhost/[YOUR_DATABASE_NAME]')
		</pre>
	</dd>
	
	<dt onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">Ruby</dt>
	<dd>
		<p>Bevor du das pg gem installierst musst du sicherstellen, dass dein <tt>$PATH</tt> korrekt gesetzt wurde (siehe <a href="documentation/cli-tools.html">Command-Line Tools</a>). Führe im Anschluss folgenden Befehl aus:</p>
		<pre>sudo ARCHFLAGS="-arch x86_64" gem install pg</pre>
		
		<h3>Rails</h3>
		<p>Füge folgende Parameter in config/database.yml ein:</p>
		<pre>
development:
    adapter: postgresql
    database: [YOUR_DATABASE_NAME]
    host: localhost
		</pre>
		<h3>Sinatra</h3>
		<p>Füge folgende Parameter in config.ru oder deinen Programmcode ein:</p>
		<pre>
set :database, "postgres://localhost/[YOUR_DATABASE_NAME]"
		</pre>
		<h3>ActiveRecord</h3>
		<p>Installiere das activerecord gem, binde 'active_record' ein und stelle eine Verbindung zum PostgreSQL Server her:</p>
		<pre>
ActiveRecord::Base.establish_connection("postgres://localhost/[YOUR_DATABASE_NAME]")
		</pre>
		<h3>DataMapper</h3>
		<p>Installiere und binde die datamapper und do_postgres gems ein und stelle eine Verbindung zum PostgreSQL Server her:</p>
		<pre>
DataMapper.setup(:default, "postgres://localhost/[YOUR_DATABASE_NAME]")
		</pre>
		<h3>Sequel</h3>
		<p>Installiere und binde das sequel gem ein und stelle eine Verbindung zum PostgreSQL Server her:</p>
		<pre>
DB = Sequel.connect("postgres://localhost/[YOUR_DATABASE_NAME]")
		</pre>
	</dd>
	
	<dt onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">Java</dt>
	<dd>
		<ol>
			<li>
				Lade den <a href="https://jdbc.postgresql.org/download.html">PostgreSQL JDBC driver</a> herunter und installiere diesen.
			</li>
			<li>
				Verbinde zur JDBC URL mittels <tt>jdbc:postgresql://localhost</tt>
			</li>
		</ol>
		<p>Mehr Informationen findest du in der offiziellen <a href="https://jdbc.postgresql.org/documentation/head/index.html">PostgreSQL JDBC Dokumentation</a>.</p>
	</dd>
	
	<dt onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">C</dt>
	<dd>
		<p>
			libpq ist die native C client library für PostgreSQL und ist sehr einfach zu verwenden:
		</p>
		<pre>#include &lt;libpq-fe.h>
int main() {
    PGconn *conn = PQconnectdb("postgresql://localhost");
    if (PQstatus(conn) == CONNECTION_OK) {
        PGresult *result = PQexec(conn, "SELECT datname FROM pg_database");
        for (int i = 0; i &lt; PQntuples(result); i++) {
            char *value = PQgetvalue(result, i, 0);
            if (value) printf("%s\n", value);
        }
        PQclear(result);
    }
    PQfinish(conn);
}</pre>
		<p>Kompiliere das file mit clang und führe es aus:</p>
		<pre>clang main.c -I$(pg_config --includedir) -L$(pg_config --libdir) -lpq
./a.out</pre>
	</dd>
	
	<dt onclick="this.parentElement.getElementsByClassName('active')[0].className='';this.className='active';">Swift</dt>
	<dd>
		<p>
			Swift erlaubt dir einen einfachen Zugriff auf das C API. Zunächst musst du libpq im bridging header einbinden:
		</p>
		<pre>#import &lt;libpq-fe.h></pre>
		<p>
			Stelle sicher dass du dein Projekt mit libpq linkst.
		</p>
		<p>
			In iOS musst du libpq selbst bauen.
		</p>
		<p>
			In macOS kannst du entweder die libpq des Systems (kein SSL Support) oder direkt die libpq von Postgres.app verwenden.
			Füge folgende build settings hinzu:
		</p>
		<table>
			<tr>
				<th>Other Linker Flags</th>
				<td><tt>-lpq</tt></td>
			</tr>
			<tr>
				<th>Header Search Paths</th>
				<td><tt>/Applications/Postgres.app/Contents/Versions/latest/include</tt></td>
			</tr>
			<tr>
				<th>Library Search Paths</th>
				<td><tt>/Applications/Postgres.app/Contents/Versions/latest/lib</tt></td>
			</tr>
		</table>
		<p>Jetztz kannst du auf die <a href="https://www.postgresql.org/docs/current/static/libpq.html">libpq C library</a> zugreifen:</p>
		<pre>let conn = PQconnectdb("postgresql://localhost".cString(using: .utf8))
if PQstatus(conn) == CONNECTION_OK {
    let result = PQexec(conn, "SELECT datname FROM pg_database WHERE datallowconn")
    for i in 0 ..&lt; PQntuples(result) {
        guard let value = PQgetvalue(result, i, 0) else { continue }
        let dbname = String(cString: value)
        print(dbname)
    }
    PQclear(result)
}
PQfinish(conn)</pre>
	</dd>
</dl>



Support
-------

Eine Liste mit häufigen Errors und Problemen findest du in der Dokumentation unter [troubleshooting](/de/documentation/troubleshooting.html).

Für allgemeine Fragen zu PostgreSQL solltest du einen Blick in die [offizielle PostgreSQL Dokumentation](https://www.postgresql.org/docs/current/static/) werfen.

Solltest du eine Frage zu Postgres.app haben, die nicht in der [Postgres.app Dokumentation](/de/documentation/) beantwortet ist,
kannst du entweder auf Twitter unter [@PostgresApp](https://twitter.com/PostgresApp) eine Frage stellen,
oder auf Github ein [neues Issue](https://github.com/postgresapp/postgresapp/issues) eröffnen.

Wenn du uns Bugs übermitteln möchtest: Gib immer die Version von Postgres.app und macOS an und stelle sicher dass dein Bug Report IMMER eine detaillierte Fehlerbeschreibung enthält!


Lizenz
-------

Postgres.app, PostgreSQL und alle verwendeten Extensions sind unter der [PostgreSQL License](http://www.postgresql.org/about/licence/) veröffentlicht. 
Die veröffentlichten binaries enthalten auch noch OpenSSL ([OpenSSL Lizenz](https://www.openssl.org/source/license.html)), PostGIS ([GPLv2](http://opensource.org/licenses/gpl-2.0)), und plv8 ([3 clause BSD](http://opensource.org/licenses/BSD-3-Clause)).

Postgres.app wird derzeit von [Jakob Egger](https://github.com/jakob) und [Chris Pastl](https://github.com/chrispysoft) weiterentwickelt und betreut.
Postgres.app wurde ursprünglich von [Mattt Thompson](https://github.com/mattt) entwickelt.