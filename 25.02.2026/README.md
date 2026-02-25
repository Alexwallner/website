# 25.02.2026 (3 Unterrichts Einheiten)

## Anpassung der Models
Da die Models noch nicht befüllt waren musste ich zwei Schritte durchführen:

### 1. Die Beziehungen 

Da ich in der Table Devices mit Fremdschlüsseln gearbeitet habe hier zur Veranschaung:

```php
$table->foreignIdFor(\App\Models\Brand::class)->nullable()->constrained()->cascadeOnDelete();
$table->foreignIdFor(\App\Models\Category::class)->constrained()->cascadeOnDelete();
$table->foreignIdFor(\App\Models\Room::class)->constrained()->cascadeOnDelete();
```

muss ich jetzt mit Beziehungen in meiem Model arbeiten.
Es gibt zwei Arten: Einmal mit belongsto und das andere mal mit hasmany.

**Belongsto** wird angewendet bei dem Model wo die Fremdschlüssel hingewiesen wurden.

```php
public function brand(): BelongsTo
    {
        return $this->belongsTo(Brand::class);
    }
```

Das hier ist das Beispiel für Brand, man muss das selbe für Category und Room machen.

**Hasmany** wird angewendet bei den Models die keinen Fremdschlüssel haben

```php
public function devices(): HasMany{
        return $this->hasMany(Device::class);
    }
```

### 2. Fillable

**fillable** in einem Model verwendet um festzulegen welche Daten per sogenanntem Mass Assignment in die Datenbank geschrieben werden dürfen.
Das Bedeutet so viel wenn man nur **Name** in fillable geschrieben wird und man dann in Create **Name und Stockwerk** gesetzt werden, dann wird **Stockwerk ignoriert**


## Controller befüllen per CRUD

In dieser Unterrichtseinheit wurden auch die 4 Controller befüllt. Als Beispiel verwende ich den BrandController

```php
 public function index(){

        $brand = Brand::all();
        return view('brands.index', ['brand' => $brand]);
    }
```

In der function index werden hier alle Brands ausgegeben. Bei return wird noch auf die view brands.index verwiesen. Das ist eine Blade Datei die man selber erstellt hat.

```php
public function create(){


        return view('brands.create');
    }
```
Um klarheit zu schaffen, die create function ist nicht da um direkt neue Datensätze zu erstellen sonder nur da um ein Formular zur erstellung dieser Datensätze zu schaffen.

```php

```


