# Intro
You can use the following steps to install an agent that will run on one of your Kubernetes clusters to collect audit flow logs from your IBM Cloud Account. These audit flow logs are stored in your Cloud Object Storage (COS) instance. Then, you can enable Security Advisor Activity Insights to analyze your activity flow logs to detect and alert you to suspicious activity.

# Prerequisites
- For Windows 10, Before starting with steps mentioned above, activate WSL (Windows Subsystem for Linux) and install [Ubuntu shell](https://win10faq.com/install-run-ubuntu-bash-windows-10/)
- yq CLI version 4.0.0 or higher
  - For MacOS, Windows 10: Install [yq CLI](http://mikefarah.github.io/yq/)
  - For CentOS, Red Hat and Ubuntu : Install yq CLI version 4.4.1 by running the folowing commands:      
  `wget https://github.com/mikefarah/yq/releases/download/v4.4.1/yq_linux_amd64.tar.gz`       
  `mv yq_linux_amd64 yq`     
  `chmod +x yq`     
  `mv yq /usr/local/bin/`       
  `yq -V`       
- cURL binary
  - For CentOS and Red Hat, update cURL binary using `yum update -y nss curl libcurl`
- Install [Kubernetes CLI (kubectl)](https://kubernetes.io/docs/tasks/tools/install-kubectl/) v1.10.11 or higher
- Install [Kubernetes Helm (package manager)](https://docs.helm.sh/using_helm/#from-script) v3.1.0 or higher
- Export [KUBECONFIG](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/#the-kubeconfig-environment-variable)  or perform any other necessary steps to make sure you can connect to your cluster via `kubectl` command.

# Steps to run
1. Download the tar file.
2. Unzip the file by running `tar -xvf security-advisor-activity-insights.tar` in your terminal.
3. Change into the file directory. `cd security-advisor-activity-insights`
4. Run the install command. 
    ```
    ./activity-insight-install.sh -c <cos_region> -k <cos_api_key> -b <cos_bucket> -a <at_region> -s <at_service_api_key> -m <default_memory_request> -l <memory_limit> -n <namespace>
    ```
     - The `<cos_region>` value is the region in which your COS is deployed. Options include `us-south` or `eu-gb`.
     - The `<cos_api_key>` value is the [api key](https://cloud.ibm.com/docs/services/cloud-object-storage/iam?topic=cloud-object-storage-service-credentials#service-credentials) that you created to access your COS instance and bucket. The key should have the Writer IAM service role.
     - The `<cos_bucket>` value is the name of bucket you created in COS istance.
     - The `<at_region>` the region of the logDNA AT instance, example: us-south (https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-regions).  
     - The `<at_service_api_key>` is a logDNA AT [service key](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-service_keys#service_keys_create) for your logDNA AT instance.
     - Optional : `<default_memory_request>` and `<memory_limit>`.  
     `<default_memory_request>` is default memory requested at time of POD creation.  
     `<memory_limit>` is maximum memory which will be provided to POD by cluster.  
     `<namespace>` is the Kubernetes namespace to install activity-insights-chart. Default: `security-advisor-activity-insights`  
     In case values are not provided, default memory 256Mi and memory limit of 512Mi is implemented.

5. Verify the installation :
     - `helm ls --namespace <namespace>` should list `activity-insights` as a release.
     - `kubectl get pods -n <namespace> | grep activity-insights` should return one pod related to `activity-insights` in the state "RUNNING".                   
6. Take the rule packages and upload to your cos bucket. A default set of rule packages can be found [here](https://cloud.ibm.com/docs/services/security-advisor?topic=security-advisor-setup-activity#activity-adding-rules).

# Deleting the setup
1. `helm uninstall activity-insights --namespace <namespace>`
2. `kubectl delete ns <namespace>`
