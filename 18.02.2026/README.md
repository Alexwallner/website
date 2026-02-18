# Arbeitschritte 18.02.2026

## Laravel Projekt ausetzten

Heute wurde der Grundstein gelegt, ich habe mich für das Projekt Inventarapp mit dem QR-Code entschieden und nach dieser Entscheidung habe ich das Projekt mit Laravel new aufgestzt. Bei dem Aufsetzten wurde sich für Blade entschieden und PEST. 

## Github 

Nach dem das Projekt aufgesetzt wurde, war der nächste Schritt die Einladung in den Classroom anehmen und das Projekt hochzuladen


## Laravel funktionstüchtig bereitstellen

Die Schritte npm install, npm run build, composer install, composer require laravel/breeze, php artisan breeze:install, php artisan install:api und npm run dev. Durch diese Schritte funktioniert jetzt das log in und alle blade datein wurden erstellt.

## Planung für logik des Projektes

In diesem Schritt habe ich einen logischen Aufbau für das Projekt erstellt. Welche Models, Controller und Tabellen ich brauche und in welcher Verbindung diese stehen müssen. Bei dem Controller wurde noch geplant welche Funktionen nötig sind

## Models

Nach der Plannung wurden die Models **Room, Brand, Device und Category** erstellt. Diese wurden im jeweiligen Model noch mit einer Function mit Has Many oder Belongs to versehen. 

## Tables 

Zu den Models wurden nun auch die jeweiligen Tabels erstellt. Im fokus lag die Tabelle **Devices** da diese die Fremdschlüsseln der anderen 3 Tabellen besitzt. 

```php
  public function up(): void
    {
        Schema::create('devices', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->string('barcode')->nullable()->unique();
            $table->foreignIdFor(\App\Models\Brand::class)->nullable()->constrained()->cascadeOnDelete();
            $table->foreignIdFor(\App\Models\Category::class)->constrained()->cascadeOnDelete();
            $table->foreignIdFor(\App\Models\Room::class)->constrained()->cascadeOnDelete();
            $table->timestamps();
        });
    }
```

Hier kann man sehen wie die Tabelle Devices aufgebaut ist.
In den weitern Tabellen wurden nur kleine Äderungen vorgenommen wie der Name und bei **Room** das Stockwerk