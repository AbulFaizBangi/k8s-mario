# Kubernetes Mario Deployment with Terraform on AWS
### Demo Preview 🎮

![Mario Game Deployment](ScreenRecording2025-01-29at11.50.57AM-ezgif.com-speed.gif)

## Prerequisites
Ensure you have the following set up before proceeding:
- An Ubuntu Instance
- IAM Role with necessary permissions
- Terraform installed on the instance
- AWS CLI and Kubectl installed on the instance

---

## Step 1: Launch Ubuntu Instance
1. **Sign in to AWS Console**: Log in to your AWS Management Console.
2. **Navigate to EC2 Dashboard**: Go to "Services" > "EC2" under the Compute section.
3. **Launch Instance**: Click on "Launch Instance."
4. **Choose an Amazon Machine Image (AMI)**: Select an Ubuntu AMI.
5. **Choose an Instance Type**: Select `t2.micro`.
6. **Configure Instance Details**:
   - Set "Number of Instances" to `1`.
   - Configure network, subnets, IAM role as needed.
   - Modify storage to `8GB`.
7. **Add Tags (Optional)**: Helps with instance organization.
8. **Configure Security Group**:
   - Use an existing security group or create a new one.
   - Ensure necessary inbound/outbound rules are set.
9. **Review and Launch**:
   - Select or create a key pair.
   - Acknowledge access to the key pair.
   - Click "Launch Instances."
10. **Access the Instance**: Use SSH with the key pair and instance’s public IP or DNS.

---

## Step 2: Create IAM Role
1. **Search for IAM in AWS Console** and go to "Roles."
2. **Click on Create Role.**
3. **Select Entity Type**: AWS Service.
4. **Use Case**: Select EC2 and click "Next."
5. **Attach Permissions**:
   - Select `AdministratorAccess` (for learning purposes only).
6. **Provide a Name** and click "Create Role."
7. **Attach Role to EC2 Instance**:
   - Go to EC2 Dashboard, select the instance.
   - Click on "Actions" > "Security" > "Modify IAM Role."
   - Select the created role and update IAM role.
8. **Connect to Instance** using MobaXterm or PuTTY.

---

## Step 3: Cluster Provisioning
1. **Clone the repository**:
   ```sh
   git clone https://github.com/Aj7Ay/k8s-mario.git
   ```
2. **Navigate into the repository**:
   ```sh
   cd k8s-mario
   ```
3. **Provide executable permissions and run the script**:
   ```sh
   sudo chmod +x script.sh
   ./script.sh
   ```
   - This installs Terraform, AWS CLI, Kubectl, and Docker.
4. **Verify installations**:
   ```sh
   docker --version
   aws --version
   kubectl version --client
   terraform --version
   ```
5. **Navigate to EKS-TF directory**:
   ```sh
   cd EKS-TF
   ```
6. **Initialize Terraform**:
   ```sh
   terraform init
   ```
   - Ensure you update the S3 bucket name in `backend.tf`.
7. **Validate and Plan Terraform Execution**:
   ```sh
   terraform validate
   terraform plan
   ```
8. **Apply Terraform to Provision Cluster**:
   ```sh
   terraform apply --auto-approve
   ```
   - This process takes about 10 minutes.
9. **Update Kubernetes Configuration**:
   ```sh
   aws eks update-kubeconfig --name EKS_CLOUD --region us-west-2
   ```

---

## Step 4: Deploy Mario Game
1. **Return to k8s-mario directory**:
   ```sh
   cd ..
   ```
2. **Apply Deployment and Service**:
   ```sh
   kubectl apply -f deployment.yaml
   kubectl get all
   kubectl apply -f service.yaml
   kubectl get all
   ```
3. **Get Service Details**:
   ```sh
   kubectl describe service mario-service
   ```
   - Copy the LoadBalancer Ingress URL and paste it into your browser to play the Mario game.

---

## Step 5: Destroying the Setup
1. **Delete Services and Deployments**:
   ```sh
   kubectl delete service mario-service
   kubectl delete deployment mario-deployment
   ```
2. **Destroy the Cluster**:
   ```sh
   terraform destroy --auto-approve
   ```
   - Resources will be removed within 10 minutes.

---

Enjoy playing the Mario game while learning Kubernetes on AWS!

