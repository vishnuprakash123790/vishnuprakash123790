1. System health monitoring script (using python)

import psutil
import os
from datetime import datetime

# Thresholds for alerts
CPU_THRESHOLD = 80  # percentage
MEMORY_THRESHOLD = 80  # percentage
DISK_THRESHOLD = 80  # percentage

# Log file
LOG_FILE = "system_health.log"

def log_message(message):
    timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
    with open(LOG_FILE, 'a') as f:
        f.write(f"{timestamp} - {message}\n")
    print(message)

def check_cpu_usage():
    cpu_usage = psutil.cpu_percent(interval=1)
    if cpu_usage > CPU_THRESHOLD:
        log_message(f"ALERT: High CPU usage detected: {cpu_usage}%")
    return cpu_usage

def check_memory_usage():
    memory = psutil.virtual_memory()
    memory_usage = memory.percent
    if memory_usage > MEMORY_THRESHOLD:
        log_message(f"ALERT: High memory usage detected: {memory_usage}%")
    return memory_usage

def check_disk_usage():
    disk = psutil.disk_usage('/')
    disk_usage = disk.percent
    if disk_usage > DISK_THRESHOLD:
        log_message(f"ALERT: High disk usage detected: {disk_usage}%")
    return disk_usage

def check_running_processes():
    process_count = len(psutil.pids())
    log_message(f"INFO: Number of running processes: {process_count}")
    return process_count

def monitor_system():
    log_message("Starting system health monitoring...")
    cpu_usage = check_cpu_usage()
    memory_usage = check_memory_usage()
    disk_usage = check_disk_usage()
    process_count = check_running_processes()

    log_message(f"CPU Usage: {cpu_usage}%")
    log_message(f"Memory Usage: {memory_usage}%")
    log_message(f"Disk Usage: {disk_usage}%")
    log_message(f"Processes: {process_count}")

if _name_ == "_main_":
    monitor_system()


 2. Application health checker script(using python)

 import requests
 from  datetime import datetime
 
 # Application URL
 APP_URL = "http://example.com"
 
 # Log file
 LOG_FILE = "application_health.log"
 
 def log_message (message):
     timestamp  = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
     with open (LOG_FILE, 'a') as f:
         f.write (f"{timestamp} - {message}\n")
     print (message)
 
 def check_application_status():
     try:
         response = requests.get(APP_URL, timeout=5)
         if response.status_code == 200:
             log_message(f"INFO: Application is UP. Status code: {response.status_code}")
         else:
             log_message(f"WARNING: Application might be DOWN. Status code: {response.status_code}")
     except requests.exceptions.RequestException as e:
         log_message(f"ERROR: Application is DOWN. Error: {e}")
 
 if _name_  == "_main_":
     check_application_status() 

 
