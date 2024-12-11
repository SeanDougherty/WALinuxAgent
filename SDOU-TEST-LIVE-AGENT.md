To test live agent:

1. provision an Azure Linux 3.0 VM in Azure
2. From the ~/ dir of your VM, clone this repo and checkout this branch
3. `cd WALinuxAgent`
4. open a second terminal and also ssh into this vm
5. from the second terminal, run `sudo journalctl -f -u waagent` to follow (-f) the unit's (-u) logs. Keep this up on a separate monitor while we work.
6. from the first terminal, where you are cd'd into WALinuxAgent: `python3 setup.py build`
7. After this succeeds run `python3 setup.py install --force`
8. You have now updated all of the live VM's waagent files, we now need to restart the service so the processes use these new files. `sudo systemctl restart waagent`
9. Watch your second terminal to see updates get printed to log.
10. To test changes, update the files in `~/WALinuxAgent` and then run steps 6-8.
11. To test adding an extension, `az vm extension set --resource-group %YOUR_RG% --vm-name %YOUR_VM% --name customScript --publisher Microsoft.Azure.Extensions --settings '{"commandToExecute": "echo \"hello world\""}'`
12. To test removing an extension, `az vm extension delete --resource-group %YOUR_RG% --vm-name %YOUR_VM% --name customScript`