### Notes on Kubernetes CKAD Preparation Video (Lecture 2)

#### General Tips for CKAD Exam
1. **Consistency in Preparation:**
   - 90% of the questions covered in this series are likely to appear in the exam.
   - Build confidence by solving questions incrementally, starting from simpler ones.

2. **Tools and Resources:**
   - Use [Killer Coda](https://killercoda.com) playground for practice.
   - Always keep the Kubernetes cheat sheet open during preparation and the exam.

---

#### Question 3: Create a Deployment with Replicas
- **Scenario Details:**
  - **Deployment Name:** `my-deployment`
  - **Image:** `nginx` (or any image of choice).
  - **Replicas:** 3 (ensure the desired number of pods are always running).

- **Steps to Solve:**
  1. Use an imperative command to create the deployment:
     ```bash
     kubectl create deployment my-deployment --image=nginx --replicas=3
     ```
  2. Verify deployment and pods:
     ```bash
     kubectl get deployments
     kubectl get pods
     ```

---

#### Question 4: Create a Pod with a Specific Label
- **Scenario Details:**
  - **Pod Name:** `web-nginx`
  - **Image:** `nginx:1.17`
  - **Label:** `tier=web-app`

- **Steps to Solve:**
  1. Use the `kubectl run` command:
     ```bash
     kubectl run web-nginx --image=nginx:1.17 --labels=tier=web-app
     ```
  2. Verify pod creation and labels:
     ```bash
     kubectl get pods
     kubectl get pods --show-labels
     ```

---

#### Question 5: Create a Static Pod
- **Scenario Details:**
  - **Node Name:** `node01`
  - **Pod Name:** `static-pod`
  - **Image:** `nginx`
  - Ensure the pod is recreated automatically in case of failure.

- **Understanding Static Pods:**
  - Bound to a specific node and managed directly by the kubelet.
  - Configured via static YAML files placed in the kubelet's `--pod-manifest-path`.

- **Steps to Solve:**
  1. SSH into the specified node:
     ```bash
     ssh node01
     ```
  2. Locate the kubelet configuration to find the static pod manifest directory:
     ```bash
     ps aux | grep kubelet
     ```
     - Look for the `--config` file path, then locate `--pod-manifest-path` in the configuration file.

  3. Navigate to the manifest directory (e.g., `/etc/kubernetes/manifests`):
     ```bash
     cd /etc/kubernetes/manifests
     ```
  4. Create the static pod YAML file:
     ```bash
     vim static-pod.yaml
     ```
     - Example YAML content:
       ```yaml
       apiVersion: v1
       kind: Pod
       metadata:
         name: static-pod
       spec:
         containers:
         - name: nginx-container
           image: nginx
       ```
  5. Save the file and exit the node:
     ```bash
     exit
     ```
  6. Verify the static pod from the control plane:
     ```bash
     kubectl get pods
     ```

---

#### Exam Preparation Techniques
1. **Documentation and Cheat Sheet:**
   - Search for relevant commands and definitions during the exam.
   - Use imperative commands to generate YAML files when needed.

2. **Practice:**
   - Regularly simulate scenarios like deployments, labeling, and static pod creation.

3. **Static Pods:**
   - Understand their use cases and differences from regular pods.
   - Familiarize yourself with kubelet configuration and static pod manifests.

---

#### Key Takeaways
- Simple questions (like scaling deployments or labeling pods) often appear in the exam with slight variations.
- Static pods are less common in daily use but are crucial for CKAD exam scenarios.
- Practice every question thoroughly, as these are highly likely to appear in the actual test.

#### Next Steps
- Review the Kubernetes cheat sheet.
- Continue solving more complex questions in upcoming videos.
