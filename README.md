# kubernetes-dashboard-v2.0.0-beta4
how to install kubernetes dashboard version 2.0.0-beta4 in Ubuntu 18.04

1. kubectl apply -f https://github.com/RobyYasirAmri/kubernetes-dashboard-v2.0.0-beta4/blob/master/kubernetes-dashboard.yaml

2. kubectl get pods --all-namespaces

3. kubectl -n kubernetes-dashboard edit service kubernetes-dashboard
equalize:

    externalTrafficPolicy: Cluster
    ports:
        - nodePort: 30000  ==> change if you want to use another port
          port: 443
          protocol: TCP
          targetPort: 8443
    selector:
      k8s-app: kubernetes-dashboard
    sessionAffinity: None
    type: NodePort ==> change from ClusterIP to NodePort

4. See the port has changed to 30000 => #kubectl -n kube-system get service kubernetes-dashboard

5. create service account => #kubectl create serviceaccount cluster-admin-dashboard-sa

6. Create role => #kubectl create clusterrolebinding cluster-admin-dashboard-sa --clusterrole=cluster-admin --serviceaccount=default:cluster-admin-dashboard-sa

7. See secret => #kubectl get secret | grep cluster-admin-dashboard-sa

8. Describe secret for dashboard login  => #kubectl describe secret cluster-admin-dashboard-sa-token-xxxx

9. Open Dashboard via Firefox https://XXX.XXX.XXX.XXX:30000
