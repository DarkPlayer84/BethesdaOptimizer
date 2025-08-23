# BethOptimizer — Standalone

Strumento **zero‑install** (solo HTML/JS/CSS) per generare file **INI** ottimizzati per i giochi Bethesda, eseguendo tutto **in locale nel browser**.

**Supporto attuale**
- Skyrim Anniversary Edition (SE/AE) → `SkyrimCustom.ini`
- Fallout 4 → `Fallout4Custom.ini`
- Starfield → `StarfieldPrefs.ini` (disattiva VSync; cap via driver/RTSS)
- Oblivion Remastered (UE) → `Engine.ini`

**Uso rapido**
1. Apri `index.html` nel browser desktop.
2. Vai su **Display & Refresh** e premi **Rileva Hz**.
3. Aggiungi i file generati all’**Area Download** e salvali.

**Licenza & attribuzione**  
I file e il sorgente sono pubblici. Se modifichi, redistribuisci o carichi il progetto altrove, **cita “DarkPlayer84” come autore originale** e mantieni questa nota di attribuzione.
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# BethOptimizer Helper Kit (v1.1) Aggiornamento dei files ad .NET 8 e 9

Questo kit aggiunge funzioni **native** per far funzionare l'app al 100%:
- Lettura **Hz monitor** e **nome GPU** via DLL C++ (WinAPI).
- Server HTTP locale (C# .NET 8/9) per esporre API a cui l'app (HTML) può fare `fetch`.
- Scrittura diretta dei file INI nelle cartelle **My Games** con backup `.bak` e newline **CRLF**.

## Progetti (Visual Studio 2022)
- **BethOptimizer.Native** — C++ DLL (`BethOptimizer.Native.dll`)
- **BethOptimizer.Helper** — .NET 8/9 console che P/Invokes la DLL e espone API su `http://127.0.0.1:8765/`

## Build
1. Apri `BethOptimizer.sln` in Visual Studio 2022.
2. Compila:
   - `BethOptimizer.Native` (x64, Release)
   - `BethOptimizer.Helper` (Release)
   Oppure usa: `scripts\build.bat`

## Esecuzione
- Esegui `scripts\run-helper.bat` (copia la DLL accanto all'EXE e avvia il server).
- L'app web può chiamare:
  - `GET /health`
  - `GET /api/refresh` → `{ primaryHz, monitors:[{deviceName, deviceString, hz}] }`
  - `GET /api/gpu` → `{ name, source }`
  - `POST /api/write-ini` → scrive gli INI (vedi payload in `web-integration/helper-integration.js`)
  - `GET /api/steam-libraries` → librerie Steam (best effort)

## Integrazione web
Importa `web-integration/helper-integration.js` e usa `boHelper.available()` per capire se l'helper è attivo. Poi chiama `boHelper.writeIni(...)`, ecc.

> Nota: il server abilita **CORS** per semplicità. È un tool locale per uso personale.

## Limitazioni
- Il browser non può caricare DLL: serve l'helper.
- La lettura Hz via WinAPI è di tipo **corrente** (impostazione attiva). Per Hz “reali” su alcuni pannelli può servire DXGI avanzato.
- Le API HTTP locali sono **non autenticate**: non esporle sulla rete, sono pensate solo per `127.0.0.1`.
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Cartella Zip OLD Cancellato
- Aggiornamento BethOld.zip è troppo grande per essere caricato, si torna al vecchio metodo
