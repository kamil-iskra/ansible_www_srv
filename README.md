# Automatyzacja Instalacji Serwera WWW przy użyciu Ansible

## Opis

Projekt ten demonstruje użycie Ansible do automatycznej instalacji i konfiguracji serwera WWW (Nginx) na maszynie wirtualnej z systemem Ubuntu 24.04.3.

Zadanie realizuje następujące kroki:
1.  Instaluje serwer Nginx przy użyciu gotowej roli z Ansible Galaxy.
2.  Tworzy prostą stronę `index.html` z napisem "Hello world".

## Wymagania

*   **Maszyna hostująca Ansible (w tym projekcie Ubuntu Server 24.04.3.):**
    *   Zainstalowany Ansible (`apt install ansible`).
    *   Zainstalowany Git do sklonowania repozytorium.
    *   Dostęp sieciowy (SSH) do docelowej maszyny wirtualnej (projekt zakłada połączenie przez SSH przy użyciu kluczy).
*   **Serwer docelowy (Ubuntu):**
    *   Maszyna wirtualna z zainstalowanym Ubuntu 24.04.3.
    *   Użytkownik z uprawnieniami do `sudo`.

## Instalacja i Uruchomienie

1.  **Sklonuj repozytorium:**
    ```bash
    git clone https://github.com/kamil-iskra/ansible_www_srv.git
    cd ansible_www_srv
    ```

2.  **Skonfiguruj inwentarz:**
    Otwórz plik `inventory.ini` i w sekcji `[servers]` zmień adres IP `ansible_host` na adres IP Twojej maszyny wirtualnej. Upewnij się również, że `ansible_user` jest poprawny.

3.  **Pobierz zewnętrzną rolę Ansible:**
    Projekt używa roli `geerlingguy.nginx` z Ansible Galaxy. Aby ją pobrać, wykonaj polecenie:
    ```bash
    ansible-galaxy install -r requirements.yml
    ```

4.  **Zapis hasła root'a przy pomocy Ansible Vault:**
    Utwórz zaszyfrowany plik `passwd.yml` przy pomocy ansible-vault:
    ```bash
    ansible-vault create passwd.yml
    ```
    Przypisz do zmiennej `ansible_sudo_pass` hasło root'a:
    ```bash
    ansible_sudo_pass: <hasło>
    ```

5.  **Uruchom playbook:**
    Wykonaj główny playbook, aby przeprowadzić konfigurację serwera:
    ```bash
    ansible-playbook -i inventory.ini --ask-vault-pass playbook.yml
    ```

## Weryfikacja

Po pomyślnym wykonaniu playbooka możesz zweryfikować działanie strony na dwa sposoby:

1.  **Z linii komend używając `curl`:**
    ```bash
    curl http://<IP_TWOJEJ_VM>
    ```

2.  **Używając przeglądarki internetowej:**
    Wpisz w pasku adresu `http://<IP_TWOJEJ_VM>`. 

W obu przypadkach powinieneś zobaczyć stronę z napisem "Hello world".