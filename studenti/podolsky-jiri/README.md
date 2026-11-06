# Extrakce údajů z inzerátů bytů

Nástroj pro strukturovanou extrakci klíčových informací z textů realitních inzerátů nabízejících byty (k pronájmu i prodeji). Z neformátovaného českého textu vytahuje cenu, dispozici, podlahovou plochu a město do validního JSON formátu.

## Schéma výstupu

```json
{
  "dispozice": "string nebo null",
  "plocha_m2": "integer nebo null",
  "cena_kc": "integer nebo null",
  "mesto": "string nebo null"
}
```

### Popis polí:
- `dispozice` (string / null): Normalizovaný tvar dispozice bytu (např. `"1+kk"`, `"1+1"`, `"2+kk"`, `"2+1"`, `"3+kk"`, `"3+1"`, `"4+kk"`, `"4+1"`, `"garsonka"`). Pokud není uvedena nebo je nejasná, pak `null`.
- `plocha_m2` (integer / null): Celková užitná / podlahová plocha bytu v celých metrech čtverečních (zaokrouhleno, bez jednotky `m2`). Pokud chybí, pak `null`.
- `cena_kc` (integer / null): Částka v Kč bez mezer, poplatků a měny jako celé číslo (např. `15000` nebo `4500000`). Pokud cena není uvedena nebo je "dohodou", pak `null`.
- `mesto` (string / null): Název města či obce v 1. pádě s velkým počátečním písmenem (např. `"Praha"`, `"Brno"`, `"Teplice"`). Městské části (např. `"Praha 4"`, `"Žižkov"`) se normalizují na základní město (`"Praha"`). Pokud město v textu není, pak `null`.

## Pravidla porovnávání (ground truth vs model)
1. **JSON validita:** Odpověď musí být validní JSON objekt obsahující všechny 4 klíče.
2. **Přesná shoda hodnot:**
   - `dispozice`: Case-insensitive porovnání po odstranění mezer (např. `"2+KK"` == `"2+kk"`).
   - `plocha_m2`: Číselná shoda (int).
   - `cena_kc`: Číselná shoda (int).
   - `mesto`: Case-insensitive shoda řetězce (ořez bílých znaků).
3. **Chybějící hodnota:** Pokud text údaj neobsahuje, model musí vrátit `null`. Prázdný řetězec `""` nebo `0` je vyhodnocen jako neshoda.

## Jak spustit

1. Vytvoř virtuální prostředí a nainstaluj závislosti:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```
2. Nastav klíče v `.env`:
   ```bash
   cp .env.example .env
   # doplň GROQ_API_KEY, GEMINI_API_KEY, MISTRAL_API_KEY
   ```
3. Spusť evaluační skript:
   ```bash
   python3 src/eval.py --model llama-3.3-70b-versatile --provider groq --dataset dev --prompt v1
   ```

## Výsledky

| Model | Poskytovatel | Prompt | Sada | Validní JSON | Úplná shoda | Dispozice | Plocha | Cena | Město | Latence | Tokeny (in/out) |
|---|---|---|---|---|---|---|---|---|---|---|---|
| *bude doplněno* | | | | | | | | | | | |

## Rozbor chyb
*Bude doplněno po měření.*

## Doporučení pro produkci
*Bude doplněno po měření.*

## Rozdíl dev a test
*Bude doplněno po měření.*

## AI asistent
*Bude doplněno.*

## Reflexe
*Bude doplněno.*
