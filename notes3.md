### Notes on Kubernetes CKAD Preparation Video (Lecture 3)

#### General Tips for CKAD Exam
1. **Practice is Essential:**
   - Watching videos alone won’t guarantee success; practice every question thoroughly.
   - Use tools like [Killer Coda](https://killercoda.com) for hands-on experience.

2. **Resource Utilization:**
   - Leverage the Kubernetes cheat sheet for quick reference during preparation and the exam.
   - Regularly validate knowledge by solving scenario-based questions.

---

#### Question 6: Create a Pod with Multiple Containers
- **Scenario Details:**
  - **Pod Name:** `pod-multi`
  - **Containers:**
    - Container 1: `name=container1`, `image=nginx`.
    - Container 2: `name=container2`, `image=busybox`, `command=sleep 4800`.

- **Steps to Solve:**
  1. Generate a base YAML file:
     ```bash
     kubectl run pod-multi --image=nginx --dry-run=client -o yaml > pod-multi.yaml
     ```
  2. Edit the YAML file to include the second container:
     ```yaml
     containers:
     - name: container1
       image: nginx
     - name: container2
       image: busybox
       command: ["sleep", "4800"]
     ```
  3. Apply the updated YAML file:
     ```bash
     kubectl apply -f pod-multi.yaml
     ```
  4. Verify the pod creation:
     ```bash
     kubectl get pods
     kubectl describe pod pod-multi
     ```

---

#### Question 7: Create a Pod in a Custom Namespace with Labels
- **Scenario Details:**
  - **Namespace:** `custom`
  - **Pod Name:** `test-pod`
  - **Image:** `nginx:1.17`
  - **Labels:**
    - `env=test`
    - `tier=backend`

- **Steps to Solve:**
  1. Create the custom namespace:
     ```bash
     kubectl create namespace custom
     ```
  2. Generate a base YAML file for the pod:
     ```bash
     kubectl run test-pod --image=nginx:1.17 --namespace=custom --labels=env=test --dry-run=client -o yaml > test-pod.yaml
     ```
  3. Edit the YAML file to add the second label:
     ```yaml
     labels:
       env: test
       tier: backend
     ```
  4. Apply the YAML file:
     ```bash
     kubectl apply -f test-pod.yaml
     ```
  5. Verify the pod creation:
     ```bash
     kubectl get pods -n custom
     kubectl describe pod test-pod -n custom
     ```

---

#### Question 8: Get Node Information in JSON Format
- **Scenario Details:**
  - **Node Name:** `node01`
  - **Output Format:** JSON
  - **File Name:** `node-info.json`

- **Steps to Solve:**
  1. Get the node information and save it in JSON format:
     ```bash
     kubectl get node node01 -o json > node-info.json
     ```
  2. Verify the file creation and contents:
     ```bash
     cat node-info.json
     ```

---

#### Exam Preparation Techniques
1. **Use Documentation and Cheat Sheets:**
   - Commands for YAML generation, namespace creation, and JSON output are frequently used and should be memorized or referenced.
   - Validate syntax using the Kubernetes cheat sheet.

2. **Hands-On Practice:**
   - Focus on creating and managing multi-container pods, custom namespaces, and retrieving specific resource details.

3. **Efficiency with Dry-Run:**
   - Use the `--dry-run=client` flag to generate YAML templates for customization, saving time during the exam.

---

#### Key Takeaways
- Multi-container pods are created by modifying the YAML definition after generating it with an imperative command.
- Custom namespaces and labels help in resource organization and are critical for exam scenarios.
- Retrieving resource information in specific formats (e.g., JSON) is a straightforward but essential skill.

#### Next Steps
- Review the Kubernetes cheat sheet for additional commands and scenarios.
- Continue practicing with more complex questions to build confidence for the CKAD exam.
