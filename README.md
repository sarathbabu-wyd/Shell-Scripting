#!/bin/bash

echo "===== SERVER PERFORMANCE STATS ====="
echo "Generated on: $(date)"
echo

# OS and Kernel Info
echo ">> OS and Kernel Information:"
uname -a
echo

# Uptime and Load Average
echo ">> Uptime and Load Average:"
uptime
echo

# Logged in users
echo ">> Logged in Users:"
who
echo

# CPU Usage
echo ">> Total CPU Usage:"
top -bn1 | grep "Cpu(s)" | \
  awk '{print "Used: " $2 + $4 "% | Idle: " $8 "%"}'
echo

# Memory Usage
echo ">> Memory Usage:"
free -h | awk 'NR==2{printf "Used: %s | Free: %s | Usage: %.2f%%\n", $3, $4, $3*100/$2 }'
echo

# Disk Usage
echo ">> Disk Usage:"
df -h --total | grep 'total' | \
  awk '{print "Used: " $3 " | Free: " $4 " | Usage: " $5 }'
echo

# Top 5 processes by CPU
echo ">> Top 5 Processes by CPU Usage:"
ps -eo pid,comm,%cpu --sort=-%cpu | head -n 6
echo

# Top 5 processes by Memory
echo ">> Top 5 Processes by Memory Usage:"
ps -eo pid,comm,%mem --sort=-%mem | head -n 6
echo

# Failed login attempts (requires root or sudo)
echo ">> Failed Login Attempts (last 10):"
journalctl _COMM=sshd | grep "Failed password" | tail -10
echo

echo "===
