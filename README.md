
# How to Set Up Red Hat OpenShift AI on Red Hat OpenShift

## Red Hat OpenShift AI
Red Hat OpenShift AI is a comprehensive artificial intelligence (AI) platform offering tools to quickly develop, train, deploy, and monitor machine learning models across on-premises environments, public clouds, and edge locations.
It leverages container orchestration for scalability and efficiency. This tutorial focuses on preparing and installing the OpenShift AI operator, including GPU enablement and storage configuration.

For this guide, we have a 5-node Red Hat OpenShift cluster setup with two worker nodes, each with an NVIDIA A30 GPU (each) installed. 


## In this article
We will look at installing Red Hat OpenShift AI
 - Install and configure the following Red Hat OpenShift Operators
	 - Node Feature Discovery Operator
	 - NVIDIA GPU Operator
	 - Serverless Operator
	 - Service Mesh Operator
	 - Red Hat Authorino Operator
	 - Red Hat OpenShift AI Operator
 - Validate the installation
	 - Validate the installation 

## Prerequisite
 - Access to Red Hat OpenShift Cluster with GPUs available
 - Cluster administrator access to your OpenShift cluster
 - oc CLI tool for accessing the cluster from the command line
 - Internet Access


## Installation
In this section, we will install the various Red Hat OpenShift Operators to set up Red Hat OpenShift AI

#### Node Feature Discovery Operator
The Node Feature Discovery (NFD) Operator automates feature discovery and labeling for OpenShift cluster nodes. It runs on and scans each node's hardware and software configurations, adding labels for features like the NVIDIA GPU PCIe vendor ID. These labels optimize resource allocation and help identify suitable nodes for specific workloads, enhancing cluster performance.

Follow the below steps to install the NFD or any other operator in general

 1. Login to **Red Hat OpenShift web console**
 2. Navigate to **Operators --> Operator Hub**
 3. In the search box, type **Node Feature** 
 4. Select the one provided by **Red Hat** as shown in the screenshot below 
 5. Click **Install** to bring up the Install Operator Screen 
 6. Leave all the values at default and click **Install**

![Install Node Feature Discovery Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/NFDOut.gif)

#### Red Hat OpenShift Serverless, ServiceMesh, and Authorino Operator
To enable the KServe component used by the single-model serving platform for large models, we need to install the Red Hat OpenShift Serverless and Red Hat OpenShift Service Mesh Operators.

-   **KServe**: A Kubernetes CRD for model orchestration, runtime management, deployment lifecycle, storage access, and networking.
-   **Red Hat OpenShift Serverless**: Enables serverless model deployments using Knative.
-   **Red Hat OpenShift Service Mesh**: Manages traffic flows and access policies with Istio. 
- **Authorino**: To integrate an authorization provider with the single-model serving platform, we need to install the 'Red Hat - Authorino' Operator.

Follow the below steps to install the all the above operator

 1. Login to **Red Hat OpenShift web console**
 2. Navigate to **Operators --> Operator Hub**
 3. In the search box, type the name of the corresponding operator 
 4. Select the one provided by **Red Hat** as shown in the screenshots below 
 5. Click **Install** to bring up the Install Operator Screen 
 6. Leave all the values at default and click **Install**

Red Hat OpenShift Serverless
![Install Serverless Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/ServerlessOut.gif)

Red Hat OpenShift ServiceMesh
![Install Service Mesh Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/ServiceMeshOut.gif)

Red Hat Authorino 
![Install Authorino Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/AuthorinoOut.gif)

#### Nvidia GPU Operator
The NVIDIA GPU Operator automates the management of NVIDIA GPUs on Kubernetes clusters, handling provisioning, configuration, and monitoring. It supports NVIDIA GPUDirect for direct GPU communication, enhancing performance, and simplifying GPU management in Kubernetes environments.

Follow the below steps to install the Nvidia GPU operator

 1. Login to **Red Hat OpenShift web console**
 2. Navigate to **Operators --> Operator Hub**
 3. In the search box type type Nvidia 
 4. Select the one provided by **Red Hat** as shown in the screenshots below 
 5. Click **Install** to bring up the Install Operator Screen 
 6. Leave all the values at default and Click **Install**

