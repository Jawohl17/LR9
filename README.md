# Лабораторна робота №9: Створення віртуальних машин у GCP
Роботу виконали: Слобожан Софія, Шевченко Артем РПЗ 24-А
## Мета
Знайомство з популярними хмарним провайдером GCP, створення віртуальних машин у середовищі GCP.

## Програмне забезпечення
Браузер з доступом до Інтернету, обліковий запис Google, доступ до GCP.

---

## Теоретичні відомості

- **Google Cloud Platform (GCP)** — хмарна платформа від Google для створення VM, сховищ, баз даних, мережевих рішень, аналітики та AI.
- **Google Compute Engine (GCE)** — сервіс для створення та керування VM.
- **Machine Images** — образи ОС або попередньо налаштовані середовища.
- **Persistent Disks** — блокові диски для зберігання даних VM.
- **Snapshots** — резервні копії дисків.
- **Machine Types** — конфігурації VM (CPU, RAM, GPU).
- **VPC** — ізольовані віртуальні мережі.
- **Firewall Rules** — правила доступу до VM.
- **External IP** — публічна IP-адреса для доступу з Інтернету.
- **SSH Keys** — ключі для безпечного підключення до Linux VM.
- **RDP** — протокол для підключення до Windows VM.

---

## Контрольні питання та відповіді
## Відповіді на питання до лабораторної роботи

1. **Для чого призначений GCE?**  
   Запуск і керування віртуальними машинами у хмарі GCP.

2. **Що таке Machine Images?**  
   Знімки ОС або середовищ для швидкого розгортання VM.  
   **Стандартні** — надані Google, **Custom Images** — створені користувачем.

3. **Для чого використовуються Persistent Disks?**  
   Для зберігання даних VM.  
   **Типи:**  
   - Standard — HDD, економний, повільний  
   - Balanced — HDD/SSD середній баланс ціни і швидкості  
   - SSD — швидкий, дорожчий

4. **Що таке Snapshots?**  
   Резервні копії дисків для відновлення або створення нових VM.  
   **Використання:** перед оновленнями, видаленням VM, для резерву.

5. **Основні типи VM (Machine Types) та їх застосування:**  
   - e2 — загальне призначення, Free Tier  
   - n2 — потужні, збалансовані CPU та RAM  
   - n2-highmem — багато оперативної пам’яті  
   - n2-highcpu — висока обчислювальна потужність

6. **Доступні дистрибутиви OS у Free Tier:**  
   - Linux: Ubuntu, Debian, CentOS  
   - Windows: Windows Server 2019, Windows Server 2022

7. **Призначення Firewall rules:**  
   Дозволяють або блокують вхідний та вихідний трафік до VM, керують доступом.

8. **Різниця між SSH та RDP:**  
   - SSH — командний рядок, Linux VM  
   - RDP — графічний інтерфейс, Windows VM  
   Використовують відповідно до ОС та типу доступу.

## Практичне завдання №1.
<img width="1189" height="211" alt="image" src="https://github.com/user-attachments/assets/1c7cbada-f245-4ad1-9d5a-990b03b3ccd4" />
<img width="1858" height="779" alt="image" src="https://github.com/user-attachments/assets/6078e9d1-f81d-447d-a64e-3204e7ddf3de" />
<img width="1211" height="651" alt="image" src="https://github.com/user-attachments/assets/46628d15-289b-45f6-a06f-76b07eee01f5" />
<img width="1169" height="803" alt="image" src="https://github.com/user-attachments/assets/75986d97-91d8-477f-80b8-074af4b56be3" />
<img width="1291" height="670" alt="image" src="https://github.com/user-attachments/assets/739085cd-7755-4349-afb8-4735bda6f754" />
<img width="1078" height="176" alt="image" src="https://github.com/user-attachments/assets/50870f97-d498-4553-9408-bdc8ba25fb0b" />
<img width="1467" height="542" alt="image" src="https://github.com/user-attachments/assets/6bc5b097-1734-4b64-9b41-a31476d7143d" />
<img width="1283" height="254" alt="image" src="https://github.com/user-attachments/assets/0b8e72fc-44ff-45b3-b0dc-1447734c92bd" />
<img width="1914" height="682" alt="image" src="https://github.com/user-attachments/assets/2a8b8e8e-9f7b-4e0b-9acc-4300c65641bf" />
<img width="1413" height="79" alt="image" src="https://github.com/user-attachments/assets/d3c8ee93-e597-4dc4-ba22-d1f839a532d9" />
<img width="1636" height="94" alt="image" src="https://github.com/user-attachments/assets/76765037-1a06-43e0-a554-b81d025b9956" />
# -------------------------------
# Крок 1. Створення Linux VM (Ubuntu 22.04)
# -------------------------------
gcloud compute instances create shewslobo \
  --zone=europe-west4-a \
  --machine-type=e2-micro \
  --image-family=ubuntu-2204-lts \
  --image-project=ubuntu-os-cloud \
  --tags=http-server,https-server \
  --metadata=startup-script='#! /bin/bash
apt-get update -y
apt-get install -y nginx
systemctl enable nginx
systemctl start nginx'

# -------------------------------
# Крок 2. Перевірка створеної VM
# -------------------------------
gcloud compute instances list

