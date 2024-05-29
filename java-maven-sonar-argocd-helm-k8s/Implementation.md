In This document we will see both CI & CD process, CI using **Jenkins** and CD using **ArgoCD**

**CI using Jenkins**

1. Follow below link and install Jenkins on your EC2 instance.
   
   https://github.com/digupawar/Jenkins-Zero-To-Hero/blob/main/README.md

2. Follow below link and install Sonarqube on same EC2 instance.

   https://github.com/digupawar/Jenkins-Zero-To-Hero/blob/main/java-maven-sonar-argocd-helm-k8s/spring-boot-app/README.md

3. Build a CI pipeline using newly created Jenkins, add required creds in credential manager( ex- Sonarqube token, Github token, docker creds)

   https://github.com/digupawar/Jenkins-Zero-To-Hero/blob/main/java-maven-sonar-argocd-helm-k8s/spring-boot-app/JenkinsFile

   **Jenkins URL:** http://54.236.213.208:8080/job/ultimate-cicd/

   ![Jenkins-cicd](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/99846c61-dbd4-46d1-bdfa-69e38938236b)

4. Code quality analysis using Sonarqube.

   **Sonarqube URL:** http://54.236.213.208:9000/projects

   ![Sonarqube](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/7bc769e0-af53-451d-9015-358ccc40603a)
   

 **CD process using ArgoCD**

 Argo CD Installation

1. Install ArgoCD operator.
   
   https://operatorhub.io/operator/argocd-operator then click on **Install** tab

2. Install ArgoCD controller.

   https://argocd-operator.readthedocs.io/en/latest/usage/basics/

   Create a **argocd-basics.yml** file and copy below content.

   ```
       apiVersion: argoproj.io/v1alpha1
       kind: ArgoCD
       metadata:
         name: example-argocd
         labels:
           example: basic
       spec: {}
   ```

3. Deploy argocd-basics.yml resource.

   ```
      kubectl create -n argocd -f examples/argocd-basic.yaml
   ```

4. If you have created K8s cluster using **kOps**, then follow below steps to expose **ArgoCD** and use from UI.

   **Expose Argo CD via a LoadBalancer Service:**
   
   Expose the Argo CD server through a **LoadBalancer** service type. This way, AWS will provision an **external IP** for 
   you that you can access directly.

   First, edit the Argo CD server service to change its type to **LoadBalancer**:

   ``` 
    kubectl edit svc argocd-server -n argocd
   ```
   **Change the spec.type field to LoadBalancer:**
   ```
      spec:
        type: LoadBalancer
   ```
   * Save and exit the editor. AWS will provision an **external IP** address for this service, which you can find using:

   ```
      kubectl get svc -n argocd
   ```
   * Look for the **EXTERNAL-IP** of the argocd-server service. Use this IP address to access the Argo CD UI.
  
     ![argocd-externalip](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/6b67a84b-44e6-4e5a-8108-832adf9396ec)

   * Load balancer from AWS console:
  
     ![image](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/1e237f11-0a83-4cf9-b9d2-36bf46086497)


5. **ArgoCD URL:**
    https://a9f940ed3ed294e9797d2b2d5203e24e-1904878439.us-east-1.elb.amazonaws.com/

    username: admin
    password: We need to get it from ArgoCD secrets.

    ```
       kubectl get secret
       kubectl edit secret example-argocd-cluster
    ```
    
    From secrets file we get encoded secret value and that needs to be decoded like below.
    ```
       echo <encoded-secret> | base64 -d
    ```

6. Login to ArgoCD UI and add the details like Github repo url, manifest path, cluster, namespace etc then click on create, it will 
       take sometime to sync with our manifest file and deploy the application on K8s cluster.

      ![argocd-ui](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/37244605-b30e-4277-b004-4f4bf6f79366)



7. **Finally Application will be deployed on K8s cluster using ArgoCD.**

   Since we are running our K8s cluster on AWS using kOps so we used service type as **LoadBalancer** and we will get a external-ip to access the application.

   https://github.com/digupawar/Jenkins-Zero-To-Hero/blob/main/java-maven-sonar-argocd-helm-k8s/spring-boot-app-manifests/service.yml#L6

   ![application](https://github.com/digupawar/Jenkins-Zero-To-Hero/assets/72307345/01f28383-1c64-43c9-8a4f-5b988d4259e2)







   


   





   
