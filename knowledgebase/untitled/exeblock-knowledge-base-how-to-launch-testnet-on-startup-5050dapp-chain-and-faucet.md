# EXEBlock Knowledge Base : How to launch Testnet on startup: 5050dapp, chain and faucet

1. Download attachments
2. Place start-testnet.sh and start-tain.sh into /home/exeblock/ folder
3. chmod u+x both scripts
4. Copy start-testnet.service to /etc/system.d/system folder
5. chmod u+x the service
6. Enable the service: sudo systemctl enable start-testnet.service
7. Start the service: sudo systemctl start start-testnet.service
8. Check the status: sudo systemctl status start-testnet.service
9. Restart the server: sudo reboot
10. Make sure 5050dapp, chain and faucet are running.

