### Notes on Kubernetes CKAD Preparation Video

#### General Tips for CKAD Exam
1. **Preparation Strategy:**
   - Watch certification tips video for foundational strategies.
   - Solve all provided scenario questions to ensure passing the exam.
   - Use Kubernetes documentation effectively during the exam.

2. **Tools and Resources:**
   - Use the Kubernetes cheat sheet for quick reference.
   - Open necessary tabs like documentation and cheat sheet for multitasking.

3. **Environment Setup:**
   - Utilize tools like [Killer Coda](https://killercoda.com) for Kubernetes practice.
   - Killer Coda provides a playground for Kubernetes commands and scenarios relevant to the CKAD exam.

---

#### Question 1: Create a Pod with Specific Configuration
- **Scenario Details:**
  - **Pod Name:** `web-pod`
  - **Image:** `busybox`
  - **Command:** `sleep 3200`
  - **Capability:** Set system time (`SYS_TIME`).

- **Steps to Solve:**
  1. Run an imperative command to create the pod:
     ```bash
     kubectl run web-pod --image=busybox --command -- sleep 3200 --dry-run=client -o yaml > busybox.yaml
     ```
  2. Edit the generated `busybox.yaml` to add `SYS_TIME` capability:
     - Use `securityContext`:
       ```yaml
       securityContext:
         capabilities:
           add: ["SYS_TIME"]
       ```
  3. Apply the YAML file:
     ```bash
     kubectl apply -f busybox.yaml
     ```
  4. Verify the pod creation:
     ```bash
     kubectl get pods
     kubectl describe pod web-pod
     ```

---

#### Question 2: Create and Update a Deployment
- **Scenario Details:**
  - **Deployment Name:** `my-project`
  - **Image:** `nginx:1.16`
  - **Replica Count:** 1
  - **Update:** Change to `nginx:1.17` with rolling update and record history.

- **Steps to Solve:**
  1. Create the deployment:
     ```bash
     kubectl create deployment my-project --image=nginx:1.16
     ```
  2. Verify the deployment:
     ```bash
     kubectl get deployments
     kubectl describe deployment my-project
     ```
  3. Update the image version with rolling update and record:
     ```bash
     kubectl set image deployment/my-project nginx=nginx:1.17 --record
     ```
  4. Check the deployment status and history:
     ```bash
     kubectl get deployments
     kubectl describe deployment my-project
     kubectl rollout history deployment/my-project
     ```

---

#### Key Exam Preparation Techniques
1. **Documentation Use:**
   - Search for specific topics like "securityContext" or "set image" in the documentation.
   - Refer to examples directly from the official docs to save time.

2. **Practice:**
   - Focus on hands-on practice to familiarize with imperative commands and YAML configurations.
   - Regularly simulate scenarios and troubleshoot errors.

3. **Cheat Sheet Reference:**
   - Keep commands like `kubectl run`, `kubectl create deployment`, and `kubectl set image` handy.
   - Utilize dry-run outputs to generate base YAML files for custom edits.

---

#### Final Tips
- Stick to the documentation for guidance during the exam.
- Practice frequently to minimize the need for searching commands during the test.
- Upcoming videos in the series will cover additional CKAD exam questions.
