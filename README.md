# Požičovňa športového vybavenia (GearRent)

Matúš Remeň

5ZYI32

## Stručný opis projektu
GearRent je webová aplikácia pre malú požičovňu športového vybavenia (bicykle, lyže, snowboardy, kajaky, stany a pod.). Rieši problém, že rezervácie sa bežne vybavujú telefonicky alebo cez e-mail, takže zákazník nevie, čo je voľné, a požičovňa nemá prehľad o vyťaženosti.

Zákazník si v aplikácii prezerá ponuku, filtruje ju podľa kategórie a ceny, vytvorí si rezerváciu na konkrétny termín a po vrátení vybavenia ho môže ohodnotiť. Správca spravuje ponuku (vrátane fotiek), schvaľuje rezervácie a vidí štatistiky vyťaženosti.
## Role v projekte
- **Návštevník (neprihlásený)** – prezerá ponuku, filtruje ju, pozrie si detail vybavenia a recenzie, môže sa zaregistrovať a prihlásiť. Nemôže rezervovať.
- **Zákazník (prihlásený)** – všetko čo návštevník + vytvára, upravuje a ruší vlastné rezervácie, píše recenzie, upravuje svoj profil (vrátane avatara).
- **Správca (admin)** – spravuje kategórie a vybavenie (CRUD, upload fotiek), schvaľuje/zamieta rezervácie, moderuje recenzie, spravuje používateľov (zmena role, blokovanie).

## Prípady použitia podľa rolí
**Návštevník**
- vyhľadá a vyfiltruje vybavenie podľa kategórie, ceny a dostupnosti
- zobrazí detail vybavenia s galériou a recenziami
- zaregistruje sa a prihlási

**Zákazník**
- vytvorí rezerváciu na zvolený termín (kontrola prekrývania termínov)
- zobrazí zoznam svojich rezervácií a zruší ešte nepotvrdenú rezerváciu
- upraví termín rezervácie
- napíše, upraví alebo zmaže vlastnú recenziu
- nahrá si profilový obrázok

**Správca**
- pridá, upraví, zmaže vybavenie a nahrá/zmaže jeho fotky
- vytvorí, upraví a zmaže kategóriu
- potvrdí alebo zamietne rezerváciu (priamo v tabuľke)
- zmaže nevhodnú recenziu
- zmení rolu alebo zablokuje používateľa
- pozrie si prehľad vyťaženosti (počet rezervácií podľa kategórií)

## Plánované entity
- **User** – id, meno, email, heslo, rola (`customer`/`admin`), avatar, registration_date
- **Category** – category_id, nazov, popis
- **Equipment** – id, category_id, nazov, popis, cena_za_den, stav (`available`/`maintenance`)
- **EquipmentImage** – id, equipment_id, cesta_k_suboru, poradie
- **Reservation** – id, user_id, equipment_id, datum_od, datum_do, celkova_cena, stav (`pending`/`approved`/`rejected`/`cancelled`), poznamka
- **Review** – id, user_id, equipment_id, hodnotenie (1–5), text, creation_date

## Vzťahy medzi entitami
- Category 1:N Equipment
- Equipment 1:N EquipmentImage
- User 1:N Reservation
- Equipment 1:N Reservation
- User M:N Equipment
- User 1:N Review
- Equipment 1:N Review

## Hlavné stránky aplikácie
1. **Domov** – úvod, odporúčané vybavenie, najlepšie hodnotené.
2. **Katalóg vybavenia** – tabuľka/karty s filtrovaním a stránkovaním.
3. **Detail vybavenia** – galéria, popis, kalendár obsadenosti, formulár rezervácie, recenzie.
4. **Moje rezervácie** – zoznam rezervácií prihláseného zákazníka, zrušenie/úprava.
5. **Profil** – úprava údajov, zmena hesla, upload avatara.
6. **Prihlásenie / Registrácia** – formuláre s údajmi.
7. **Admin – vybavenie a kategórie** – CRUD, upload a správa fotiek.
8. **Admin – rezervácie** – tabuľka s in-place zmenou stavu.
9. **Admin – používatelia a štatistiky** – správa rolí, prehľad vyťaženosti.

## Rozdelenie funkcionality
**Základné**
- registrácia, prihlásenie, odhlásenie, role, autorizácia
- CRUD nad entitami
- upload a mazanie fotiek vybavenia
- AJAX: prihlásenie, filtrovanie katalógu, stránkovanie, zmena stavu rezervácie
- validácia vstupov na klientovi aj serveri, ochrana proti SQL injection
- responzívny dizajn (Bootstrap + aspoň 20 vlastných CSS pravidiel), aspoň 50 riadkov JS
- docker spustenie s importom ukážkových dát (SQL seed)

**Rozširujúce**
- externé API: predpoveď počasia (Open-Meteo) na detaile vybavenia pre vonkajšie aktivity
- HTML5 API: Geolocation alebo Drag & Drop pri nahrávaní fotiek
- SASS pre štýly
- pokročilá validácia formulárov na klientovi (živá kontrola, kontrola obsadenosti termínu cez AJAX)
- graf vyťaženosti v admin štatistikách