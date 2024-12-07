### Notes on Kubernetes CKAD Preparation Video (Lecture 4)

#### General Tips for CKAD Exam
1. **Preparation Strategy:**
   - Practice is key; don’t just watch the videos—write and execute commands yourself.
   - Repeat the questions multiple times to solidify command knowledge.

2. **Exam Success Tips:**
   - Use Kubernetes cheat sheets for quick command references.
   - Ensure you are working in the correct Kubernetes context during the exam.

---

#### Question 9: Retrieve OS Image of All Nodes in JSON Path Format
- **Scenario Details:**
  - Retrieve OS images of all nodes.
  - Save the results in a file named `all-nodes-os-info.txt` at the root directory.

- **Steps to Solve:**
  1. Inspect the JSON structure of node information:
     ```bash
     kubectl get nodes -o json
     ```
     - Locate the path: `status.nodeInfo.osImage`.

  2. Extract the OS images using a JSONPath query:
     ```bash
     kubectl get nodes -o jsonpath='{range .items[*]}{.status.nodeInfo.osImage}{"\n"}{end}' > /root/all-nodes-os-info.txt
     ```

  3. Verify the contents of the file:
     ```bash
     cat /root/all-nodes-os-info.txt
     ```

---

#### Question 10: Create a Persistent Volume
- **Scenario Details:**
  - **Volume Name:** `pv-demo`
  - **Storage:** 100 MB
  - **Access Mode:** `ReadWriteMany`
  - **Host Path:** `/pv/host-data`

- **Steps to Solve:**
  1. Open the Kubernetes documentation to reference Persistent Volume examples.
  2. Create a YAML file (`pv-demo.yaml`) with the following content:
     ```yaml
     apiVersion: v1
     kind: PersistentVolume
     metadata:
       name: pv-demo
     spec:
       capacity:
         storage: 100Mi
       accessModes:
         - ReadWriteMany
       hostPath:
         path: /pv/host-data
     ```
  3. Apply the Persistent Volume configuration:
     ```bash
     kubectl apply -f pv-demo.yaml
     ```
  4. Verify the creation:
     ```bash
     kubectl get pv
     ```

---

#### Question 11: Debug a Non-Responding Node
- **Scenario Details:**
  - **Node Name:** `node01`
  - Debug and resolve issues preventing the node from responding.

- **Steps to Solve:**
  1. Check the status of all nodes:
     ```bash
     kubectl get nodes
     ```
  2. SSH into the problematic node:
     ```bash
     ssh node01
     ```
  3. Verify if the kubelet is running:
     ```bash
     ps aux | grep kubelet
     ```
     - If the kubelet is not running, check logs for errors:
       ```bash
       journalctl -xe | grep kubelet
       ```
  4. Inspect the kubelet configuration file:
     ```bash
     vim /etc/kubernetes/kubelet.conf
     ```
     - Verify paths like `--client-cert` and `--client-key` are correct.

  5. Restart the kubelet service:
     ```bash
     systemctl restart kubelet
     ```
  6. Confirm the kubelet is running:
     ```bash
     ps aux | grep kubelet
     ```
  7. Check the node status from the control plane:
     ```bash
     kubectl get nodes
     ```

---

#### Key Exam Techniques
1. **Use JSONPath Queries:**
   - JSONPath is essential for extracting specific details from large JSON outputs.
   - Familiarize yourself with common paths like `status.nodeInfo`.

2. **Persistent Volume Creation:**
   - Always use YAML files for PV creation as imperative commands are not supported.
   - Understand and practice PV specifications from the documentation.

3. **Node Debugging:**
   - Learn to troubleshoot kubelet issues, including configuration errors and service status.

---

#### Key Takeaways
- Retrieving specific details using JSONPath is a common scenario in the CKAD exam.
- Persistent Volume creation is a guaranteed topic—be comfortable with writing YAML definitions.
- Debugging non-responding nodes involves SSH access, kubelet inspection, and log analysis.

#### Next Steps
- Continue practicing these and other Kubernetes scenarios.
- Watch the series multiple times and simulate exam-like conditions during practice.
- Review CKAD tips and cheat sheets before the exam.
