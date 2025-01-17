# Observability-Project-on-EKS-Cluster
Implementation of Observability on EKS Cluster

There are three components of observability
1. Monitoring
2. Logs
3. Traces

Monitoring is used to understand what the issue is, by seeing different metrices like CPU Utilization, Memory Utilization, http request count, response time etc. We will use Prometheus for collecting metrices and Grafana for visualising those metrices.

Logs are used to understand why the issue has hapened by seeing logs of that issue. We aill use EFK(Elastic Search, Fluentbit & Kibana) stack for logging.

Traces are used to understand how the issue has happened. We will use Jaeger for collecting the traces.

Prometheus : Prometheus has three components node exporter, kube-state metrics, custom metrics.
             Node exporter collcets node related information and works as a deamon set.
             Kubestate-metrics collects pod, deployment and replicas related information works as a K8's API server.
             custom metrics is used to create custom metrices
             It stores all the metrices on TSDB (Time Series Database)

Grafana : Grafana is used to visualise dashboards
          We will use PromQL query to fetch the custom metrices from Prometheus
          Grafana uses the API server to fetch the information metrices

**Step 1 : Installing Kube-prometheus stack using Helm : **

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

helm repo update

**Step 2 : Deploy the chart into a new namespace "monitoring": **

kubectl create ns monitoring

helm install monitoring prometheus-community/kube-prometheus-stack \
-n monitoring

**Step 3 : Check the installation**
kubectl get all -n monitoring

Prometheus UI:
kubectl port-forward service/prometheus-operated -n monitoring 9090:9090

**Note : you need to pass --address 0.0.0.0 to the above command. Then you can access the UI on instance-ip:port**

Grafana UI: password is prom-operator

kubectl port-forward service/monitoring-grafana -n monitoring 8080:80

Alertmanager UI:

kubectl port-forward service/alertmanager-operated -n monitoring 9093:9093

