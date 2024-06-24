---
title: "[CZ] Záchrana dat ve stylu MacGyvera - zábava z katastrofy"
description: "Help"
pubDate: "Jun 24 2024"
category: "linux   - "
---

## Obnova dat ve stylu MacGyvera - sranda z katastrofy

Představte si, že uděláte kritickou chybu. Takovou, která ohrožuje vaši
obživu, osobní informace a duševní pohodu.

Instalaci Microsoft Windows.

Nedávno se stal incident. Nechal jsem Windows aktualizovat,
a po restartu byla tabulka oddílů *pryč*.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Tabulka oddílů:</strong> Datová struktura na disku, která popisuje, jak je disk rozdělen na logické sekce zvané oddíly. Říká operačnímu systému, jak přistupovat k obsahu disku.
</div>

Odkládal jsem aktualizace Windows tak dlouho, jak to jen šlo,
role tohoto OS na mém počítači byla zcela sekundární - existuje pár
videoher s anticheatem na úrovni rootkitu, velmi agresivním, které rád
občas hraju s přáteli. Bohužel je nemohu hrát na Linuxu.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Rootkit:</strong> Typ škodlivého softwaru navržený k poskytnutí neoprávněného přístupu k počítači nebo síti, přičemž aktivně skrývá svou přítomnost. Rootkity mohou být obzvláště obtížné detekovat a odstranit, protože často modifikují operační systém, aby skryly své aktivity. V kontextu anti-cheat softwaru "úroveň rootkitu" naznačuje hlubokou systémovou integraci pro komplexní monitorování integrity hry.

Je tu jeden obzvlášť závažný případ od společnosti, jejíž název začíná na *R* a končí na *iot*.
</div>

Takže možná jsem *částečně* vinen tím, že se všechno zhroutilo.

Aby to bylo ještě zajímavější, byl jsem daleko od domova, v podstatě
v přerostlé vesnici, s pouze následující technologií k dispozici:
- Jeden USB disk s instalátorem OpenBSD
- Router/Modem na vrcholu police s pouze jedním velmi krátkým ethernetovým kabelem
- Můj telefon
- Laptop, který byl nyní efektivně mrtvý

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">OpenBSD:</strong> Svobodný a open-source operační systém podobný Unixu, zaměřený na bezpečnost a správnost kódu. Je známý svými proaktivními bezpečnostními
funkcemi a často se používá ve firewallech, síťových serverech a bezpečnostními profesionály.

V neposlední řadě jej používají hipsteři jako já.
</div>

Nemohl jsem tu sehnat druhý flash disk, protože byl víkend (pátek, což je
*nejhorší možný čas*, samozřejmě).

Protože jsem nemohl naflashovat Linux na flash disk a provést obnovu dat
klasickým způsobem, musel jsem trochu *improvizovat*.

### Hm, kdo je MacGyver?

Pokud jste příliš mladí, abyste si pamatovali, MacGyver byl hrdina TV seriálu z 80. let, který dokázal vyřešit
jakýkoli problém s kancelářskou sponkou, žvýkačkou a čistou vynalézavostí. Něco jako to,
co my uživatelé Linuxu děláme, když Windows nevyhnutelně exploduje (mnoho takových případů - musel jsem si užívat
rozbité ovladače eGPU přibližně jednou za 1-2 měsíce).

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">eGPU:</strong> Externí grafická procesorová jednotka. Přenosná grafická karta, kterou lze připojit k notebooku nebo stolnímu počítači, v mém případě přes Thunderbolt 3.
Což je dobré, skvělé, ale také proprietární technologie Intelu, a mimo Linux, Mac a Windows nevěřím, že existuje jiný operační systém s ovladači.
Doufám, že USB4 to vyřeší, protože by mělo zahrnovat funkce eGPU v otevřenějším standardu. Také to bylo špatné, že jste nemohli mít TB3 na AMD notebooku.
</div>

MacGyver byl před mou dobou, ale užíval jsem si sledování Richarda Deana Andersona po léta
a léta v Stargate SG-1 a dalších.

Ale odbočuji. Zpět k našemu příběhu bídy a ztracených tabulek oddílů.

