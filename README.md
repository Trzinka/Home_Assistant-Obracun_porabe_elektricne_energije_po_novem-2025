Uporabljam nasledne add-on:
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

poleg teh uporabljam še vizualni add.on:
- https://github.com/frlequ/network-tariff-card

***************************************************************************************************************************************************************************************
Glede na zgodovino porabe moči v zimskem času (gretje na elektriko "IR paneli" + sanitarna voda "bojler")
![202501-Elektro graf poraba MOČ](https://github.com/user-attachments/assets/1bab80e5-1a41-484c-90b4-4b85dff53b52)

na te ocene dogovorjene moči po blokih Elektra si ne upam kaj spreminjati, ker se hitro zgodi trenutek nepazljivosti in se zgodi kot prikazuje zgornja slika.
![Moj elektro-Bloki](https://github.com/user-attachments/assets/958c90fb-9e70-47fe-b03d-857318a3b505)
Razmišljal sem tudi o tem, da bi naredil avtomatizacijo oziroma neko logiko, ki bi preprečevala istočasen vklop več naprav na enkrat (vse večje porabnike imam opremljene s stikali, ki merijo porabo električne energije) a po pravici ne vem kje začeti in čemu dati prednost tako, da ne preostane nič drugega kot osebna pozornost, da ne vklopim več istočasnih velikih porabnikov električne energije.
***************************************************************************************************************************************************************************************

`Moj elektro dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si. Podatki v kWh so za en dan nazaj oziroma so včerajšnji:`
![Moj elektro](https://github.com/user-attachments/assets/54ea62ce-c0a1-4322-8426-b5cf66451239)

`Home assistant network tariff dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si:`
![sensor elektro_network_tariff](https://github.com/user-attachments/assets/e5aea75c-0123-42f3-a076-4e2d0d57814f)

`Energy and tariff costs dodatek nudi naslednje podatke, ki jih dobiva direktno iz spleta https://mojelektro.si. Cene posameznih postavk lahko spremenite glede na cene vašega dobavitelja električne energije`
![Energy cost](https://github.com/user-attachments/assets/56db2565-f444-4841-8516-da3adc7b556b)

Naredil sem tudi svoje senzorje cen v datoteki sensors.yaml čeprav bi lahko uporabljal add-on https://github.com/frlequ/energy-and-tariff-costs oziroma jih kot boste kasneje videli delno uporabljam:
```yaml
#============================================
# Cene
#============================================
  - platform: template
    sensors:
      cena_elektricne_energije_et:
        friendly_name: "Cena električne energije ET"
        value_template: "0.077000"
        unit_of_measurement: "EUR"
        unique_id: "9b434ad5-7c23-4d49-8ffa-a30c17e2084a"
#============================================
      cena_dogovorjena_moc_casovni_blok_1:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 1"
        value_template: "3.613240"
        unit_of_measurement: EUR
        unique_id: 6b53b29d-2234-4dfc-a7ea-34986ad917b7

      cena_dogovorjena_moc_casovni_blok_2:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 2"
        value_template: "0.882400"
        unit_of_measurement: EUR
        unique_id: 9c9ac06f-e8af-4601-9619-a9ab5a468724

      cena_dogovorjena_moc_casovni_blok_3:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 3"
        value_template: "0.191370"
        unit_of_measurement: EUR
        unique_id: f88bbc15-0147-40a6-a143-fc26430a1e45

      cena_dogovorjena_moc_casovni_blok_4:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 4"
        value_template: "0.013160"
        unit_of_measurement: EUR
        unique_id: aa7f86ab-a151-48b8-b395-f1905cc98596
      
      cena_dogovorjena_moc_casovni_blok_5:
        friendly_name_template: "Cena za dogovorjeno moč za časovni blok 5"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: 8a395d09-d227-420f-be83-c7e9cf6bf0c1
 #=================    
      cena_prevzeta_ee_casovni_blok_1:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 1"
        value_template: "0.019580"
        unit_of_measurement: EUR
        unique_id: f56768b8-ae04-420c-b8b4-f4eb60202c70

      cena_prevzeta_ee_casovni_blok_2:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 2"
        value_template: "0.018440"
        unit_of_measurement: EUR
        unique_id: 2593a72f-f6e1-42f5-b957-7c510c6922cc

      cena_prevzeta_ee_casovni_blok_3:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 3"
        value_template: "0.018370"
        unit_of_measurement: EUR
        unique_id: 3431129c-3ee7-4b0b-89be-288a75966bf5

      cena_prevzeta_ee_casovni_blok_4:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 4"
        value_template: "0.018380"
        unit_of_measurement: EUR
        unique_id: 9a3a6150-c0d5-4854-9a3d-e972fed8faa6

      cena_prevzeta_ee_casovni_blok_5:
        friendly_name_template: "Cena za prevzeto EE za časovni blok 5"
        value_template: "0.000000"
        unit_of_measurement: EUR
        unique_id: be98de2a-a493-4d31-9986-2f109ca4d327
#============================================
      cena_prispevka_za_delovanje_operaterja_trga:
        friendly_name_template: "Cena prispevka za delovanje operaterja trga"
        value_template: "0.000130"
        unit_of_measurement: EUR
        unique_id: 0c975325-7cc9-444b-a28a-323a5158aaab

      cena_prispevka_za_energetsko_ucinkovitost:
        friendly_name_template: "Cena prispevka za energetsko učinkovitost"
        value_template: "0.000800"
        unit_of_measurement: EUR
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
        unit_of_measurement: EUR
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
Za pridobivanje podatkov direktno iz števca, ki mi to omogoča (moral sem zaprositi Elektro, da mi odpre port. Števec (Iskra AM550) je nameščen blizu WiFi dostopne točke) uporabljam P1 meter https://www.homewizard.com/shop/wi-fi-p1-meter/ imam pa še Shelly Pro 3EM https://www.shelly.si/si/merilniki-porabe-energije/71-123-shelly-pro-3em-3-fazni-merilnik.html#/41-tokovniki-120_a.

***************************************************************************************************************************************************************************************

Iz senzorja `sensor.p1_meter_power_phase_3`, ki privzeto meri porabo v W sem naredil senzor `sensor.p1_meter_power_phase_3_w_to_kwh`, ki pretvarja porabo iz W v kWh:
![sensor p1_meter_power_phase_3](https://github.com/user-attachments/assets/2a84dd16-7bc1-4f29-bd55-7ab33aad1a84)
Postopek:

Pod `Settings` izberite `Devices & services` in nato `Helpers` ter kliknite na gumb `Create helper` kot prikazuje slika:
![Create helper](https://github.com/user-attachments/assets/b95896c7-876b-437a-bbec-afc9ed6d7be8)
Iz menija izberite `Integral`:
![Integral](https://github.com/user-attachments/assets/0cae4495-64f7-43d5-9ea4-12a5660b6f38)
Zatem označite/izberite kot prikazujejo rdeče puščice. Kjer je modra puščica izberite senzor, ki meri porabo v W, pod name vpišite ime senzorja, ki ga ustvarjate in za konec še `Submit`:
![Integral create](https://github.com/user-attachments/assets/5b2beb10-91fb-4d6e-b6b8-c9630bda2f02)


***************************************************************************************************************************************************************************************

Sedaj ustvarimo še nov senzor `sensor.p1_meter_phase_3_mesecno_kwh`, ki bo zbiral mesečno porabo električne energije:
![P1 meter phase 3-Mesečno-kWh](https://github.com/user-attachments/assets/1a1a633e-acbf-46a1-8e33-0cf0023738a0)
Izberi `Utility Meter`
![Utility Meter](https://github.com/user-attachments/assets/2018862e-8cf0-4762-92f2-9038787a9479)
Zatem kjer kaže rdeča puščica izberi `mesečno`,kjer kaže modra puščica iberi senzor, ki ga želiš, dodaj ime in klikni `Submit`
![sensor p1_meter_phase_3_mesecno_kwh](https://github.com/user-attachments/assets/cf14d559-7eea-4091-ad44-6edc1a4e6a44)

Sedaj smo naredili senzor z imenom `sensor.p1_meter_phase_3_mesecno_kwh`, ki ga bomo kasneje uporabljali pri izračunu elektrike.

***************************************************************************************************************************************************************************************
V `configuration.yaml` datoteko dodamo:
```yaml
input_number:
  faza3_blok1_consumption:
    name: "Faza 3 Blok 1 Poraba"
    min: 0
    max: 10000
    step: 0.01
    unit_of_measurement: 'kWh'

  faza3_blok2_consumption:
    name: "Faza 3 Blok 2 Poraba"
    min: 0
    max: 10000
    step: 0.01
    unit_of_measurement: 'kWh'

  faza3_blok3_consumption:
    name: "Faza 3 Blok 3 Poraba"
    min: 0
    max: 10000
    step: 0.01
    unit_of_measurement: 'kWh'

  faza3_blok4_consumption:
    name: "Faza 3 Blok 4 Poraba"
    min: 0
    max: 10000
    step: 0.01
    unit_of_measurement: 'kWh'

  faza3_blok5_consumption:
    name: "Faza 3 Blok 5 Poraba"
    min: 0
    max: 10000
    step: 0.01
    unit_of_measurement: 'kWh'
```

V `automations.yaml` datoteko dodajmo:
```yaml
- id: '1739128677653'
  alias: Spremljanje porabe po blokih faza 3
  description: ''
  triggers:
  - entity_id: sensor.elektro_network_tariff
    trigger: state
  conditions: []
  actions:
  - target:
      entity_id: "{% if is_state('sensor.elektro_network_tariff', '1') %}\n  input_number.faza3_blok1_consumption\n{%
        elif is_state('sensor.elektro_network_tariff', '2') %}\n  input_number.faza3_blok2_consumption\n{%
        elif is_state('sensor.elektro_network_tariff', '3') %}\n  input_number.faza3_blok3_consumption\n{%
        elif is_state('sensor.elektro_network_tariff', '4') %}\n  input_number.faza3_blok4_consumption\n{%
        elif is_state('sensor.elektro_network_tariff', '5') %}\n  input_number.faza3_blok5_consumption\n{%
        endif %}\n"
    data:
      value: '{{ (states(''sensor.p1_meter_phase_3_mesecno_kwh'')) }}

        '
    action: input_number.set_value
  mode: single
```

Če se še spomnimo nam `sensor.elektro_network_tariff` daje podatke kateri blok je trenutno v uporabi. `input_number.faza3_blokX_consumption` so entitete katerim se bo dodajala vrednost.
***************************************************************************************************************************************************************************************


