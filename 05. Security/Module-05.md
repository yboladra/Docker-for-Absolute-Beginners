# 05. Security in Docker

Security in Docker involves measures to safeguard containers, the Docker environment, and underlying infrastructure. 
Key aspects include:

1. **Container Isolation**: Ensuring each container operates in its isolated space, preventing unauthorized access.

2. **Image Verification**: Validating images' authenticity and integrity to avoid tampering.

3. **Host Security**: Securing the Docker host through updates, firewalls, user access restrictions, and log monitoring.

4. **Runtime Security**: Implementing mechanisms like AppArmor and SELinux to enforce security policies.

5. **Network Security**: Utilizing firewalls, network segmentation, and encryption to protect data in transit.

6. **Image Scanning**: Employing continuous scanning tools to identify vulnerabilities in images.

7. **Access Control**: Managing user permissions with RBAC to restrict unauthorized access.

8. **Secrets Management**: Safeguarding sensitive data with encryption and access control mechanisms.

9. **Logging and Monitoring**: Monitoring container activities for detecting and responding to security incidents.

These measures collectively contribute to enhancing the security posture of Docker environments and mitigating potential risks.

## Container Security Components

## Secure Images

### Start Minimal
- Command: `docker build -t myimage:latest --from ubuntu:minimal` (replace `ubuntu:minimal` with your minimal base image).
- Starting with minimal base images reduces attack surface and potential vulnerabilities.

### Scan Images with WhiteSource Bolt
- Integrate WhiteSource Bolt as an extension in your Azure DevOps pipeline.
- WhiteSource Bolt scans code for vulnerabilities and license compliance issues in your application and its dependencies.

### Scan Images with Trivy
- Integrate Trivy into your Azure DevOps pipeline using a task runner like bash:
```yaml
steps:
- script: |
    bash -c 'apk add --no-cache trivy'
    trivy image myimage:latest
```
Trivy scans container images for vulnerabilities in OS packages and application dependencies.

## Control Access

## RBAC in AKS
- Command: `az aks role assignment create --cluster-name myakscluster --role Contributor --assignee user@domain.com`
- Role-Based Access Control (RBAC) allows fine-grained control over access to Azure Kubernetes Service (AKS) resources.

## Minimize Privileges
- Command (Dockerfile):
```dockerfile
RUN useradd -r restricteduser
RUN su - restricteduser
```
Creating and switching to a non-root user within the container minimizes the impact of potential security breaches.

## Runtime Security

## Docker Content Trust (DCT)
- Implement DCT signing with tools like `ctr` or `docker trust` to ensure image integrity and authenticity.

## Monitor Containers
- Command:
```bash
az monitor log queries -g myresourcegroup -w myakscluster --query ContainerInsights | where Name contains "Security"
```
Query security-related container logs in Azure Monitor to monitor runtime security.

### CI/CD Pipeline Integration

### Scan in Azure DevOps
- Use the "Scan container image in Azure Container Registry" task or integrate WhiteSource Bolt and Trivy using pipeline tasks.

### Fail on Vulnerabilities
 Configure build pipelines to fail if vulnerabilities are detected during image scanning.
  Here's an example configuration in Azure DevOps:
  ```yaml
#Azure DevOps Pipeline Configuration
#Description: Builds a Docker image and scans it for vulnerabilities using WhiteSource Bolt.
#Fail the pipeline if any high severity vulnerabilities are detected.
trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: Docker@2
  inputs:
    containerRegistry: 'YourContainerRegistryConnection'  # Replace with your container registry connection name
    repository: 'YourDockerRepository'  # Replace with your Docker repository name
    command: 'build'
    Dockerfile: '**/Dockerfile'
    tags: 'latest'

- task: WhiteSourceBolt@20
  inputs:
    scanType: 'docker'
    dockerImageName: 'YourDockerImageName'  # Replace with your Docker image name
    failBuildOnVulnerability: true  # Fail the build if any vulnerabilities are detected
    severityThreshold: 'high'  # Set severity threshold to fail on high severity vulnerabilities
```
## Additional Considerations

## Secrets Management
- Utilize Azure Key Vault to manage sensitive credentials and configuration data within containers securely.

## Network Security
- Implement network policies in AKS or container firewalls to restrict container network traffic and enhance security.

## Continuous Monitoring
Continuously monitor container environments for suspicious activity to detect and respond to security incidents promptly.

By incorporating these secure image practices, access control measures, runtime security, and robust CI/CD pipeline integration with tools like WhiteSource and Trivy, you can establish a comprehensive container security strategy in Azure and Azure DevOps. Remember to tailor the specific commands and tools to your environment and security requirements.


## Alright, see you in the next module.