# -------------------------------
# Крок 3. Підключення до VM через SSH
# -------------------------------
gcloud compute ssh shewslobo --zone=europe-west4-a

# -------------------------------
# Крок 4. Перевірка стану Nginx всередині VM
# -------------------------------
sudo systemctl status nginx

# або, якщо потрібна швидка перевірка без systemd:
curl -I http://127.0.0.1

# -------------------------------
# Крок 5. Редагування головної сторінки Nginx
# -------------------------------
sudo nano /var/www/html/index.nginx-debian.html

# Додати HTML блок:
# <h2>Команда: DevMasters</h2>
# <p>Студенти: Іваненко Іван, Петренко Олена</p>

# Зберегти: Ctrl+O → Enter, вийти: Ctrl+X

# -------------------------------
# Крок 6. Перевірка у браузері
# -------------------------------
# Дізнатись External IP
gcloud compute instances list

# У браузері:
# http://<EXTERNAL_IP>

# -------------------------------
# Крок 7. Зупинка або видалення VM
# -------------------------------

# Зупинка VM (налаштування зберігаються)
gcloud compute instances stop shewslobo --zone=europe-west4-a

# Видалення VM (всі дані зникають)
gcloud compute instances delete shewslobo --zone=europe-west4-a --quiet



## Практичне завдання №2. Віртуальна машина Windows Server – чек-лист

- [ ] Перейти до сервісу **Virtual Machines → Create** у GCP.  
- [ ] Вибрати **Region** той самий, що й для Linux VM.  
- [ ] Встановити **Machine type**: e2-micro (Free Tier).  
- [ ] Обрати **Boot disk**: Windows Server 2019 Datacenter або Windows Server 2022.  
- [ ] Позначити **Allow RDP (3389)** у Firewall.  
- [ ] Натиснути **Create** для запуску VM.  
- [ ] Відкрити властивості VM та натиснути **Set Windows password** для отримання логіна і пароля.  
- [ ] Підключитися через **Remote Desktop (RDP)**, використовуючи зовнішню IP-адресу та облікові дані.  
- [ ] Переглянути поточну назву комп’ютера через **This PC → Properties**.  
- [ ] Змінити ім’я комп’ютера на, наприклад, **WSERVER_Команда** через **Settings → System → About → Rename this PC**.  
- [ ] Перезавантажити VM після зміни імені.  
- [ ] (Опціонально) Зупинити або видалити VM через Console або Cloud Shell.

>  **Примітка:** Практичне завдання №2 не було виконано через відсутність фізичного доступу до Remote Desktop для Windows VM у поточному середовищі. Всі пункти наведено як теоретичний чек-лист з поясненням кроків.


## Контрольні питання після виконання практичних завдань

1. **Яку роль відіграють SSH-ключі та паролі?**  
   SSH-ключі забезпечують безпечне підключення до Linux VM без пароля.  
   Паролі використовуються для Windows VM або альтернативного доступу.

2. **Команда для підключення до Linux VM через gcloud CLI:**  
   ```bash
   gcloud compute ssh <vm-name> --zone=<zone>
   ### Способи підключення до Linux VM у GCP:
- SSH через браузер (Open in browser window)  
- SSH через термінал (gcloud compute ssh або стандартний ssh)  

### Як підключитися до Windows VM у GCP:
Через RDP (Remote Desktop Protocol) за зовнішньою IP-адресою, використовуючи логін і пароль.  

### Різниця між станами Stop і Delete:
- **Stop** — VM вимикається, ресурси зберігаються (можна перезапустити)  
- **Delete** — VM повністю видаляється, диск та IP також видаляються  

### Для чого використовується Startup script:
Автоматична установка та налаштування програмного забезпечення під час створення VM.  

### Як дозволити або заборонити вхідний трафік до VM у GCP:
Через **Firewall rules**: встановлюють дозволені порти та IP-адреси (наприклад, Allow HTTP/HTTPS, блокування інших).

## Висновки

Під час виконання лабораторної роботи ми ознайомилися з хмарною платформою Google Cloud Platform та сервісом Compute Engine, який дозволяє створювати та адмініструвати віртуальні машини з різними операційними системами.

Ми створили Linux-віртуальну машину, встановили на неї вебсервер Nginx та перевірили його роботу через браузер. Також внесли власну інформацію на головну сторінку вебсервера, що дало змогу попрактикуватися з конфігураційними файлами.

Для підключення до Linux VM ми використовували SSH через браузер та термінал, а для Windows VM — протокол RDP. На прикладі Windows VM змінили ім’я комп’ютера, що показало роботу з налаштуванням системи у хмарі.

Ми навчилися керувати станом віртуальних машин: команда **Stop** дозволяє тимчасово вимкнути VM без видалення ресурсів, а **Delete** повністю видаляє машину разом з дисками та IP-адресою.  

Також закріпили навички використання **Startup scripts** для автоматичної установки програмного забезпечення під час створення VM та налаштування **Firewall rules** для контролю доступу до ресурсів.

В цілому, лабораторна робота дозволила на практиці ознайомитися з основними принципами створення та управління віртуальними машинами у хмарному середовищі.
