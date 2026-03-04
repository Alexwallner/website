# 04.03.2026 (4 UE)

## Anpassung der Controller

In der heutigen Stunde habe ich kleine logik Fehler in jenen Controller behoben. Die zwei Anlaufstellen war das überdenken welche function in welcher Datei sein soll. Hier habe ich mich dazu entschieden die **destroy function** von der **edit** in die **index** verschoben. Ein anderer fehler war die Singular-Plural logik. Hier wurden alle felder die Plural benötigen auch so geändert.



## Verbindung im Web

Der näcshte Schritt war die ganzen Routes zu verbinden. Heute habe ich die Devices und die Rooms hinzugefügt da ich diese Daten testen will und diese auch getestet habe.

```php
Route::middleware(['auth', 'verified'])->group(function () {
    Route::get('/devices', [DeviceController::class, 'index'])
        ->name('devices.index');
    Route::get('/devices/create', [DeviceController::class, 'create'])
        ->name('devices.create');
```

## Frontend erstellen

Ich habe für Ordner Devices und Rooms erstellt und in diesen Datein Show,Create,Index und Edit erstellt 

![ordner](/ordnerstruktur.png)


### Index 

In der Index habe ich einfach die Variablen der Models eingefügt und in der Navigation die Index hinzugefüt habe von Rooms und Devices
In der Index wird auch auf Create, Show und Edit verweist und man kann mit einen Delete Button die Devices Löschen.
Bei rooms habe ich noch floors ein Dropdown Menü erstellt da sich die Stockwerke nicht mehr ändern werden und diese bei 1,2,3 bleiben.

![index](/index.png)


## Fixes am Ende des Tages

In den Controllern musste ich einen Teil der Variablen wieder ändern da ich ein Problem zwischen Web und den Controllern hatte. 
Ein weiteres Problem war nochmal die Struktur in den Migrations zu bearbeiten vorallem in der devices_tabel 

