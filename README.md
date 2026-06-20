# Block Jump – hra pre Flipper Zero

Jednoduchá "endless runner" hra v štýle skákania nad prekážkami (žáner Super Mario),
ale s vlastnou originálnou grafikou – štvorčeková postavička s "čiapkou", nie Nintendo
postavička (tá je copyrightovaná, takže ju nepoužívam).

## Ako skompilovať a nahrať na Flipper Zero

1. Nainštaluj ufbt (micro Flipper Build Tool):
   ```
   pip install ufbt --break-system-packages
   ufbt update
   ```

2. V priečinku `block_jump` (kde je `application.fam`) spusti:
   ```
   ufbt build
   ```

3. Pripoj Flipper Zero cez USB a nahraj priamo:
   ```
   ufbt launch
   ```
   alebo skopíruj výsledný `.fap` z priečinka `dist/` do Apps → Games cez qFlipper / mobilnú appku.

## Ovládanie

- **Up / OK** = skok
- **Back** = ukončiť hru
- Po Game Over: **OK** = reštart

## Ako to funguje

- `GameContext` drží stav hry (pozícia postavičky, prekážka, skóre, rýchlosť).
- `draw_callback` kreslí scénu na canvas (zem, postavička, prekážka, skóre).
- `timer_callback` (cca 30× za sekundu) rieši gravitáciu, pohyb prekážky a koliziu.
- Zvuky idú cez `furi_hal_speaker_*` – krátky tón pri skoku, "cink" pri získaní bodu,
  nízky tón pri Game Over.

## Poznámka k API

Kód vychádza z bežných patternov Flipper Zero SDK (gui/canvas, FuriTimer, FuriMessageQueue,
furi_hal_speaker). Flipper firmware existuje vo viacerých vetvách (official, Momentum,
Unleashed) a API sa medzi verziami mierne líši – pri prvom builde môžeš dostať pár
chýb kompilátora, ktoré sa zvyčajne riešia drobnou úpravou názvu funkcie/include podľa
hlášky. Nemám tu k dispozícii reálny Flipper SDK toolchain na odskúšanie kompilácie,
takže to berz ako solídny základ, nie 100% 