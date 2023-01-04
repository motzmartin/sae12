# Partie capteur.

Tout d'abord, ne devons installer des prérequis pour le bon fonctionnement de notre programme.
<br>Les commandes à entrer sont les suivantes :
```bash
sudo apt update && sudo apt upgrade
sudo apt install python3-pip libgpiod2
pip install adafruit-circuitpython-dht
```
Ensuite, nous allons créer un fichier .py afin de lire les données du capteur.
<br>Voici le code détaillé :
```python
import time, board, adafruit_dht

dht_device = adafruit_dht.DHT22(board.D4) # Charge le capteur sur le PIN GPIO 4

while True:
    try:
        temperature, humidity = dht_device.temperature, dht_device.humidity # Récupère les données du capteur
        print("Température: " + str(temperature) + " °C, Humidité: " + str(humidity) + "%", end="\r") # Affiche ces données dans la console
    except RuntimeError: # Il arrive que les données du capteur arrivent dans le Raspberry en étant corrompues, on ignore donc cette erreur
        time.sleep(2.0) # Pause de 2 secondes
        continue # On continue la boucle
    except Exception as error: # Cette erreur est considérée comme grave, elle n'est pas censée arriver mais on ne sait jamais
        dht_device.exit() # Ferme proprement la liaison entre le capteur et le programme
        raise error # Laisse l'erreur interrompre le programme
    time.sleep(2.0) # Pause de 2 secondes

# Code pensé et rédigé par Martin MOTZ.
```
