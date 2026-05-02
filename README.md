# Prognoza generacji OZE w Polsce

**Autorzy:**
- Wiktor Matla (148896)
- Bartłomiej Kachniarz (76901)

## Cel

Model sieci neuronowej prognozujący godzinową generację energii wiatrowej i fotowoltaicznej w Krajowym Systemie Elektroenergetycznym (KSE) na 24h do przodu.

- **Wejście:** prognoza pogody na kolejne 24h dla kilku lokalizacji w Polsce + dane historyczne pogody i generacji 2023–2025
- **Wyjście:** jeden model, dwa targety (`wi`, `pv`), 24 godziny w przód, krok godzinowy

## Struktura projektu


Projekt_DL/
├── 01_pse_generacja.ipynb       pobranie generacji OZE z PSE
├── 02_pogoda.ipynb              pobranie pogody historycznej z Open-Meteo Archive
├── 03_eda.ipynb                 łączenie zbiorów + EDA
├── 04_modele_baseline_mlp.ipynb baseline + MLP
├── 05_modele_lstm.ipynb         LSTM seq2seq + strojenie hiperparametrów
├── 06_wyniki.ipynb              porównanie modeli + analiza błędów
├── data/
│   ├── pse/                     cache CSV z API PSE (gitignore)
│   ├── pse_archiwum/            ręcznie pobrane CSV-ki ze strony PSE (gitignore)
│   └── processed/               wynikowe pliki przekazywane między notebookami
├── README.md
└── .gitignore


Każdy notebook czyta dane z `data/processed/` od poprzedniego i zapisuje swoje wyniki.

## Plan pracy

1. **Wstęp**
2. **Dane** — generacja z PSE (`01`), pogoda z Open-Meteo Archive (`02`), łączenie i czyszczenie (`03`)
3. **EDA** (`03`) — rozkłady, sezonowość, korelacje, outliery
4. **Architektura sieci i procedura uczenia** (`04`, `05`) — baseline, MLP, LSTM seq2seq, walk-forward, strojenie
5. **Wyniki** (`06`) — metryki MAE/RMSE, analiza błędów
6. **Podsumowanie** (`06`)

## Źródła danych

- **Generacja OZE:** [API PSE](https://api.raporty.pse.pl) (od 2024-06-14) + ręczny eksport CSV ze [strony archiwum PSE](https://www.pse.pl/dane-systemowe/funkcjonowanie-kse/raporty-dobowe-z-pracy-kse/generacja-zrodel-wiatrowych) (2023-01-01 → 2024-06-13)
- **Pogoda historyczna:** [Open-Meteo Archive](https://open-meteo.com/en/docs/historical-weather-api), reanaliza ERA5