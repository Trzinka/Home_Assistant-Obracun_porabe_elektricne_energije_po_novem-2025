## 📜 Licenca  
Ta dela so prosto dostopna za vsako uporabo brez omejitev.  
Avtor ne zahteva atribucije, vendar je hvaležen za povratne informacije.

V datoteki `configuration.yaml` zaradi boljše preglednosti razčlenite datoteko in vpišite in v korenske imeniku naredite ustrezne datoteke:
```yaml
utility_meter: !include utility_meter.yaml
automation: !include automations.yaml
sensor: !include sensors.yaml
```

ali pa tako kot imam jaz ustvarite mapo `share` in v njej naredite mapo `sensors` ter v njej poljubno poimenujte *.yaml datoteke.
```yaml
sensor: !include_dir_merge_list share/sensors/
```
![Mapa sensors](https://github.com/user-attachments/assets/00a0f1b5-1c65-4c7d-96d2-643dc7ea3cf3)

Uporabljam nasledne HACS add-on:
- https://github.com/frlequ/homeassistant-mojelektro
- https://github.com/frlequ/home-assistant-network-tariff
- https://github.com/frlequ/energy-and-tariff-costs

Naredil sem dva senzorja v datoteki sensors.yaml. Če hočete dobiti malo več podatkov oziroma v lepši obliki:
```yaml
#============================================
# Delovni dan
#============================================
  - platform: template
    sensors:
      delovni_dan:
        friendly_name: "Dela prosti dan"
        value_template: >
          {% if state_attr('sensor.elektro_network_tariff', 'holiday') %}
            Da
          {% else %}
            Ne
          {% endif %}
        icon_template: >
          {% if state_attr('sensor.elektro_network_tariff', 'holiday') %}
            mdi:calendar-check
          {% else %}
            mdi:calendar-remove
          {% endif %}
        unique_id: 3115d14c-88a2-4580-af6c-b8ccfacbe4bb
```

in še senzor:
```yaml
#============================================
# Tarifi
#============================================
  - platform: template
    sensors:
      tarifi:
        friendly_name: "Tarifi"
        value_template: >
          {% set current_month = now().month %}
          {% if current_month >= 11 or current_month <= 2 %}
            Visoka tarifa
          {% else %}
            Nizka tarifa
          {% endif %}
        unique_id: c9214188-d611-400c-99df-3b1ce45622ae
```

poleg teh uporabljam še vizualni HACS add.on:
- https://github.com/frlequ/network-tariff-card

***************************************************************************************************************************************************************************************
Glede na zgodovino porabe moči v zimskem času (gretje na elektriko "IR paneli" + sanitarna voda "bojler")
![202501-Elektro graf poraba MOČ](https://github.com/user-attachments/assets/1bab80e5-1a41-484c-90b4-4b85dff53b52)

na te ocene dogovorjene moči po blokih Elektra si ne upam kaj spreminjati, ker se hitro zgodi trenutek nepazljivosti in se zgodi kot prikazuje zgornja slika.
![Moj elektro-Bloki](https://github.com/user-attachments/assets/958c90fb-9e70-47fe-b03d-857318a3b505)
Razmišljal sem tudi o tem, da bi naredil avtomatizacijo oziroma neko logiko, ki bi preprečevala istočasen vklop več naprav istočasno (vse večje porabnike imam opremljene s pametnimi merilniki porabe elektrike in stikali, ki tudi prikazujejo trenutno porabo električne energije, nekaj jih imam pa, ki kažejo samo porabo in so brez stikala "pri nakupu bodite pozorni, da ima naprava tudi stikalo, če vam je pomembo. Za večje porabnike vsekakor svetujem le te").
Pri pralnem, sušilnem in pomivalnem stroju in pečici nam ne preostane nič drugega kot osebna pozornost, da ne vklopim istočasno več velikih porabnikov električne energije.
Lahko bi naredili neko avtomatizacijo tudi za te naprave a nastane problem, da po isklopu elektrike te naprave ne znajo samodejno nadaljevati kjer so končale brez našega fizičnega posredovanja!

***************************************************************************************************************************************************************************************

`Moj elektro dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si. Podatki v kWh so za en dan nazaj oziroma so včerajšnji:`
![Moj elektro](https://github.com/user-attachments/assets/54ea62ce-c0a1-4322-8426-b5cf66451239)

`Home assistant network tariff dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si:`
![sensor elektro_network_tariff](https://github.com/user-attachments/assets/e5aea75c-0123-42f3-a076-4e2d0d57814f)

`Energy and tariff costs dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si. Cene posameznih postavk lahko spremenite glede na cene vašega dobavitelja električne energije`
![Energy cost](https://github.com/user-attachments/assets/56db2565-f444-4841-8516-da3adc7b556b)

Naredil sem tudi svoje senzorje cen v datoteki elektrika_cenik.yaml (mapa `share` v mapi `sensors` glej prvo sliko) čeprav bi lahko uporabljal add-on https://github.com/frlequ/energy-and-tariff-costs oziroma jih kot boste kasneje videli delno uporabljam:
```yaml
#============================================
# Cene
#============================================
  - platform: template
    sensors:
      cena_elektricne_energije_et:
        friendly_name: "Cena električne energije ET"
        value_template: "0.077000"
        unit_of_measurement: "EUR/kWh"
        unique_id: "9b434ad5-7c23-4d49-8ffa-a30c17e2084a"
#============================================
      cena_dogovorjena_moc_casovni_blok_1:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 1"
        value_template: "0.912240"
        unit_of_measurement: EUR/kW
        unique_id: 6b53b29d-2234-4dfc-a7ea-34986ad917b7

      cena_dogovorjena_moc_casovni_blok_2:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 2"
        value_template: "0.912240"
        unit_of_measurement: EUR/kW
        unique_id: 9c9ac06f-e8af-4601-9619-a9ab5a468724

      cena_dogovorjena_moc_casovni_blok_3:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 3"
        value_template: "0.162970"
        unit_of_measurement: EUR/kW
        unique_id: f88bbc15-0147-40a6-a143-fc26430a1e45

      cena_dogovorjena_moc_casovni_blok_4:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 4"
        value_template: "0.004070"
        unit_of_measurement: EUR/kW
        unique_id: aa7f86ab-a151-48b8-b395-f1905cc98596
      
      cena_dogovorjena_moc_casovni_blok_5:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 5"
        value_template: "0.000000"
        unit_of_measurement: EUR/kW
        unique_id: 8a395d09-d227-420f-be83-c7e9cf6bf0c1
 #=================    
      cena_prevzeta_ee_casovni_blok_1:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 1"
        value_template: "0.019980"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: f56768b8-ae04-420c-b8b4-f4eb60202c70

      cena_prevzeta_ee_casovni_blok_2:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 2"
        value_template: "0.018330"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 2593a72f-f6e1-42f5-b957-7c510c6922cc

      cena_prevzeta_ee_casovni_blok_3:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 3"
        value_template: "0.018090"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 3431129c-3ee7-4b0b-89be-288a75966bf5

      cena_prevzeta_ee_casovni_blok_4:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 4"
        value_template: "0.018550"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: 9a3a6150-c0d5-4854-9a3d-e972fed8faa6

      cena_prevzeta_ee_casovni_blok_5:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 5"
        value_template: "0.000000"
        unit_of_measurement: EUR/kWh
        device_class: energy
        unique_id: be98de2a-a493-4d31-9986-2f109ca4d327
#============================================
      cena_prispevka_za_delovanje_operaterja_trga:
        friendly_name_template: "Cena prispevka za delovanje operaterja trga"
        value_template: "0.000130"
        unit_of_measurement: EUR/kWh
        unique_id: 0c975325-7cc9-444b-a28a-323a5158aaab

      cena_prispevka_za_energetsko_ucinkovitost:
        friendly_name_template: "Cena prispevka za energetsko učinkovitost"
        value_template: "0.000800"
        unit_of_measurement: EUR/kWh
        unique_id: 5cbf3c18-87b3-4373-ab6c-26148c5a8c60

      cena_prispevka_za_spte_in_ove:
        friendly_name_template: "Cena prispevka za SPTE in OVE"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: 6dc798f8-ba9b-4a9e-8df4-d7449c0bdea5
#============================================
      cena_trosarine:
        friendly_name_template: "Cena trošarine"
        value_template: "0.001530"
        unit_of_measurement: EUR/kWh
        unique_id: 5e1bfb3f-d6e2-4f42-b9b9-6789df157e0d
#============================================
      cena_jederske_energije:
        friendly_name_template: "Cena jederske energije"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: a7d6e709-83d3-4ade-b542-84deca5d761e

      cena_pavsalnih_stroskov:
        friendly_name_template: "Cena pavšalnih stroškov"
        value_template: "1.990000"
        unit_of_measurement: EUR
        unique_id: 980811d4-3c6f-434c-a624-bf24c460dbc1

      cena_e_popusta:
        friendly_name_template: "Cena E-popusta"
        value_template: "0.810000"
        unit_of_measurement: EUR
        unique_id: 06476065-a9e7-40fe-8deb-1e6504a19ada
```
# Za pridobivanje podatkov direktno iz števca, ki mi to omogoča (moral sem zaprositi Elektro, da mi odpre port. Števec (Iskra AM550) je nameščen blizu WiFi dostopne točke) uporabljam P1 meter https://www.homewizard.com/shop/wi-fi-p1-meter/ imam pa še Shelly Pro 3EM https://www.shelly.si/si/merilniki-porabe-energije/71-123-shelly-pro-3em-3-fazni-merilnik.html#/41-tokovniki-120_a.

***************************************************************************************************************************************************************************************

Iz senzorja `sensor.p1_meter_power_phase_3`, ki privzeto meri porabo v W sem naredil senzor `sensor.p1_meter_power_phase_3_w_to_kwh`, ki pretvarja porabo iz W v kWh:
![sensor p1_meter_power_phase_3](https://github.com/user-attachments/assets/2a84dd16-7bc1-4f29-bd55-7ab33aad1a84)
# Postopek:

Pod `Settings` izberite `Devices & services` in nato `Helpers` ter kliknite na gumb `Create helper` kot prikazuje slika:
![Create helper](https://github.com/user-attachments/assets/b95896c7-876b-437a-bbec-afc9ed6d7be8)
Iz menija izberite `Integral`:
![Integral](https://github.com/user-attachments/assets/0cae4495-64f7-43d5-9ea4-12a5660b6f38)
Zatem označite/izberite kot prikazujejo rdeče puščice. Kjer je modra puščica izberite senzor, ki meri porabo v W, pod name vpišite ime senzorja, ki ga ustvarjate in za konec še `Submit`:
![Integral create](https://github.com/user-attachments/assets/5b2beb10-91fb-4d6e-b6b8-c9630bda2f02)


***************************************************************************************************************************************************************************************

Sedaj ustvarimo še nov senzor `sensor.p1_meter_phase_3_mesecno_kwh` (senzor za 3 fazo), ki bo zbiral mesečno porabo električne energije:
![P1 meter phase 3-Mesečno-kWh](https://github.com/user-attachments/assets/1a1a633e-acbf-46a1-8e33-0cf0023738a0)
Izberi `Utility Meter`
![Utility Meter](https://github.com/user-attachments/assets/2018862e-8cf0-4762-92f2-9038787a9479)
Zatem kjer kaže rdeča puščica izberi `mesečno`, kjer kaže modra puščica izberi senzor, ki ga želiš, dodaj ime in klikni `Submit`
![sensor p1_meter_phase_3_mesecno_kwh](https://github.com/user-attachments/assets/cf14d559-7eea-4091-ad44-6edc1a4e6a44)

Sedaj smo naredili senzor z imenom `sensor.p1_meter_phase_3_mesecno_kwh` (senzor za 3 fazo), ki ga bomo kasneje uporabljali pri izračunu elektrike.
Isto naredite za preostale faze in skupno.

V `utility_meter.yaml` datoteko dodamo:
```yaml
#########################################
# BLOKI/TARIFE
##########################################
tarife_p1_meter_skupaj_mesecno:
  name: Mesečna poraba P1 meter skupaj elektrike tarife
  source: sensor.p1_meter_power_w_to_kwh
  cycle: monthly
  tariffs: 
    - 1
    - 2
    - 3
    - 4
    - 5
tarife_p1_meter_faza1_mesecno:
  name: Mesečna poraba P1 meter faza 1 elektrike tarife
  source: sensor.p1_meter_power_phase_1_w_to_kwh
  cycle: monthly
  tariffs: 
    - 1
    - 2
    - 3
    - 4
    - 5
tarife_p1_meter_faza2_mesecno:
  name: Mesečna poraba P1 meter faza 2 elektrike tarife
  source: sensor.p1_meter_power_phase_2_w_to_kwh
  cycle: monthly
  tariffs: 
    - 1
    - 2
    - 3
    - 4
    - 5
tarife_p1_meter_faza3_mesecno:
  name: Mesečna poraba P1 meter faza 3 elektrike tarife
  source: sensor.p1_meter_power_phase_3_w_to_kwh
  cycle: monthly
  tariffs: 
    - 1
    - 2
    - 3
    - 4
    - 5
```

V `automations.yaml` datoteko dodajmo:
```yaml
##########################################
#       P1 meter
##########################################
- id: '1739795608260'
  alias: Izbira_tarif_blokov
  description: ''
  triggers:
  - entity_id: sensor.elektro_network_tariff
    trigger: state
  actions:
    - choose:
        - conditions:
            - condition: state
              entity_id: sensor.elektro_network_tariff
              state: "1"
          sequence:
            - action: select.select_option
              target:
                entity_id:
                  - select.tarife_p1_meter_skupaj_mesecno
                  - select.tarife_p1_meter_faza1_mesecno
                  - select.tarife_p1_meter_faza2_mesecno
                  - select.tarife_p1_meter_faza3_mesecno
              data:
                option: '1'
        - conditions:
            - condition: state
              entity_id: sensor.elektro_network_tariff
              state: "2"
          sequence:
            - action: select.select_option
              target:
                entity_id:
                  - select.tarife_p1_meter_skupaj_mesecno
                  - select.tarife_p1_meter_faza1_mesecno
                  - select.tarife_p1_meter_faza2_mesecno
                  - select.tarife_p1_meter_faza3_mesecno
              data:
                option: '2'
        - conditions:
            - condition: state
              entity_id: sensor.elektro_network_tariff
              state: "3"
          sequence:
            - action: select.select_option
              target:
                entity_id:
                  - select.tarife_p1_meter_skupaj_mesecno
                  - select.tarife_p1_meter_faza1_mesecno
                  - select.tarife_p1_meter_faza2_mesecno
                  - select.tarife_p1_meter_faza3_mesecno
              data:
                option: '3'
        - conditions:
            - condition: state
              entity_id: sensor.elektro_network_tariff
              state: "4"
          sequence:
            - action: select.select_option
              target:
                entity_id:
                  - select.tarife_p1_meter_skupaj_mesecno
                  - select.tarife_p1_meter_faza1_mesecno
                  - select.tarife_p1_meter_faza2_mesecno
                  - select.tarife_p1_meter_faza3_mesecno
              data:
                option: '4'
        - conditions:
            - condition: state
              entity_id: sensor.elektro_network_tariff
              state: "5"
          sequence:
            - action: select.select_option
              target:
                entity_id:
                  - select.tarife_p1_meter_skupaj_mesecno
                  - select.tarife_p1_meter_faza1_mesecno
                  - select.tarife_p1_meter_faza2_mesecno
                  - select.tarife_p1_meter_faza3_mesecno
              data:
                option: '5'
  mode: single

```

Če se še spomnimo nam `sensor.elektro_network_tariff` daje podatke kateri blok je trenutno v uporabi. `sensor.tarife_p1_meter_skupaj_mesecno_1` je entiteta katera zbira porabo za tekoči mesec skupno vse faze za 1 blok.
Kot primer `sensor.tarife_p1_meter_faza3_mesecno_2` je entiteta katera zbira porabo za tekoči mesec 3 faze za 2 blok. 
Tako naredimo entitete za skupaj vse faze, vsako fazo posebaj in še po blokih!
***************************************************************************************************************************************************************************************

# Sedaj pa še izračun:
Popravljeno 12.03.2025
V datoteko sensors.yaml dodajte (jaz imam v mapi `share` in podmapi `sensors` sem ustvaril datoteko `elektrika_obracun.yaml` zaradi preglednosti, ker se mi je nabralo že ogromno entitet, ki spadajo v skupino senzorjev kot sem zapisal zgoraj):
```yaml
#=========================================================================================================================================================
#                          Obračun elektrike
#=========================================================================================================================================================
#============================================
# Izračun stroška električne energije
#============================================ 
  - platform: template
    sensors:
      skupaj_izracun_stroska_elektricne_energije:
        friendly_name: "Skupaj izračun stroška električne energije"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_elektricne_energije_et') | float(default=0)) * 
              (states('sensor.p1_meter_phase_3_mesecno_kwh') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "157cc9e7-fa6b-4961-b971-b2a5107aa273"
#============================================
# Izračun stroška omrežnine za elektroenergetski sistem
#============================================
  - platform: template
    sensors:
      izracun_stroska_dogovorjene_moci_casovni_blok1:
        friendly_name: "Izračun stroška dogovorjene moči za časovni blok 1"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_dogovorjena_moc_casovni_blok_1') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_1') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR/kW"
        unique_id: "a379725a-9627-43da-b2f3-74d5a4d1808b"

  - platform: template
    sensors:
      izracun_stroska_dogovorjene_moci_casovni_blok2:
        friendly_name: "Izračun stroška dogovorjene moči za časovni blok 2"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_dogovorjena_moc_casovni_blok_2') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_2') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR/kW"
        unique_id: "b1cde06a-4acc-47d3-a49f-265f683f366f"

  - platform: template
    sensors:
      izracun_stroska_dogovorjene_moci_casovni_blok3:
        friendly_name: "Izračun stroška dogovorjene moči za časovni blok 3"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_dogovorjena_moc_casovni_blok_3') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_3') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR/kW"
        unique_id: "8257ea99-54ea-488d-ae05-453bf55e2a4c"

  - platform: template
    sensors:
      izracun_stroska_dogovorjene_moci_casovni_blok4:
        friendly_name: "Izračun stroška dogovorjene moči za časovni blok 4"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_dogovorjena_moc_casovni_blok_4') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_4') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR/kW"
        unique_id: "df8d0f0d-4113-4804-b146-c585b60c965d"

  - platform: template
    sensors:
      izracun_stroska_dogovorjene_moci_casovni_blok5:
        friendly_name: "Izračun stroška dogovorjene moči za časovni blok 5"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_dogovorjena_moc_casovni_blok_5') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_5') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR/kW"
        unique_id: "0cf92b56-cbb2-46b9-a81d-6eced4160e4e"
#===
# SKUPAJ
#===
  - platform: template
    sensors:
      skupaj_izracun_stroska_dogovorjene_moci:
        friendly_name: "Skupaj izračun stroška dogovorjene moči"
        value_template: >
          {{ "%.5f"|format((states('sensor.izracun_stroska_dogovorjene_moci_casovni_blok1') | float(default=0)) + 
              (states('sensor.izracun_stroska_dogovorjene_moci_casovni_blok2') | float(default=0)) + 
              (states('sensor.izracun_stroska_dogovorjene_moci_casovni_blok3') | float(default=0)) + 
              (states('sensor.izracun_stroska_dogovorjene_moci_casovni_blok4') | float(default=0)) + 
              (states('sensor.izracun_stroska_dogovorjene_moci_casovni_blok5') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: "7d475559-ff86-4ef5-93b6-c3ffb1847a24"        
 #=================  
  - platform: template
    sensors:
      izracun_stroska_prevzete_ee_casovni_blok1:
        friendly_name: "Izračun stroška prevzete EE za časovni blok 1"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prevzeta_ee_casovni_blok_1') | float(default=0)) * 
              (states('sensor.tarife_p1_meter_faza3_mesecno_1') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "b8677233-fd2b-4d70-96d4-5a0015e1f63f"

  - platform: template
    sensors:
      izracun_stroska_prevzete_ee_casovni_blok2:
        friendly_name: "Izračun stroška prevzete EE za časovni blok 2"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prevzeta_ee_casovni_blok_2') | float(default=0)) * 
              (states('sensor.tarife_p1_meter_faza3_mesecno_2') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "22653dee-7dff-461a-a05a-ff6093b915cb"

  - platform: template
    sensors:
      izracun_stroska_prevzete_ee_casovni_blok3:
        friendly_name: "Izračun stroška prevzete EE za časovni blok 3"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prevzeta_ee_casovni_blok_3') | float(default=0)) * 
              (states('sensor.tarife_p1_meter_faza3_mesecno_3') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "7a8d080d-e8db-47e4-9341-79d66f49177a"

  - platform: template
    sensors:
      izracun_stroska_prevzete_ee_casovni_blok4:
        friendly_name: "Izračun stroška prevzete EE za časovni blok 4"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prevzeta_ee_casovni_blok_4') | float(default=0)) * 
              (states('sensor.tarife_p1_meter_faza3_mesecno_4') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "962ae12e-2784-4600-a775-4afeda115191"

  - platform: template
    sensors:
      izracun_stroska_prevzete_ee_casovni_blok5:
        friendly_name: "Izračun stroška prevzete EE za časovni blok 5"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prevzeta_ee_casovni_blok_5') | float(default=0)) * 
              (states('sensor.tarife_p1_meter_faza3_mesecno_5') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "16be39c0-048a-40c3-9f59-bee2237b275c"
#===
# SKUPAJ
#===
  - platform: template
    sensors:
      skupaj_izracun_stroska_prevzete_ee:
        friendly_name: "Skupaj izračun stroška prevzete EE"
        value_template: >
          {{ "%.5f"|format((states('sensor.izracun_stroska_prevzete_ee_casovni_blok1') | float(default=0)) + 
              (states('sensor.izracun_stroska_prevzete_ee_casovni_blok2') | float(default=0)) + 
              (states('sensor.izracun_stroska_prevzete_ee_casovni_blok3') | float(default=0)) + 
              (states('sensor.izracun_stroska_prevzete_ee_casovni_blok4') | float(default=0)) + 
              (states('sensor.izracun_stroska_prevzete_ee_casovni_blok5') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: "373340f1-c286-4b84-950c-55e3de69acfe" 
#============================================
# Izračun stroška prispevkov in ostalih dajatev
#============================================
      izracun_stroska_prispevka_za_delovanje_operaterja_trga:
        friendly_name: "Izračun stroška prispevka za delovanje operaterja trga"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prispevka_za_delovanje_operaterja_trga') | float(default=0)) * 
              (states('sensor.p1_meter_phase_3_mesecno_kwh') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "01357d12-b7e9-4b21-b54d-022c059a8811"

      izracun_stroska_prispevka_za_energetsko_ucinkovitost:
        friendly_name: "Izračun stroška prispevka za energetsko učinkovitost"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prispevka_za_energetsko_ucinkovitost') | float(default=0)) * 
              (states('sensor.p1_meter_phase_3_mesecno_kwh') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "470278cf-f5b1-4566-b14a-3637e1197bfb"

      izracun_stroska_prispevka_za_spte_in_ove:
        friendly_name: "Izračun stroška prispevka za SPTE in OVE"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_prispevka_za_spte_in_ove') | float(default=0)) * 
              (states('sensor.moj_elektro_casovni_blok_1') | float(default=0)) / 2) }}
        unit_of_measurement: "EUR"
        unique_id: "c3a77213-c751-496b-998f-8c3b1782c4c3"
#===
# SKUPAJ
#===
      skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev:
        friendly_name: "Skupaj izračun stroška prispevkov in ostalih dajatev"
        value_template: >
          {{ "%.5f"|format((states('sensor.izracun_stroska_prispevka_za_delovanje_operaterja_trga') | float(default=0)) + 
              (states('sensor.izracun_stroska_prispevka_za_energetsko_ucinkovitost') | float(default=0)) + 
              (states('sensor.izracun_stroska_prispevka_za_spte_in_ove') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: "620071a0-8057-4cc0-89bd-7e4c9fe3f44f"
#============================================
# Izračun stroška trošarine
#============================================
      skupaj_izracun_stroska_trosarine:
        friendly_name: "Skupaj izračun stroška trošarine"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_trosarine') | float(default=0)) * 
              (states('sensor.p1_meter_phase_3_mesecno_kwh') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "c4d265d9-c7ce-478d-9f16-8402d14e61cb"
#============================================
# Izračun stroška storitve pogodbenega računa
#============================================
      skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna:
        friendly_name: "Skupaj izračun stroška storitev pogodbenega računa"
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_jederske_energije') | float(default=0)) + 
              (states('sensor.cena_pavsalnih_stroskov') | float(default=0)) - 
              (states('sensor.cena_e_popusta') | float(default=0))) }}
        unit_of_measurement: "EUR"
        unique_id: "a15e8c65-ac34-4e68-ba1f-81d2052291e8"  

#============================================
# SKUPAJ IZRAČUN
#============================================
      skupaj_izracun_stroskov:
        friendly_name: "Skupaj izračun stroškov"
        value_template: >
          {{ "%.5f"|format((states('sensor.skupaj_izracun_stroska_elektricne_energije') | float(default=0)) + 
              (states('sensor.skupaj_izracun_stroska_dogovorjene_moci') | float(default=0)) + 
              (states('sensor.skupaj_izracun_stroska_prevzete_ee') | float(default=0)) + 
              (states('sensor.skupaj_izracun_stroska_prispevkov_in_ostalih_dajatev') | float(default=0)) + 
              (states('sensor.skupaj_izracun_stroska_trosarine') | float(default=0)) + 
              (states('sensor.skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna') | float(default=0))) }}
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        unique_id: "c88ba7d9-5e8b-467b-bc50-6c1e452f2c8c"
```

![20250218-Energija](https://github.com/user-attachments/assets/68be9c42-b88c-41a4-b5c8-509d7e3e45ad)
Če želite videti stroške porabe električne moči v Energy (zgornja slika)
dodajte v datoteko `elektrika_obracun.yaml`
```yaml
#============================================
# SKUPAJ IZRAČUN ZA ENERGIJO LOVELACE KARTICO
#============================================
      skupaj_izracun_samo_energija:
        friendly_name: "Skupaj izračun samo energija"
        unit_of_measurement: "EUR/kWh"
        device_class: energy
        value_template: >
          {{ "%.5f"|format((states('sensor.cena_elektricne_energije_et') | float(default=0)) + 
              (states('sensor.cena_prispevka_za_delovanje_operaterja_trga') | float(default=0)) + 
              (states('sensor.cena_prispevka_za_energetsko_ucinkovitost') | float(default=0)) + 
              (states('sensor.cena_trosarine') | float(default=0))) }}
        unique_id: 92d6a7a2-4566-4864-b1f0-6dabad414f6c
```

Izgled kartice:
![Ha-Elektrika](https://github.com/user-attachments/assets/07a0f281-f385-45f2-adf8-0b829fd11cac)

in še te, ki trenutno kaže podatke porabe po fazah (uporabljam samo 2 fazi od treh), blokih in skupno.

![20250218-Poraba elektrike](https://github.com/user-attachments/assets/b3e5cc94-22f0-4801-ad6f-904d1a61a5f0)

še koda kartice:
```yaml
type: horizontal-stack
cards:
  - type: vertical-stack
    cards:
      - type: tile
        entity: sensor.tarife_p1_meter_skupaj_mesecno_1
        icon: mdi:transmission-tower
        color: red
      - type: tile
        entity: sensor.tarife_p1_meter_skupaj_mesecno_2
        icon: mdi:transmission-tower
        color: orange
      - type: tile
        entity: sensor.tarife_p1_meter_skupaj_mesecno_3
        icon: mdi:transmission-tower
        color: yellow
      - type: tile
        entity: sensor.tarife_p1_meter_skupaj_mesecno_4
        icon: mdi:transmission-tower
        color: blue-grey
      - type: tile
        entity: sensor.tarife_p1_meter_skupaj_mesecno_5
        icon: mdi:transmission-tower
        color: green
    title: Skupaj
  - type: vertical-stack
    cards:
      - type: tile
        entity: sensor.tarife_p1_meter_faza2_mesecno_1
        icon: mdi:transmission-tower
        color: red
      - type: tile
        entity: sensor.tarife_p1_meter_faza2_mesecno_2
        icon: mdi:transmission-tower
        color: orange
      - type: tile
        entity: sensor.tarife_p1_meter_faza2_mesecno_3
        icon: mdi:transmission-tower
        color: yellow
      - type: tile
        entity: sensor.tarife_p1_meter_faza2_mesecno_4
        icon: mdi:transmission-tower
        color: blue-grey
      - type: tile
        entity: sensor.tarife_p1_meter_faza2_mesecno_5
        icon: mdi:transmission-tower
        color: green
    title: Faza 2
  - type: vertical-stack
    cards:
      - type: tile
        entity: sensor.tarife_p1_meter_faza3_mesecno_1
        icon: mdi:transmission-tower
        color: red
      - type: tile
        entity: sensor.tarife_p1_meter_faza3_mesecno_2
        icon: mdi:transmission-tower
        color: orange
      - type: tile
        entity: sensor.tarife_p1_meter_faza3_mesecno_3
        icon: mdi:transmission-tower
        color: yellow
      - type: tile
        entity: sensor.tarife_p1_meter_faza3_mesecno_4
        icon: mdi:transmission-tower
        color: blue-grey
      - type: tile
        entity: sensor.tarife_p1_meter_faza3_mesecno_5
        icon: mdi:transmission-tower
        color: green
    title: Faza 3
title: Energija
```

# Za bolj zahtevne še prikaz trenutnih mesečnih stroškov:
Popravljeno/dodano 12.03.2025

![20250312-Izračun porabe elektrike](https://github.com/user-attachments/assets/d5f07f42-ae7c-4b93-a43c-f7c6dc4b3bd6)

Predhodno je potrebno narediti še nekaj entitet, ki prepolovijo strošek blokov. Jaz sem ustvaril datoteko `dogovorjena_moc_polovica.yaml` v mapi `share` in v podmapi `sensors`:

```yaml
- platform: template
  sensors:
    moj_elektro_casovni_blok_1_polovica:
      friendly_name: "Moj Elektro Casovni Blok 1 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.moj_elektro_casovni_blok_1') | float / 2 }}"
      unique_id: 4525ad63-7144-4ba8-95f6-7aa649b3879c

- platform: template
  sensors:
    moj_elektro_casovni_blok_2_polovica:
      friendly_name: "Moj Elektro Casovni Blok 2 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.moj_elektro_casovni_blok_2') | float / 2 }}"
      unique_id: 4301a898-a54c-411c-ba1a-1759e8f0676d

- platform: template
  sensors:
    moj_elektro_casovni_blok_3_polovica:
      friendly_name: "Moj Elektro Casovni Blok 3 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.moj_elektro_casovni_blok_3') | float / 2 }}"
      unique_id: 38be6300-1de6-4a0c-a24e-b78abbcb6dfb

- platform: template
  sensors:
    moj_elektro_casovni_blok_4_polovica:
      friendly_name: "Moj Elektro Casovni Blok 4 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.moj_elektro_casovni_blok_4') | float / 2 }}"
      unique_id: 9b0c24d9-e872-479b-81a1-d2df008e2162

- platform: template
  sensors:
    moj_elektro_casovni_blok_5_polovica:
      friendly_name: "Moj Elektro Casovni Blok 5 Polovica"
      unit_of_measurement: "kW"
      device_class: power
      value_template: "{{ states('sensor.moj_elektro_casovni_blok_5') | float / 2 }}"
      unique_id: c17ef3b7-63bd-499a-a57d-08b870b11414
```

Še koda kartice:
```yaml
type: horizontal-stack
cards:
  - type: entities
    entities:
      - entity: sensor.cena_elektricne_energije_et
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.cena_dogovorjena_moc_casovni_blok_1
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.cena_dogovorjena_moc_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.cena_dogovorjena_moc_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.cena_dogovorjena_moc_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.cena_dogovorjena_moc_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.cena_prevzeta_ee_casovni_blok_1
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.cena_prevzeta_ee_casovni_blok_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.cena_prevzeta_ee_casovni_blok_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.cena_prevzeta_ee_casovni_blok_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.cena_prevzeta_ee_casovni_blok_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.cena_prispevka_za_delovanje_operaterja_trga
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.cena_prispevka_za_energetsko_ucinkovitost
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.cena_prispevka_za_spte_in_ove
        icon: none
        name: .
      - entity: sensor.cena_trosarine
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
    state_color: false
  - type: entities
    entities:
      - entity: sensor.p1_meter_phase_3_mesecno_kwh
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.moj_elektro_casovni_blok_1_polovica
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.moj_elektro_casovni_blok_2_polovica
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.moj_elektro_casovni_blok_3_polovica
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.moj_elektro_casovni_blok_4_polovica
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.moj_elektro_casovni_blok_5_polovica
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.tarife_p1_meter_faza3_mesecno_1
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.tarife_p1_meter_faza3_mesecno_2
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.tarife_p1_meter_faza3_mesecno_3
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.tarife_p1_meter_faza3_mesecno_4
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.tarife_p1_meter_faza3_mesecno_5
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.p1_meter_phase_3_mesecno_kwh
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.p1_meter_phase_3_mesecno_kwh
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.moj_elektro_casovni_blok_1_polovica
        icon: none
        name: .
      - entity: sensor.p1_meter_phase_3_mesecno_kwh
        icon: none
        name: .
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
    state_color: false
  - type: entities
    entities:
      - entity: sensor.skupaj_izracun_stroska_elektricne_energije
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.izracun_stroska_dogovorjene_moci_casovni_blok1
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.izracun_stroska_dogovorjene_moci_casovni_blok2
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.izracun_stroska_dogovorjene_moci_casovni_blok3
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.izracun_stroska_dogovorjene_moci_casovni_blok4
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.izracun_stroska_dogovorjene_moci_casovni_blok5
        icon: mdi:meter-electric
        card_mod:
          style: |
            :host {
              color: orange;
              }
      - entity: sensor.izracun_stroska_prevzete_ee_casovni_blok1
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.izracun_stroska_prevzete_ee_casovni_blok2
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.izracun_stroska_prevzete_ee_casovni_blok3
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.izracun_stroska_prevzete_ee_casovni_blok4
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.izracun_stroska_prevzete_ee_casovni_blok5
        card_mod:
          style: |
            :host {
              color: yellow;
              }
      - entity: sensor.izracun_stroska_prispevka_za_delovanje_operaterja_trga
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.izracun_stroska_prispevka_za_energetsko_ucinkovitost
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #AA0000;
              }
      - entity: sensor.izracun_stroska_prispevka_za_spte_in_ove
      - entity: sensor.skupaj_izracun_stroska_trosarine
        icon: mdi:transmission-tower
        card_mod:
          style: |
            :host {
              color: #2cff05;
              }
      - entity: sensor.skupaj_izracun_stroska_storitev_storitev_pogodbenega_racuna
        card_mod:
          style: |
            :host {
              color: rgb(78, 192, 245);
              }
            hui-generic-entity-row {
              border-bottom: 5px solid rgb(78, 192, 245);
              width: 100%;
              }
      - entity: sensor.skupaj_izracun_stroskov
        icon: mdi:currency-eur
        card_mod:
          style: |
            :host {
              color: red;
              font-weight: bold;
              }
    state_color: false
```

Zahvaljujem se Blažu Česnu, ki mi je nesebično nudil pomoč in frlequ https://github.com/frlequ za dodatke! Za vse ki mu želijo donirati https://buymeacoffee.com/frlequ.


***************************************************************************************************************************************************************************************
p.s. In še nekaj za vse tiste, ki imate morda čas se lotite izdelave Floor-plan kartice:
![20250211-Ha-Fp](https://github.com/user-attachments/assets/dfd50763-70ca-4114-92f7-af82f2ed675a)

https://github.com/ExperienceLovelace/ha-floorplan/discussions/411


# 📜 Licenca  
Ta dela so prosto dostopna za vsako uporabo brez omejitev.  
Avtor ne zahteva atribucije, vendar je hvaležen za povratne informacije.
