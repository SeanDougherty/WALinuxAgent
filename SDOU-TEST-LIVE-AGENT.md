## Testing the Live Agent

### Prerequisites:
- Provision an Azure Linux 3.0 VM in Azure

### Steps:

1. **Clone the Repository:**
   - SSH into your Azure VM.
   - Navigate to the `~` directory and clone the repository. Checkout the appropriate branch:
     ```sh
     git clone https://github.com/SeanDougherty/WALinuxAgent
     cd WALinuxAgent
     git checkout traced
     ```

2. **Monitor the Agent Logs:**
   - Open a second terminal and SSH into the same VM.
   - In the second terminal, run the following command to follow the agent logs:
     ```sh
     sudo journalctl -f -u waagent
     ```
   - Keep this terminal open on a separate monitor for real-time log updates.

3. **Build and Install the Agent:**
   - In the first terminal, after navigating to the `WALinuxAgent` directory:
   - Build the project:
     ```sh
     python3 setup.py build
     ```
   - Once the build succeeds, install the agent:
     ```sh
     python3 setup.py install --force
     ```
   - Restart the waagent service to apply the new files:
     ```sh
     sudo systemctl restart waagent
     ```

4. **Verify the Changes:**
   - Watch the second terminal for log updates to verify the waagent service restart.

5. **Test Changes:**
   - To make and test changes, update the files in the `~/WALinuxAgent` directory.
   - Repeat steps 3 to 4.

6. **Test Extensions:**

   - **Add an Extension:**
     ```sh
     az vm extension set --resource-group <YOUR_RESOURCE_GROUP> --vm-name <YOUR_VM_NAME> --name customScript --publisher Microsoft.Azure.Extensions --settings '{"commandToExecute": "echo \"hello world\""}'
     ```
   
   - **Remove an Extension:**
     ```sh
     az vm extension delete --resource-group <YOUR_RESOURCE_GROUP> --vm-name <YOUR_VM_NAME> --name customScript
     ```

Replace `<YOUR_RESOURCE_GROUP>`, and `<YOUR_VM_NAME>` with the actual values.