# EnvoyGateway Example - Hello World (English & Português)

## English

### Overview

This repository provides an example of deploying a simple **hello-world application** behind **Envoy Gateway** using Kubernetes Gateway API. It demonstrates how to install Envoy Gateway and expose an application using Gateway, HTTPRoute, and supporting Kubernetes resources.

---

## Prerequisites

* A running **Kubernetes cluster**
* `kubectl` configured to access your cluster
* Helm installed (required by `install-envoy.sh`)

---

## Installation Sequence

The installation follows the exact order of the files provided in the repository.

### 1. Install Envoy Gateway

The installation script located in `install/install-envoy.sh` installs Envoy Gateway via Helm:

```bash
./install/install-envoy.sh
```

This executes the following Helm command:

```bash
helm install eg oci://docker.io/envoyproxy/gateway-helm --version v1.6.0 -n envoy-gateway-system --create-namespace
```

---

### 2. Apply GatewayClass

File: `install/gatewayclass.yaml`

```bash
kubectl apply -f install/gatewayclass.yaml
```

This defines the GatewayClass that Envoy Gateway will use.

---

### 3. Deploy the Application

Apply the namespace, deployment, and service for the example app.

#### Namespace

File: `apps/namespace.yaml`

```bash
kubectl apply -f apps/namespace.yaml
```

#### Deployment

File: `apps/deployment.yaml`

```bash
kubectl apply -f apps/deployment.yaml
```

#### Service

File: `apps/service.yaml`

```bash
kubectl apply -f apps/service.yaml
```

The service is internal, and Envoy Gateway will expose it externally.

---

### 4. Create the Gateway

File: `apps/gateway.yaml`

```bash
kubectl apply -f apps/gateway.yaml
```

This creates the Envoy Gateway instance listening on port **80**.

---

### 5. Create the HTTPRoute

File: `apps/httproute.yaml`

```bash
kubectl apply -f apps/httproute.yaml
```

This HTTPRoute attaches to the Gateway and routes traffic to the `hello-world` service on port **8081**.

---

## Testing

Once all resources are applied and Envoy Gateway has provisioned an external load balancer, obtain its IP:

```bash
kubectl get gateway -A
```

Then test:

```bash
curl http://<EXTERNAL-IP>/
```

Expected output:

```
hello world
```

---

## Português (Brazil)

### Visão Geral

Este repositório contém um exemplo de como implantar uma aplicação **hello-world** atrás do **Envoy Gateway** utilizando o Kubernetes Gateway API. Ele demonstra como instalar o Envoy Gateway e expor a aplicação usando Gateway, HTTPRoute e demais recursos Kubernetes.

---

## Pré-requisitos

* Um **cluster Kubernetes** funcional
* `kubectl` configurado
* Helm instalado (necessário pelo script `install-envoy.sh`)

---

## Sequência de Instalação

A instalação segue exatamente a ordem dos arquivos fornecidos.

### 1. Instalar o Envoy Gateway

O script `install/install-envoy.sh` instala o Envoy Gateway via Helm:

```bash
./install/install-envoy.sh
```

O comando executado é:

```bash
helm install eg oci://docker.io/envoyproxy/gateway-helm --version v1.6.0 -n envoy-gateway-system --create-namespace
```

---

### 2. Aplicar o GatewayClass

Arquivo: `install/gatewayclass.yaml`

```bash
kubectl apply -f install/gatewayclass.yaml
```

Define o GatewayClass usado pelo Envoy Gateway.

---

### 3. Deploy da Aplicação

Aplicar namespace, deployment e service da aplicação.

#### Namespace

Arquivo: `apps/namespace.yaml`

```bash
kubectl apply -f apps/namespace.yaml
```

#### Deployment

Arquivo: `apps/deployment.yaml`

```bash
kubectl apply -f apps/deployment.yaml
```

#### Service

Arquivo: `apps/service.yaml`

```bash
kubectl apply -f apps/service.yaml
```

O service é interno; o Envoy Gateway fará a exposição externa.

---

### 4. Criar o Gateway

Arquivo: `apps/gateway.yaml`

```bash
kubectl apply -f apps/gateway.yaml
```

Cria o Gateway que escuta na porta **80**.

---

### 5. Criar o HTTPRoute

Arquivo: `apps/httproute.yaml`

```bash
kubectl apply -f apps/httproute.yaml
```

Associa o HTTPRoute ao Gateway e direciona tráfego para o serviço `hello-world` na porta **8081**.

---

## Testes

Após a criação dos recursos e provisionamento do Load Balancer, obtenha o IP externo:

```bash
kubectl get gateway -A
```

Testar:

```bash
curl http://<IP-EXTERNO>/
```

Saída esperada:

```
hello world
```
