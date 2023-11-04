# Prometheus Operatorについて

## 概要

Prometheus Operatorは、Prometheusや関連する監視コンポーネントを管理やKubernetesネイティブなデプロイメントを提供します。
このプロジェクトの目的は、KubernetesクラスターのPrometheusベースの監視スタックの設定を簡素化し、自動化することにあります。

Prometheus Operatorには以下の特徴があります。

Kubernetesカスタムリソース：Kubernetesのカスタムリソースを使用して、PrometheusやAlertmanager、関連するコンポーネントをデプロイし、管理します。

簡素化されたデプロイメント設定：Prometheusの基本設定であるバージョン、永続性、保持ポリシー、KubernetesリソースのReplicaなどを設定することができます。

Prometheusターゲット設定：Prometheus固有の言語を学ぶ必要なく、Kubernetesラベルクエリに基づいて監視ターゲット設定を自動的に生成します。

![image](https://prometheus-operator.dev/img/architecture.png)

### メトリクスの収集

メトリクスを収集するために、Prometheus Operator は `ServiceMonitor`や`PodMonitor`を使用して、監視対象のサービスを指定します。

これにより、CPUやメモリ使用率、HTTPリクエスト数、レイテンシーなどのメトリクスを追跡できます。

#### 前提

ここでは、repricaが3つでport`8080`で公開されているアプリケーションを参考に説明していきます。

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: example-app
  template:
    metadata:
      labels:
        app: example-app
    spec:
      containers:
      - name: example-app
        image: fabxc/instrumented_app
        ports:
        - name: web
          containerPort: 8080
---
kind: Service
apiVersion: v1
metadata:
  name: example-app
  labels:
    app: example-app
spec:
  selector:
    app: example-app
  ports:
  - name: web
    port: 8080
```

### ServiceMonitorの設定

`ServiceMonitor`オブジェクトは、サービスのエンドポイントからメトリクスを収集することができます。

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: example-app
  labels:
    team: frontend
spec:
  selector:
    matchLabels:
      app: example-app
  endpoints:
  - port: web
```

### PodMonitorの設定

`PodMonitor`オブジェクトは、個々のポッドから直接メトリクスを収集することができます。

```yaml
apiVersion: monitoring.coreos.com/v1
kind: PodMonitor
metadata:
  name: example-app
  labels:
    team: frontend
spec:
  selector:
    matchLabels:
      app: example-app
  podMetricsEndpoints:
  - port: web
```

![image](https://prometheus.io/assets/grafana_prometheus.png)

## 参考文献

https://prometheus-operator.dev/