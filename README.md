# StreamFab-Docker

Läuft StreamFab in einem browser-zugänglichen Desktop auf deinem
Unraid-Server — via Wine, auf Basis von `linuxserver/webtop`. Gleiches
Prinzip wie beim DVDFab-Paket.

## Geprüft, nicht angenommen

StreamFab gibt es offiziell nur für Windows, Mac und Android — keine
native Linux-Version, auch nicht als Alpha wie damals bei DVDFab. Der
von der Community (u. a. im DVDFab-Forum-Umfeld) empfohlene Weg ist
ebenfalls Wine. Es gibt kein offizielles StreamFab-Docker-Image.

## Unterschied zum DVDFab-Paket

- Kein optisches Laufwerk nötig (StreamFab lädt von Streaming-Seiten,
  nicht von Disc) — daher kein Device-Passthrough im Compose-File/Template.
- StreamFab bringt einen eingebauten Chromium-basierten Browser zum
  Stream-Capture mit. Das ist der Teil, der unter Wine am ehesten
  zickt (GPU-Beschleunigung, Sandboxing von Chromium-Embedded-Prozessen).
  Ich habe das nicht selbst getestet — probier es aus, bevor du dich
  drauf verlässt. Falls der eingebaute Browser abstürzt oder schwarz
  bleibt, hilft oft `winetricks d3dcompiler_47` (ist schon im Init-Skript
  dabei) oder testweise die Software-Rendering-Option in StreamFabs
  eigenen Einstellungen.
- Andere Ports im Compose-Beispiel (3010/3011 statt 3000/3001), falls
  du DVDFab und StreamFab parallel auf demselben Host laufen lassen willst.

## Setup

1. `docker compose build`
2. Container starten: `docker compose up -d`
3. `StreamFab_Setup*.exe` in `appdata/installer/` legen, Container neu starten
4. `http://<unraid-ip>:3010` öffnen → Installer läuft im XFCE-Desktop,
   Installation/Lizenz-Login wie gewohnt per Maus/Tastatur
5. Output-Verzeichnis in StreamFab auf den mit `/output` gemappten Pfad stellen

## Lizenz

Wie beim DVDFab-Paket: kein StreamFab im Image enthalten, du bringst
deinen eigenen lizenzierten Installer mit und aktivierst normal in der App.

## Dateien

- `Dockerfile`
- `root/etc/cont-init.d/90-streamfab-setup`
- `docker-compose.yml`
- `streamfab-unraid-template.xml`
