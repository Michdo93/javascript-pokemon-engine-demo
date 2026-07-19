# Tile/Sprite-Werkzeugkette + Engine

Alle Dateien in **einen Ordner** zusammen mit deinen zwei Bilddateien legen,
exakt benannt: **`tileset.png`** und **`hero.png`** (oder `IMG_SRC`/`TILESET_SRC`/`HERO_SRC`
ganz oben im jeweiligen `<script>` anpassen). Dann einfach im Browser öffnen
(Doppelklick auf die `.html`-Datei).

## Dateien

| Datei | Zweck |
|---|---|
| `01-tileset-cutter.html` | Einzelne 8x8-Kachel aus `tileset.png` ansehen/finden. Klick ins Bild wählt eine Kachel, Liste + JS-Export zum Sammeln von Namen↔Koordinaten. |
| `02-tileset-block-composer.html` | Einen 16x16-**Block** aus 4x 8x8-Kacheln zusammensetzen (TL/TR/BL/BR), optional 2. Ebene (transparent, liegt oben drauf). Live-Vorschau Ebene1 / Ebene2 / kombiniert. |
| `03-tileset-multi-block.html` | Mehrere Blöcke zu einem **zusammengesetzten Objekt** anordnen (z.B. Baum aus 2x3 Blöcken). Palette anlegen, ins Raster malen, pro Zelle Kollisions-Code (frei/blockiert/dahinter) setzen. |
| `04-hero-sprite-cutter.html` | Rechteck-Auswahl (Drag) in `hero.png`, den 12 Zellen (4 Richtungen × 3 Frames) zuweisen. Mit "spiegeln"-Option, um z.B. "links" aus "rechts" abzuleiten. |
| `05-hero-animation-preview.html` | Den `HERO_FRAMES`-Export aus Tool 4 einfügen und die Lauf-Animation je Richtung abspielen/durchklicken. |
| `06-engine.html` | Die eigentliche Spiel-Engine: Grid-Bewegung, 2-Ebenen-Rendering, Kollisions-Codes, Tiefensortierung, Schild-Skripte. Lädt `tileset.png`/`hero.png` direkt. |

## Koordinaten-Konventionen

- **Tileset**: alles in **8×8-Raster-Einheiten** (Spalte, Zeile), Ursprung oben links.
  Pixel = `spalte*8, zeile*8`. Ein "Block" = 4 solcher Kacheln (2×2) = 16×16 Pixel.
- **Hero**: rohe **Pixel-Rechtecke** `{x, y, w, h}` (+ optional `flip:true`), da Hero-Sprites
  erfahrungsgemäß nicht sauber im Raster liegen. Ursprung ebenfalls oben links.

## Workflow

1. Mit **Tool 1** die Kacheln finden, die du brauchst (Gras, Weg, Baum-Teile, …), Koordinaten notieren.
2. Mit **Tool 2** daraus 16×16-Blöcke bauen (Ebene 1 = Untergrund, Ebene 2 = Objekt-Overlay).
3. Für zusammengesetzte Objekte (Baum, Zaun-Ecke, …): mit **Tool 3** mehrere Blöcke zu einem
   Objekt anordnen inkl. Kollisions-Code pro Zelle, JS exportieren.
4. Mit **Tool 4** die Hero-Sprites zuschneiden (12 Zellen: unten/oben/links/rechts × 3 Frames).
5. Mit **Tool 5** die Animation prüfen, bevor du sie in die Engine übernimmst.
6. Die Exporte aus Tool 2/3/4 in `06-engine.html` einsetzen:
   - `GROUND_BLOCKS` (Format wie Tool-2-Export)
   - `placeTileObject(OBJECT_xxx, x, y)` für Tool-3-Objekte (z.B. echter Baum statt des
     aktuell prozedural gezeichneten Platzhalter-Baums)
   - `HERO_FRAMES` (Format wie Tool-4-Export)

## Aktueller Stand in `06-engine.html`

- `GROUND_BLOCKS.GRASS` / `.PATH` sind mit verifizierten Koordinaten aus dem Beispiel-Tileset
  vorbelegt (reine Gras- bzw. Erdweg-Fläche).
- `HERO_FRAMES` sind mit den im Chat verifizierten Koordinaten aus dem Beispiel-Sprite
  vorbelegt (unten / oben / links / rechts, inkl. Spiegelung für links).
- Der **Baum** ist weiterhin prozedural gezeichnet (Canvas-Formen, kein Tileset-Ausschnitt) –
  im Beispiel-Tileset liegen Bäume nicht sauber im 16px-Raster (per Alpha-Kanal-Analyse
  nachgewiesen: benachbarte Baumgrafiken hängen bildlich zusammen). Sobald du dir mit
  Tool 3 einen echten Baum zusammengesetzt hast, `placeTileObject(...)` verwenden.
