# Hent bildeadressene fra Aktiv-annonsen (10 sekunder)

Nettleseren din har tilgang til bildene – dette verktøyet samler alle
bildeadressene på annonsesiden og kopierer dem ferdig formatert.

## Slik gjør du

1. Åpne annonsen: <https://aktiv.no/bolig/boliger-til-salgs/uggdal-bardsund-10-117003569>
2. Klikk deg inn i bildegalleriet («Se alle bilder») og bla gjennom alle
   bildene, slik at alt lastes inn.
3. Trykk **F12** (eller høyreklikk → «Inspiser») og velg fanen **Console**.
4. Lim inn hele koden under og trykk **Enter**:

```js
(() => {
  const best = i => {
    if (i.srcset) {
      const c = i.srcset.split(",").map(s => s.trim().split(" ")[0]);
      return c[c.length - 1];
    }
    return i.currentSrc || i.src;
  };
  const urls = [...new Set(
    [...document.querySelectorAll("img")].map(best)
      .filter(u => u && u.startsWith("http") &&
        !/logo|icon|\.svg|avatar|ansatt|megler|profil/i.test(u))
  )];
  const out = "bilder: [\n" + urls.map(u => '  "' + u + '",').join("\n") + "\n],";
  console.log(out);
  navigator.clipboard.writeText(out).then(
    () => alert("Kopierte " + urls.length + " bildeadresser til utklippstavlen!"),
    () => prompt("Kopier teksten under manuelt:", out)
  );
})();
```

5. Du får beskjed om hvor mange adresser som ble kopiert. **Lim resultatet
   inn i chatten** (eller rett inn i `EIENDOM.bilder`-listen i
   `eiendom/index.html`), så er den automatiske bildevisningen i gang.

> Får du en tom eller kort liste? Bla gjennom hele galleriet først – bildene
> lastes inn etter hvert som du blar – og kjør koden på nytt.

## Alternativ uten konsoll

Høyreklikk på hvert bilde i galleriet → **«Kopier bildeadresse»** → lim inn
i chatten, én per linje. Skriv gjerne et stikkord bak hver («drone»,
«strand», «stue») så setter jeg riktige bildetekster.