USB s OpenBSD bylo mou kancelářskou sponkou, protože to bylo něco, z čeho jsem mohl bootovat a připojit se
k internetu.

Také jsem zjistil, že `testdisk` byl k dispozici jako balíček pro OpenBSD. Nicméně,
instalátor není opravdu LiveCD, na jaké můžete být zvyklí z Linuxu - nemůže dočasně instalovat
balíčky.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">testdisk:</strong> Výkonný open-source software pro obnovu dat. Primárně se používá k obnovení ztracených oddílů a opětovnému zprovoznění nespouštěných disků. testdisk umí:
<ul>
<li>Obnovit smazané oddíly</li>
<li>Přestavět boot sektory</li>
<li>Obnovit smazané soubory z souborových systémů</li>
<li>Kopírovat soubory ze smazaných souborových systémů</li>
</ul>
Je to cenný nástroj pro správce systémů a uživatele čelící situacím ztráty dat.
</div>

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">LiveCD:</strong> Bootovatelné CD nebo USB zařízení obsahující operační systém, který běží přímo z vyjímatelného média bez instalace na pevný disk počítače.
Často se používá pro obnovu systému nebo vyzkoušení nových operačních systémů.
</div>

Hádejte, co jsem musel udělat?

*Nainstalovat OpenBSD zpět na flash disk* (instalátor se naštěstí načítá do RAM).

Nebyl to obtížný proces a musím pochválit vývojáře OpenBSD za to, že udělali
instalátor velmi uživatelsky přívětivý.

Po instalaci jsem měl příjemné překvapení: OpenBSD je jedním z mála BSD, které
má ovladače pro síťovou kartu, kterou jsem vložil do svého ThinkPadu - Intel AX200.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Intel AX-200:</strong> Bezdrátový síťový adaptér Wi-Fi 6 (802.11ax) a Bluetooth 5.1. Je známý svými vysokorychlostními schopnostmi a vylepšeným výkonem v přeplněných síťových prostředích.
Podpora pro tento novější hardware může být někdy omezená v určitých operačních systémech. Koupil jsem ho ze dvou důvodů:
<ol>
<li>Intel AC-7260, se kterým byl můj Thinkpad T480s dodán, se choval trochu podezřele a měl jsem podezření, že by mohl odejít. Mé podezření se potvrdilo o několik let později (minulý víkend),
když jsem se ho pokusil znovu vložit, abych mohl mít DragonFly BSD s podporou Wifi. Neobjevil se ani jako zařízení v Linuxu, natož v BSD. Pak jsem také našel D-Link Wifi dongle s
čipem Realtek, o kterém jsem si myslel, že je opravdu běžný. Znovu špatně, Dfly má ovladače pro mnoho, ne pro tento. Nejsou žádné ovladače pro AX-200. Kritický neúspěch u všech 3 možností bezdrátového připojení, které jsem měl. :)</li>
<li>Četl jsem článek o Wifi 6 a myslel jsem si, že je to opravdu cool, a impulzivně jsem koupil router za 5k CZK, který k té kartě pasoval. Je to nejlepší router, který vlastním, velký úspěch.</li>
</ol>
</div>

To znamenalo, že jsem mohl pokračovat ve zbytku své cesty za obnovou vsedě.

### Instalace testdisku na OpenBSD a znovuobjevení oddílů

Jsem kutil, takže jsem měl svůj hlavní Linux rozdělený na *více oddílů*, každý s jiným
souborovým systémem (EXT4, F2FS, XFS, BTRFS)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Souborové systémy:</strong> Metody organizace a ukládání dat na úložná zařízení. Různé souborové systémy mají různé funkce a výkonnostní charakteristiky:
<ul>
<li>EXT4: Čtvrtý rozšířený souborový systém, běžně používaný v Linuxu</li>
<li>F2FS: Flash-Friendly File System, navržený pro flash úložná zařízení</li>
<li>XFS: Vysoce výkonný žurnálovací souborový systém</li>
<li>BTRFS: B-tree File System, zaměřený na odolnost vůči chybám a snadnou správu</li>
</ul>
</div>

Opravdu jsem je chtěl všechny vyzkoušet, zjistit, jak fungují a experimentovat s nimi. A udělal jsem to, a bylo to skvělé. Nicméně, mezi tímto, sekundárním Linuxem,
a Windows jsem měl na disku asi *16-18 oddílů*. Ups, to je hodně.

