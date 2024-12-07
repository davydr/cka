### Transcript for the Video on Adding a User in Kubernetes

#### Introduction
0:00 - **Introduction to the Scenario:**
- Discussing a question about creating a Kubernetes user and assigning access with specific permissions.
- Question **#14**: Create a user named Ajit with permissions to create, list, get, update, and delete Pods.
- High-probability question in the CKA exam.

---

### Steps to Solve the Question

#### **1. Generate Private Key**
- Use `OpenSSL` to create a private key for the user:
  ```bash
  openssl genrsa -out /root/ajit.key 2048
  ```

---

#### **2. Generate Certificate Signing Request (CSR)**
- Create a CSR file for the user:
  ```bash
  openssl req -new -key /root/ajit.key -out /root/ajit.csr
  ```
  - You can leave optional fields (country, state, etc.) blank if not required.

---

#### **3. Base64 Encode the CSR**
- Base64 encode the content of the CSR file:
  ```bash
  cat /root/ajit.csr | base64 | tr -d "\n"
  ```
- Replace the `request` field in the following YAML with the Base64-encoded CSR:
  ```yaml
  apiVersion: certificates.k8s.io/v1
  kind: CertificateSigningRequest
  metadata:
    name: ajit
  spec:
    groups:
    - system:authenticated
    request: <BASE64_ENCODED_CSR>
    signerName: kubernetes.io/kube-apiserver-client
    usages:
    - client auth
  ```

- Save this as `ajit-csr.yaml`.

---

#### **4. Apply the CSR**
- Submit the CSR to the cluster:
  ```bash
  kubectl apply -f ajit-csr.yaml
  ```

- Check the CSR:
  ```bash
  kubectl get csr
  ```

---

#### **5. Approve the CSR**
- Approve the CSR:
  ```bash
  kubectl certificate approve ajit
  ```

- Verify the approval:
  ```bash
  kubectl get csr ajit -o yaml
  ```

---

#### **6. Extract the Certificate**
- Extract the certificate and save it:
  ```bash
  kubectl get csr ajit -o jsonpath='{.status.certificate}' | base64 --decode > /root/ajit.crt
  ```

---

#### **7. Create Role and RoleBinding**
- Create a role with the required permissions:
  ```bash
  kubectl create role developer \
    --verb=create,get,list,update,delete \
    --resource=pods
  ```

- Bind the role to the user:
  ```bash
  kubectl create rolebinding developer-binding-ajit \
    --role=developer \
    --user=ajit
  ```

---

#### **8. Add the User to kubeconfig**
- Add credentials to the kubeconfig:
  ```bash
  kubectl config set-credentials ajit \
    --client-certificate=/root/ajit.crt \
    --client-key=/root/ajit.key
  ```

- Add a context for the user:
  ```bash
  kubectl config set-context ajit-context \
    --cluster=kubernetes \
    --user=ajit
  ```

- Switch to the user's context:
  ```bash
  kubectl config use-context ajit-context
  ```

---

#### **9. Verify User Access**
- Check the current context:
  ```bash
  kubectl config get-contexts
  ```
- Test permissions (e.g., create a Pod):
  ```bash
  kubectl auth can-i create pods
  ```

---

### Key Notes
- Practice the CSR process, as it can involve creating and encoding files.
- Ensure role and rolebinding match the scenario requirements (resources and permissions).
- Use Kubernetes documentation as a reference during the exam.

---

### Conclusion
- This scenario is important and commonly asked in CKA exams.
- Practicing these steps ensures quick and accurate execution during the exam.
- Feedback from users confirms that similar questions are frequently included in the exam.

#### Closing:
- Stay subscribed and practice with Kubernetes documentation to improve speed and accuracy.
