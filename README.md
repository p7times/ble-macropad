# BLE Macropad: controller în timp real pe RTOS

Macropad wireless cu 16 butoane, bazat pe un microcontroler ARM cu Bluetooth Low Energy. Apăsările sunt citite în timp real și trimise laptopului ca tastatură BLE (HID over GATT), pentru a executa comenzi pe laptop.

Proiect pentru disciplina *Sisteme Distribuite și de Timp Real*.

## Funcționalități

- Matrice 4x4 de butoane, scanată de un timer hardware la 1 ms, cu debounce
- Trimitere taste prin BLE (HID over GATT), fără drivere sau software pe laptop
- Feedback local pe LED-uri: tasta apăsată, starea conexiunii BLE, nivelul bateriei
- Alimentare de la acumulator Li-Po, încărcare prin USB-A
- PCB propriu, proiectat în KiCad

## Arhitectură

| Componentă | Alegere |
|---|---|
| Microcontroler | `[de ales: nRF52840 / STM32WB55]` |
| RTOS | `[de ales: Zephyr / FreeRTOS]` |
| Protocol | Bluetooth Low Energy (HID over GATT) |
| Limbaj | C |

Două taskuri independente, sincronizate prin primitive RTOS:

- **Task scanare:** citește matricea, face debounce, actualizează LED-urile, pune evenimentele în coadă
- **Task BLE:** gestionează conexiunea și trimite rapoartele HID
- **Concurență:** coadă de mesaje între taskuri, mutex pe starea comună, ISR -> semafor

## Resurse hardware folosite

Timer (scanare), PWM (LED-uri), DMA (LED-uri RGB), ADC (tensiunea bateriei).

## Structura repo-ului

```
firmware/   codul sursă C
hardware/   proiect KiCad (schemă, PCB)
docs/       specificații, schemă bloc, analiză de timp real
tools/      scripturi de test (ex. citire taste cu evdev, măsurare latență)
paper/      articol IEEE (LaTeX)
```

## Analiză de timp real

Latența apăsare -> transmisie radio se măsoară pe placă, cu un pin de test și un analizor logic. Factorii analizați: intervalul de conexiune BLE, prioritățile taskurilor, dimensiunea cozii, debounce. Detalii în `docs/`.

## Build și rulare

```
# de completat după alegerea platformei
```

## Stare

- [ ] Prototip pe placă de dezvoltare
- [ ] Scanare matrice + debounce
- [ ] BLE HID funcțional
- [ ] Taskuri, coadă, mutex
- [ ] Măsurători de latență
- [ ] PCB în KiCad
- [ ] Articol IEEE