Instalace `testdisku` byla triviální, stačilo udělat

```sh
pkg_add testdisk
```

(což možná budete muset spustit s `doas`, OpenBSD analogem `sudo`)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">doas:</strong> Příkaz podobný sudo, ale navržený tak, aby byl jednodušší a bezpečnější. Je to výchozí nástroj pro eskalaci oprávnění v OpenBSD, umožňující uživatelům spouštět příkazy s různými oprávněními.
</div>

Po několika okamžicích jsem mohl spustit `testdisk` a byl jsem přivítán známou obrazovkou:

```
TestDisk 7.2, Data Recovery Utility, February 2024
Christophe GRENIER <grenier@cgsecurity.org>
https://www.cgsecurity.org


TestDisk je bezplatný software pro obnovu dat navržený k pomoci při obnově ztracených
oddílů a/nebo zprovoznění nespouštěných disků, když jsou tyto příznaky
způsobeny vadným softwarem, určitými typy virů nebo lidskou chybou.
Může být také použit k opravě některých chyb souborového systému.

Informace shromážděné během používání TestDisku mohou být zaznamenány pro pozdější
přezkoumání. Pokud se rozhodnete vytvořit textový soubor testdisk.log, bude
obsahovat možnosti TestDisku, technické informace a různé
výstupy; včetně názvů složek/souborů, které TestDisk použil k nalezení a
zobrazení na obrazovce.

Použijte šipky pro výběr, poté stiskněte klávesu Enter:
>[ Create ] Vytvořit nový log soubor
 [ Append ] Připojit informace k log souboru
 [ No Log ] Nezaznamenávat nic
```
<small>Možná [není nic sladšího než neděle](https://www.youtube.com/watch?v=FNTKEGVSGo8), ale úvodní obrazovka testdisku je těsně druhá</small>

TestDisk je můj spasitel, můj rytíř v zářivém brnění. Byl tu jen jeden malý problém: Nebyl zkompilován
s podporou pro všechny souborové systémy, které jsem používal. Přesto jsem byl *nadšený*, protože můj domovský oddíl byl `EXT4`.

### Obnova klíčů a dalších tajemství z mého /home oddílu

Věděl jsem, že oddíl tam je, a že mohu obnovit své SSH klíče (a další klíče) a další tajemství, takže jsem mohl
spustit `testdisk` na NVME SSD, které je v mém notebooku, a voilà, měl bych být schopen získat všechno zpět.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">NVME SSD:</strong> NVMe (Non-Volatile Memory Express) je hostitelské řadičové rozhraní a úložný protokol, který výrazně zrychluje přenos dat mezi klientskými systémy a SSD. NVMe SSD nabízejí mnohem rychlejší čtení/zápis ve srovnání s tradičními SATA SSD.
</div>

Byl tu jeden malý problém:
- I když z továrny měl tento Thinkpad 256GB NVME SSD disk, *hmm, trošku jsem to poupgradil*. To, co bylo 256GB,
  je nyní 4TB Western Digital WD Black disk (měl jsem s nimi dobré zkušenosti a mám druhý, menší WD Black disk,
  který stále běží perfektně navzdory tomu, že trpěl mnohem větším zneužíváním)
- Procházení 4 terabajtů trvá *dlouhou dobu*

A tak jsem musel čekat a čekat. A čekat. Tato část procesu trvala asi 5 hodin.

#### Sike lol

Nový zábavný problém: `testdisk` našel *mnohem více oddílů*, než jsem věděl, že existují. Moje teorie je, že buď vykopal oddíly z
dávné minulosti (což by neměl, tento disk je docela nový), nebo protože jsem měl bitové kopie disků uložené na SSD, rozpoznává
oddíly v nich.

Nicméně, `testdisk` rozpoznal mnohem více oddílů, než měl.

![](./macgyver/sweaty_bear.jpg)

Na chvíli jsem se obával.

Ale pak se opět projevila moje zvědavost a náklonnost k kutilství a začal jsem prohlížet oddíly. Podařilo se mi
identifikovat můj /home oddíl bez nejmenších pochybností, což mi umožnilo použít funkci individuální obnovy souborů
`testdisku` ke zkopírování klíčů a tajemství.

Díky tomu bych mohl znovu pracovat.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">SSH klíče:</strong> SSH (Secure Shell) klíče jsou kryptografické páry klíčů používané pro bezpečnou komunikaci a autentizaci v síťových protokolech. Poskytují bezpečnější alternativu k přihlášení pomocí hesla, zejména pro vzdálený přístup k systému a správu.
</div>

> Všechny své pracovní věci mám na rackovém serveru s AMD EPYC, takže mohu kompilovat všechny ty Rust programy
> dostatečně rychle. K tomuto serveru přistupuji přes SSH a vyvíjím přímo na serveru (editor `kakoune` funguje dostatečně dobře
> přes SSH)

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">AMD EPYC:</strong> Řada vysoce výkonných x86-64 mikroprocesorů navržených společností AMD pro použití v serverech a datových centrech. Procesory EPYC jsou známé svým vysokým počtem jader, což je činí vynikajícími pro úlohy paralelního zpracování, jako je kompilace.
</div>

### Další kroky

Skutečnost, že jsem našel více oddílů, než by mělo být, mě inspirovala k tomu, abych k věcem přistupoval *velmi opatrně*.
V tomto bodě jsem byl stále ještě na OpenBSD a chtěl jsem se vrátit k Linuxu, protože jsem tam znal nástroje lépe.

Na Linuxu mohu připojit oddíl bez tabulky oddílů takto:

```sh
sudo mount -t ext4 -o ro,offset=<offset oddílu> /dev/nvme0n1 /mnt/mujodil
```
<small><small>pozor na jednotky - Bajty != Kilobajty != Bloky</small></small>

Offset najdete pomocí `fdisk` (na správně fungujících discích), nebo v našem případě pomocí `testdisk`.

Proces na OpenBSD je o něco složitější:

```sh
vnconfig vnd0 /dev/rsd0c -o <offset>
mount -t ext2fs -o ro /dev/vnd0c /mnt/mujodil
```

Nejprve musíme deklarovat virtuální diskové zařízení na správném offsetu, pak ho můžeme připojit.

Měl jsem možnost rekonstruovat *nějakou* tabulku oddílů pomocí `testdisk`. To by však neslo riziko
obnovení nesprávné tabulky oddílů a já jsem chtěl být v tomto ohledu *velmi opatrný*.

To mě vedlo k formulaci dalšího plánu:
1. Vytvořit obraz celého 4TB disku
2. Nahrát obraz na můj serverový stroj, který má další, zcela prázdný 10TB disk
3. Vytáhnout oddíly jeden po druhém a prohlédnout si je

První část je nejjednodušší:

```sh
dd if=/dev/nvme0n1 of=/nekde/disk.img status=progress bs=1M
```

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">dd:</strong> Nástroj příkazového řádku pro Unix a systémy podobné Unixu. Jeho primárním účelem je konverze a kopírování souborů, ale může být také použit k vytváření obrazů disků. Název "dd" údajně pochází z "data definition", JCL příkazu.
</div>

S offsety a velikostmi z `testdisk` je poslední část velmi snadná.

Můžete znovu použít `dd`:

```sh
dd if=puvodni_obraz_disku.img of=extrahovany_oddil.img bs=1 skip=$OFFSET count=$VELIKOST
```
<small>na Linuxu můžete pro chuť přidat `status=progress`. Pokud jsou velikost a offset vašeho oddílu v blocích, nepoužívejte `bs=1`</small>

Překvapivě mnoho lidí neví, že `dd` je v podstatě jen chytřejší `cp`, a že
Linux opravdu dodržuje mantru "všechno je soubor". Takže dd může brát soubory a zařízení jako kterýkoli parametr,
a může také brát vstup ze stdin (pokud nenastavíte `if=`) nebo výstup do stdout (pokud nenastavíte `of=`).

Toto jsou skutečné dvě výzvy, které jsem musel vyřešit:
- Kam uložím obraz? Bude mít 4TB
- Jak ho efektivně přenesu přes síť - disk byl poměrně nový a určitě by tam byly tuny nul, které by se
  dobře komprimovaly
  - Můj internet tady není nejrychlejší

K řešení prvního problému jsem se rozhodl objednat flash disk (abych mohl spustit Linux) a 5TB externí pevný disk.
Ty by měly přijít v pondělí, tak jsem se mezitím šel dotknout trávy:

![](./macgyver/grass.png)
<small>podívejte se na to, midjourney vygeneroval správný počet prstů, poprvé!</small>

Trochu jsem si pohrál s OpenBSD. Jedna věc, kterou jsem shledal obzvláště zajímavou, je `pledge()`. Je to
bezpečnostní opatření, které umožňuje procesu dobrovolně omezit zdroje, které používá, v podstatě se *zavazujete, že
budete používat jen to, co říkáte*.

Pokud ne, jádro vás zastřelí.

To je podobné Linuxovému **seccomp**, ale je to mnohem jednodušší na použití.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">seccomp:</strong> Zkratka pro "secure computing mode", je to bezpečnostní zařízení v jádru Linuxu. seccomp umožňuje procesu provést jednosměrný přechod do "bezpečného" stavu, kde nemůže
provádět žádná systémová volání kromě těch povolených.
</div>

Zvažte `pledge()`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    // Zavazujeme se používat pouze stdio a rpath (pro čtení souborů)
    if (pledge("stdio rpath", NULL) == -1) {
        perror("pledge");
        return 1;
    }

    FILE *file = fopen("/etc/hosts", "r");
    if (file == NULL) {
        perror("Chyba při otevírání souboru");
        return 1;
    }

    char buffer[256];
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        printf("%s", buffer);
    }

    fclose(file);
    return 0;
}
```

versus přibližně ekvivalentní `seccomp`:

```c
#include <stdio.h>
#include <stdlib.h>
#include <seccomp.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    scmp_filter_ctx ctx;

    ctx = seccomp_init(SCMP_ACT_KILL); // Ukončit proces, pokud provede nepovolené systémové volání

    // Povolit nezbytná systémová volání
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(read), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(write), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(open), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(close), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(fstat), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(exit_group), 0);

    if (seccomp_load(ctx) < 0) {
        perror("seccomp_load");
        return 1;
    }

    FILE *file = fopen("/etc/hosts", "r");
    if (file == NULL) {
        perror("Chyba při otevírání souboru");
        return 1;
    }

    char buffer[256];
    while (fgets(buffer, sizeof(buffer), file) != NULL) {
        printf("%s", buffer);
    }

    fclose(file);
    return 0;
}
```

Všimněte si, že abychom byli spravedliví, musíme přiznat, že seccomp je velmi jemný nástroj pro omezování systémových volání a
je velmi konfigurovatelný. Jen že vstupní náklady jsou výrazně vyšší než u `pledge()`, a proto
ho většina vývojářů nepoužívá.

### Závěr

Než jsem vytvořil 4TB kopii obrazu, stáhl jsem si iso [Void Linuxu](https://voidlinux.org/), naflashoval ho na druhý
flash disk a přešel na Linux. Pár dní bych běžel z live prostředí.

<div style="background-color: #7FB4CA; padding: 10px; margin: 10px 0;">
<strong style="color: #000">Void Linux:</strong> Nezávislá linuxová distribuce, která používá správce balíčků XBPS a init systém runit. Je známá svým modelem průběžných vydání, minimalismem a zaměřením na stabilitu.
</div>

Vytvoření 4TB obrazu na externím HDD trvalo téměř 3 dny čistého času. Použil jsem nejrychlejší USB port,
který jsem měl, a naformátoval jsem HDD jako `ext4` (potřebuji souborový systém, který zvládne velké soubory, a byl co nejvíce univerzální a NE NTFS).
Tři dny nebyly tak špatný čas.

Jakmile to bylo hotovo, měl jsem HDD s obrazem mého původního disku a mohl jsem udělat dvě věci:
- Pracovat s obrazem
- Znovu nainstalovat věci na můj disk

A tak jsem zpět na Linuxu. Artix, s init systémem S6 a jádrem `linux-cachyos-bore`.

V druhé části mého úkolu budeme psát program v Rustu pro obnovitelný přenos 4TB souboru s kompresí zstandard.
