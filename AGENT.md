# Eggie web — instrukce pro agenta

Veřejná stránka hry Eggie: Fart & Fly. Uživatel komunikuje česky. Heslo k registrátorovi nežádej a neukládej. Soukromé repo hry `mrmichal-sketch/eggie` není tento web. Nesahat do jeho tajemství.

## Kde to je

- Lokálně: `C:\Hermes\eggie-web`, větev `main`.
- Vzdáleně: veřejné https://github.com/mrmichal-sketch/eggie-web
- GitHub Pages: větev `main`, cesta `/`. Soubor `CNAME` obsahuje jen `eggie.fun`.
- Živá stránka: http://eggie.fun (GitHub ji servíruje). `https://` zapni, teprve až certifikát sedí. Do té doby DNS neměň.
- `github.io` po existenci `CNAME` přesměrovává na apex. Neber 301 z `https://mrmichal-sketch.github.io/eggie-web/` jako důkaz, že nová verze je venku.

Git identita v tomhle repu už je nastavená (`mrmichal-sketch`). Globální git config nepřepisuj.

## Jak upravit stránku

1. `git fetch` a podívej se, jestli lokální `main` není pozadu. Pokud ano, nejdřív rebase na `origin/main`. Push z pozadu smaže `CNAME` a doména odpadne.
2. Uprav `index.html` a soubory v `assets/`. Nové soubory musí být v gitu, jinak je Pages neobslouží.
3. Commit a `git push origin main`.
4. Po pushi ověř `git ls-files` — `CNAME` tam pořád musí být.
5. Hotovo není 200 z `http://eggie.fun`. Čekej, až `gh api repos/mrmichal-sketch/eggie-web/pages/builds` ukáže u nejnovějšího buildu `status` = `built`. Do té doby apex drží staré `Last-Modified` i přes `Cache-Control: no-cache`.
6. Pak teprve ověř tělo stránky na `http://eggie.fun`.

Custom domain znovu neclaimuj, pokud `CNAME` sedí a Pages hlásí `cname: eggie.fun`.

## Jazyk

Výchozí jazyk je angličtina. Čeština se zapne při `?lang=cs`, `?lang=cz`, nebo když referrer je `eggie.cz`. Přepínač jsou tlačítka `[data-set-lang]`. Viditelnost řeší:

`html[data-lang="en"] .cs, html[data-lang="cs"] .en, html:not([data-lang]) .cs { display: none !important; }`

Každý nový text musí mít obě verze, `.en` i `.cs`. Tagline:

- EN: Hold to rise. Release to fall. Dodge the knives, eat to keep the gas, and chase a world top 10.
- CS: Drž a stoupej. Pusť a padáš. Mine nože, jez, ať nedojde plyn, a hoň světovou top 10.

## Stažení

Neměň odkazy, dokud není nový release. Teď je `v1.0.10`:

- https://github.com/mrmichal-sketch/eggie-web/releases/download/v1.0.10/Eggie-Windows-1.0.10.zip
- https://github.com/mrmichal-sketch/eggie-web/releases/download/v1.0.10/Eggie-Android-1.0.10.apk

Samostatné `.exe` na release nedávej. App Store je odstavec bez `href` (`Waiting on Apple` / `Čeká na Apple`), dokud uživatel nedá schválený odkaz. Žádný falešný odkaz.

Zdroj binárek, až bude nová verze: `C:\Users\stare\Documents\ChatGPT\Eggie\release\downloads`.

## Co se nesmí

- Neklikat **Připojit** v administraci domény. To zapne cizí hosting, ne Pages.
- Neměnit NS, MX, TXT (SPF/DMARC) ani jména `mail`, `webmail`, `smtp`, `pop3`, `imap`, `autodiscover`, `autoconfig`, `admin`. Webové A/AAAA apexu a CNAME `www` už jsou nastavené. DNS znovu nepiš, pokud se nemění hosting.
- `eggie.cz` nepřepisuj na IP adresy GitHubu. Až se bude přesměrovávat, tak nemaskovaný Webforward na `https://eggie.fun/?lang=cs`. To zatím není hotové.
- Poštu nezakládej ze záložky DNS. **Vytvořit nový záznam** tam zakládá DNS, ne schránku.

## Ověření po úpravě

- `CNAME` je v `git ls-files`.
- Nejnovější Pages build je `built` a bez `error`.
- `http://eggie.fun` vrací nový obsah (hledej změnu, ne jen stav 200).
- Přepínač EN/CS pořád schovává druhý jazyk.
- App Store pořád nemá odkaz, pokud uživatel žádný nedodal.
