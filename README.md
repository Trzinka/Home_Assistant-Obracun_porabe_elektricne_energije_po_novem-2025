Uporabljam nasledne add-on:
- https://github.com/frlequ/homeassistant-mojelektro
- https://github.com/frlequ/home-assistant-network-tariff
- https://github.com/frlequ/energy-and-tariff-costs

poleg teh še vizualni add.on:
- https://github.com/frlequ/network-tariff-card

`Moj elektro nudi naslednje podatke:`
![Moj elektro](https://github.com/user-attachments/assets/54ea62ce-c0a1-4322-8426-b5cf66451239)

`Home assistant network tariff nudi naslednje podatke:`
![sensor elektro_network_tariff](https://github.com/user-attachments/assets/e5aea75c-0123-42f3-a076-4e2d0d57814f)

`Energy and tariff costs nudi naslednje podatke`
![Energy cost](https://github.com/user-attachments/assets/56db2565-f444-4841-8516-da3adc7b556b)

Istočasno sem naredil svoje senzorje v datoteki sensors.yaml:
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
