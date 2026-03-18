# 18.03.2026 (4 UE)

## Funktion erstellt für Erstellung beliebiger Anzahl an Devices

```php
public function store(Request $request)
    {

        $attributes = $request->validate([
            'name' => 'required|string|max:255',
            'barcode' => 'nullable|string|unique:devices,barcode',
            'brand_id' => 'nullable|exists:brands,id',
            'category_id' => 'required|exists:categories,id',
            'room_id' => 'required|exists:rooms,id',
            'amount' => 'required|integer|min:1|max:100',
        ]);

        $amount = $attributes['amount'];


        unset($attributes['amount']);


        for ($i = 1; $i <= $amount; $i++) {
            $data = $attributes;

            if ($amount > 1 && $i > 1) {
                $data['barcode'] = null;
            }


            if ($amount > 1) {
                $data['name'] = $attributes['name'] . " ($i/$amount)";
            }

            Device::create($data);
        }

        return redirect()->route('devices.index')->with('success', "$amount Geräte wurden angelegt.");
    }

```
Um noch devices mehrfach zu erstellen habe ich die Store Methode (Create) abgeändert. Hier bei habe ich noch bei **Attributes amount** hinzugefügt. Dannach hole ich mir mit $amount = attributes['amount']; die Anzahl die eingegeben wurde. Dannach muss man jedoch unset ausführen da amount nicht in meiner Database also in keiner Tabell vorhanden ist. Danach wird eine normale for schleife ausgeführt und der count geht immer um 1 hoch bis es die eingegeben Zahl erreicht hat.


## Start des Scannes

Für den Scan musste ich vorher eine Struktur aufbauen.

Ich habe mich für die zwei Models und Migrations entschieden (InventoryCheck und InventoryItem). Dazu gibt es einen Controller Inventory.

### Aufbau dieser migrations

#### InventoryCheck

```php
Schema::create('inventory_checks', function (Blueprint $table) {
            $table->id();
            $table->foreignIdFor(\App\Models\Room::class)->constrained()->cascadeOnDelete();
            $table->timestamp('completed_at')->nullable(); // Wann wurde der Check be
            $table->timestamps();
        });
```

#### InventoryItem

```php
 Schema::create('inventory_items', function (Blueprint $table) {
            $table->id();
            $table->foreignIdFor(InventoryItem::class)->constrained()->onDelete('cascade');
            $table->foreignIdFor(\App\Models\InventoryCheck::class)->constrained()->onDelete('cascade');
            $table->foreignIdFor(Device::class)->constrained()->onDelete('cascade');
            $table->boolean('is_found')->default(false);
            $table->timestamp('scanned_at')->nullable();
            $table->timestamps();
        });
```

### Models

In diesen zwei Models musste ich noch jeweils fillable machen hier habe ich einfach die ganzen felder hinzugefügt und die Beziehungen.
Einerseit belongsto und hasmany

```php
protected $fillable = [
        'room_id',  
        'completed_at'
    ];

  
    public function room(): BelongsTo
    {
        return $this->belongsTo(Room::class);
    }

  
   public function items(): HasMany
   {
     return $this->hasMany(InventoryItem::class);
   }
```

### Anfang Controller Erstellung 

Die haupt function und noch einzige in diesem Controller ist die Start. 

```php
public function start(Room $room)
    {
        $check = InventoryCheck::create([
            'room_id' => $room->id,
        ]);

      
        foreach ($room->devices as $device) {
            InventoryItem::create([
                'inventory_check_id' => $check->id,
                'device_id' => $device->id,
                'is_found' => false,
            ]);
        }

        return redirect()->route('inventory.show', $check);
    }

   
    public function show(InventoryCheck $check)
    {
        
        $check->load('items.device', 'room');

        return view('inventory.scan', compact('check'));
    }

    
    public function scan(Request $request, InventoryCheck $check)
    {
        $barcode = $request->barcode;


        $item = InventoryItem::where('inventory_check_id', $check->id)
            ->whereHas('device', function($query) use ($barcode) {
                $query->where('barcode', $barcode);
            })->first();

        if ($item) {
            $item->update([
                'is_found' => true,
                'scanned_at' => now()
            ]);

            return response()->json([
                'success' => true,
                'device_name' => $item->device->name
            ]);
        }

        return response()->json([
            'success' => false,
            'message' => 'Gerät nicht in diesem Raum gefunden!'
        ], 404);
    }


    public function complete(InventoryCheck $check)
    {
        $check->update(['completed_at' => now()]);

        return redirect()->route('rooms.index')
            ->with('success', 'Inventur für ' . $check->room->name . ' erfolgreich abgeschlossen!');
    }
```