![Install NVIDIA GPU Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/NvidiaGPUOperatorOut.gif)

Once the NVIDIA GPU Operator is installed we will need to create an instance of the ClusterPolicy, follow the steps below to configure the Nvidia GPU Operator

 1. Once the operator installation is complete click on **View Operator** or Navigate to **Operators --> Installed Operators --> NVIDIA GPU Operator**
 2. Under the **Cluster Policy** card click **Create Instance** 
 3. In the **Create Instance** screen accept the defaults and click **Create** 
 4. Next check the NVIDIA GPU operator installation progress by looking at the  pods running as shown in the screenshot  
 5. It should take about 15-20 minutes, depending on the number of worker nodes

![Create GPU Cluster Policy](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/NvidiaOperatorConfigOut01.gif)


 6. Once all the Pods are up and running navigate to the **nvidia-driver-deamonset-. . . . .** (This pod is available for each worker node)
 7. Go to the terminal for the pod and run the **nvidia-smi** command to display the details about the GPUs available on that node as shown in the screenshot. The NVIDIA System Management Interface (nvidia-smi) is a command-line utility that monitors and manages NVIDIA GPU devices.

![NVIDIA SMI](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/NvidiaOperatorConfigOut02.gif)

#### Red Hat OpenShift AI Operator
Red Hat OpenShift AI is a scalable MLOps platform that supports the full AI/ML lifecycle, enabling teams to build, deploy, and manage AI applications both on-premises and in the cloud. Previously known as OpenShift Data Science, it leverages open-source technologies for innovation and consistency. Installation is now possible.

Follow the below steps to install the Nvidia GPU operator

 1. Login to **Red Hat OpenShift web console**
 2. Navigate to **Operators --> Operator Hub**
 3. In the search box type Red Hat OpenShift AI 
 4. Select the one provided by **Red Hat** as shown in the screenshots below 
 5. Click **Install** to bring up the Install Operator Screen 
 6. Leave all the values at default and Click **Install**

![Install RHOAI Operator](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/RHOAIOperatorOut01.gif)

Next, we will configure the Red Hat OpenShift AI Operator using the following steps:

 1. Navigate to **Operators --> Installed Operators**
 2. Click **Red Hat OpenShift AI Operator**
 3. On the Details Page that Opens up click on **Create Instance** 
 4. Use the default values and click **Create**
 5. Navigate to the **Data Science Cluster** tab to monitor the progress of the cluster under the Status column
 6. You can also see the status of other components under the DSC Initialization and the Feature Tracker tabs
 8. At this point, please wait for the DataScience Cluster to be provisioned

![DataScience Cluster Progress](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/RHOAIOperatorOut03.gif)

Once the Data Science Cluster is successfully installed:

 1. A new menu item **Red Hat OpenShift AI** is available under the squares Icon on the top right, as shown in the screenshot below.
 2. Click it to navigate to the login screen for Red Hat OpenShift AI
 3. Click Login with OpenShift to log in
 4. After logging in, you will land on the Red Hat OpenShift AI home screen

You have successfully installed Red Hat OpenShift AI

![RHOAI Login](https://raw.githubusercontent.com/rohitralhan/InstallRHOAI/refs/heads/main/images/RHOAILoginOut.gif)

## **Conclusion**

By following these steps, you can successfully set up Red Hat OpenShift AI on OpenShift, enabling a scalable and enterprise-ready AI/ML development platform. Whether running notebooks, training models, or deploying inference services, OpenShift AI provides the necessary tools for your AI workloads.

## References
For more detailed instructions, refer to:
 - [Red Hat OpenShift AI - Install & Deploy](https://docs.redhat.com/en/documentation/red_hat_openshift_ai_self-managed/2.16/html/installing_and_uninstalling_openshift_ai_self-managed/installing-and-deploying-openshift-ai_install)
