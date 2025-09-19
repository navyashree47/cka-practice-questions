# Helm-practice-questions

1. One application, webpage-server-01, is currently deployed on the Kubernetes cluster using Helm. A new version of the application is available in a Helm chart located at /root/new-version.
Validate this new Helm chart, then install it as a new release named webpage-server-02. After confirming the new release is installed, uninstall the old release webpage-server-01.

    Solution:
      In this task, we will use the helm commands. Here are the steps:
      Use the helm ls command to list the Helm releases in the default namespace.
    
        helm ls -n default

      Validate the Helm chart using the helm lint command:
    
        cd /root/
        helm lint ./new-version

      Install the new version of the application as a new release named webpage-server-02:

        helm install webpage-server-02 ./new-version

      Uninstall the old release webpage-server-01 using the following command:

        helm uninstall webpage-server-01 -n default
