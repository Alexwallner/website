# 25.03.2026 (4 Unterrichts Einheiten)


## Scan Funktion Einbauen

```php
<script src="https://unpkg.com/html5-qrcode"></script>

    <script>
        document.addEventListener("DOMContentLoaded", function () {

            const scanBtn = document.getElementById('scan-barcode-btn');
            const reader = document.getElementById('reader');
            const barcodeInput = document.getElementById('barcode');

            let scanner;

            scanBtn.addEventListener('click', function () {

                reader.classList.remove('hidden');

                scanner = new Html5QrcodeScanner("reader", {
                    fps: 10,
                    qrbox: { width: 250, height: 250 }
                });

                scanner.render(onScanSuccess);
            });

            function onScanSuccess(decodedText) {

                barcodeInput.value = decodedText;

                scanner.clear();
                reader.classList.add('hidden');
            }

        });
        </script>
```

Scanner über Laptop-Kamera gestartet und Barcode automatisch ins Input feld geschrieben


## Code parts

```php
scanner = new Html5QrcodeScanner("reader", {
    fps: 10,
    qrbox: { width: 250, height: 250 }
});
```

Erstellt Scanner und legt fest wie schnell gescannt wird und wie groß der Scanbereich ist.


```php
scanner.render(onScanSuccess);
```

Startet die Kamera und beginnt mit dem Scannen.
Wenn ein Code erkannt wird, wird automatisch die Funktion onScanSuccess aufgerufen



```php
function onScanSuccess(decodedText)
```

Wird ausgeführt sobald ein Barcode erkannt wurde.
decodedText enthält den gescannten Wert (z.B. die Barcode-Nummer).

Dies wurde in der Room.index in der device.edit und in der device.create eingebaut 


## Bug Fixes 














