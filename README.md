# DEPRECATED

# Intro
You can use the following steps to install an agent that will run on one of your Kubernetes clusters to collect audit flow logs from your IBM Cloud Account. These audit flow logs are stored in your Cloud Object Storage (COS) instance. Then, you can enable Security Advisor Activity Insights to analyze your activity flow logs to detect and alert you to suspicious activity.

# Versions 
- `v2.0`(**DEPRECATED**) : Follow the steps to [install](https://github.com/ibm-cloud-security/security-advisor-activity-insights/tree/master/v2.0#prerequisites) `v2.0`. This is supported on clusters with version "">=1.10.3" and <= v1.15.10". It is supported only on Helm v2
- `v3.0` : New customer are advised to install this version. It supports Kuberenetes version ">=v1.16.7" with Helm v3. Helm v2 is not supported. Installation steps can be found [here](https://github.com/ibm-cloud-security/security-advisor-activity-insights/tree/master/v3.0#prerequisites)


# Troubleshooting

Run diagnostics to check if there is any connectivity issues with respect to IAM, Activity Tracker or COS from within the pod.

Download the tool from the release based on your machine architecture. We support linux, windows and mac(darwin) (amd64).

Before running any of the command make sure you have targeted your kubectl to the cluster where you deployed the activity insights. If you don't have kubectl you will need kube config file.


Make the tool executable
```
chmod +x ./si-diagnostics-1.0-linux-amd64
```

Run the diagnostics
```
./si-diagnostics-1.0-linux-amd64
```

If due to some reason you don't have kubectl but you do have kube config
```
./si-diagnostics-1.0-linux-amd64 --kubeconfig <path to your kube config file>
```

If you want to check any custom URL connectivity you can pass url paramter
```
./si-diagnostics-1.0-linux-amd64 -url https://www.ibm.com
```

You can also capture the logs by passing the last n line of logs to capture. Default is last 500 lines
```
./si-diagnostics-1.0-linux-amd64 -lines 500
```

Once the diagnostic is complete it will emit a file si-diagnostics.txt in the same directory from where you ran the tool.
Attach this file to the IBM support ticket. Make sure the file doesn't have any API keys etc.
The tool only sends a curl request to each of IBM iam , cos and activity tracker URLs to test the connectivity. So we don't
expect any sensitive information in the txt. The logs which are captured by default ( 500 lines) are also not expected to contain
any sensitive information.

