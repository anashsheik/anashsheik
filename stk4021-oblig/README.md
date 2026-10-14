# STK4021 oblig: slik bruker du `oblig.Rmd` i RStudio

## 1. Mappestruktur

Pakk ut zip-fila et sted på maskinen, for eksempel `Dokumenter/stk4021-oblig/`.
Mappa skal se slik ut:

```
stk4021-oblig/
├── oblig.Rmd        <- hele rapporten: tekst + R-kode
├── README.md        <- denne fila
└── data/
    ├── A.txt
    ├── seis00.txt
    ├── seis01.txt
    └── seis02.txt
```

`oblig.Rmd` leser dataene fra `data/`, så `oblig.Rmd` og `data/` må ligge i samme mappe.

## 2. Installer pakker (bare første gang)

1. Åpne RStudio.
2. Skriv dette i **Console** (nederst til venstre) og trykk Enter etter hver linje:

```r
install.packages(c("rmarkdown", "knitr", "tinytex"))
tinytex::install_tinytex()
```

- `rmarkdown` og `knitr` gjør `.Rmd` om til et dokument.
- `tinytex` er en liten LaTeX-installasjon som trengs for å lage **PDF**. Det tar noen minutter.
  Har du allerede LaTeX (MiKTeX/MacTeX), kan du hoppe over den siste linja.
- Start RStudio på nytt etterpå.

## 3. Åpne fila

**File → Open File…** → velg `oblig.Rmd`.

Når du åpner en `.Rmd` direkte, bruker RStudio mappa der fila ligger som arbeidsmappe
når du knitter. Derfor finner den `data/`.

## 4. Lag PDF-en (Knit)

Trykk **Knit** (ikon med garnnøste øverst i editoren), eller `Ctrl+Shift+K` (Mac: `Cmd+Shift+K`).

R kjører all koden fra toppen, setter inn tall og figurer og lager `oblig.pdf` i samme mappe.
PDF-en åpnes automatisk. Det tar 10–30 sekunder.

## 5. Hvordan en `.Rmd` er bygd opp

| Del | Hvordan den ser ut | Hva den gjør |
|---|---|---|
| Header (YAML) | mellom `---` øverst | tittel, navn, at utdata er PDF |
| Tekst | vanlig tekst, `#` = overskrift | blir brødtekst i PDF-en |
| Matematikk | `$...$` (i linja) og `$$...$$` (egen linje) | LaTeX-formler |
| Kodeblokk ("chunk") | ```` ```{r navn} ```` … ```` ``` ```` | R-kode som kjøres; figurer havner i PDF-en |
| Inline R | `` `r round(P10, 2)` `` | setter et beregnet tall rett inn i teksten |

- `echo = FALSE` (satt øverst i `setup`-blokken) skjuler koden i selve teksten.
- Den siste blokken (`ref.label = knitr::all_labels()`) samler all koden i vedlegget.
- `set.seed(...)` foran simuleringene gjør at du får de samme tallene hver gang.

## 6. Kjøre kode bit for bit (anbefalt for å lære)

- Klikk i en kodeblokk og trykk den grønne ▶ (*Run Current Chunk*) oppe til høyre i blokken.
  Resultatet vises rett under blokken, og variablene havner i **Environment** (øverst til høyre).
- Kjør blokkene **i rekkefølge ovenfra**, fordi senere blokker bruker variabler fra tidligere
  (for eksempel `A`, `Sigma_m` og `post0`). **Run → Run All** kjører alt.
- `Ctrl+Enter` kjører bare linja du står på.
- Skriv et variabelnavn i Console (for eksempel `V_summary`) for å se verdien, eller `?chol` for hjelp om en funksjon.

Hvis noe ikke finner `data/A.txt` når du kjører blokker manuelt:
**Session → Set Working Directory → To Source File Location**.

## 7. Før innlevering

1. Bytt `NAME` i `author:` øverst med navnet ditt.
2. Skriv egenrapporten (seksjonen *Self-report*) med egne ord.
3. Knit, og les gjennom hele PDF-en.
4. Lever `oblig.pdf` i Canvas. Det skal være én fil, med koden i vedlegget.

## Vanlige feil

| Feilmelding | Løsning |
|---|---|
| `cannot open file 'data/A.txt'` | `oblig.Rmd` og `data/` ligger ikke i samme mappe, eller arbeidsmappa er feil (se punkt 6) |
| `LaTeX failed to compile` / `pdflatex not found` | Kjør `tinytex::install_tinytex()` og start RStudio på nytt |
| `there is no package called 'rmarkdown'` | Kjør `install.packages("rmarkdown")` |
| `object 'post0' not found` når du kjører én blokk | Kjør blokkene ovenfor først |
