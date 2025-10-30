# ngrok Kubernetes Operator - KubeCon Demo Suite

This repository contains three standalone demos showcasing different capabilities of the ngrok Kubernetes Operator.

## Demo Structure

### 📁 Demo 1: Gateway API (`gateway-api-demo/`)
Modern Kubernetes networking with Gateway API

Shows how ngrok integrates with the Kubernetes Gateway API standard to provide:
- Public HTTP/HTTPS endpoints
- Advanced routing and traffic management
- Standard, portable configuration

---

### 📁 Demo 2: ngrok CRDs + Ingress (`ngrok-crds-demo/`)
Flexible public exposure with Ingress and native CRDs

Demonstrates multiple ways to expose services publicly:
- Standard Kubernetes Ingress (easiest migration)
- ngrok native CRDs (maximum control)
- Traffic policies for security and management

---

### 📁 Demo 3: Kubernetes Bindings (`bindings-demo/`)
Private cross-cluster communication without VPNs

Showcases secure cluster-to-cluster connectivity:
- Private (not public) service projection
- No VPN or network configuration needed
- Native Kubernetes service discovery
- Works across any cloud or on-premises
