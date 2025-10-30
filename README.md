# ngrok Kubernetes Operator - KubeCon Demo Suite

This repository contains three standalone demos showcasing different capabilities of the ngrok Kubernetes Operator. Each demo is self-contained with all necessary files and detailed scripts.

## Demo Structure

Each demo is self-contained in its own directory with all necessary files and a detailed demo flow.

### 📁 Demo 1: Gateway API (`gateway-api-demo/`)
**Duration:** 5-7 minutes  
**Focus:** Modern Kubernetes networking with Gateway API

Shows how ngrok integrates with the Kubernetes Gateway API standard to provide:
- Public HTTP/HTTPS endpoints
- Advanced routing and traffic management
- Standard, portable configuration

**Best for:**
- Teams adopting Gateway API (the future of K8s networking)
- Platform engineers building self-service platforms
- Organizations wanting standards-based solutions

---

### 📁 Demo 2: ngrok CRDs + Ingress (`ngrok-crds-demo/`)
**Duration:** 5-7 minutes  
**Focus:** Flexible public exposure with Ingress and native CRDs

Demonstrates multiple ways to expose services publicly:
- Standard Kubernetes Ingress (easiest migration)
- ngrok native CRDs (maximum control)
- Traffic policies for security and management

**Best for:**
- Teams migrating from existing Ingress controllers
- Use cases needing maximum flexibility
- Organizations wanting both simplicity AND power

---

### 📁 Demo 3: Kubernetes Bindings (`bindings-demo/`)
**Duration:** 10-12 minutes  
**Focus:** Private cross-cluster communication without VPNs

Showcases secure cluster-to-cluster connectivity:
- Private (not public) service projection
- No VPN or network configuration needed
- Native Kubernetes service discovery
- Works across any cloud or on-premises

**Best for:**
- Multi-cluster architectures
- Hybrid cloud deployments
- Dev → Staging/Prod connectivity
- Organizations avoiding VPN complexity

---

## Quick Start

### Prerequisites
- Docker Desktop or similar (for kind)
- kubectl installed
- Helm 3 installed
- ngrok account (free tier available at ngrok.com)

### Get Your Credentials
1. Sign up at [ngrok.com](https://ngrok.com)
2. Get your authtoken from [dashboard](https://dashboard.ngrok.com/get-started/your-authtoken)
3. Get your API key from [dashboard](https://dashboard.ngrok.com/api)

### Choose Your Demo

**Want to show public exposure?**
- Start with **Demo 2** (CRDs/Ingress) - easiest
- Or **Demo 1** (Gateway API) - most modern

**Want to show private connectivity?**
- Go straight to **Demo 3** (Bindings) - unique capability

**Want to show everything?**
- Run all three demos in sequence (25-30 minutes total)

---

## Demo Comparison

| Feature | Gateway API | CRDs/Ingress | Bindings |
|---------|-------------|--------------|----------|
| **Exposure** | Public | Public | Private |
| **Primary Use** | Modern routing | Flexible exposure | Cross-cluster |
| **Complexity** | Medium | Low-Medium | Medium |
| **Migration Path** | Gateway API adopters | Ingress users | VPN replacement |
| **Unique Value** | Standards-based | Familiar + powerful | No VPN needed |
| **Best Audience** | Platform engineers | App developers | Architects |

---

## Full Demo Sequence (If Time Permits)

### Part 1: Public Exposure (10-12 minutes)
Run Demo 2 OR Demo 1 to show public service exposure.

**Key Message:** "ngrok makes it trivially easy to expose services with enterprise features."

### Part 2: Private Connectivity (10-12 minutes)
Run Demo 3 to show cross-cluster bindings.

**Key Message:** "ngrok enables secure cluster-to-cluster communication without VPN complexity."

### Part 3: Wrap-up (3-5 minutes)
Show how both capabilities work together:

```
┌─────────────┐  Public      ┌─────────────┐
│   Ingress   │ ◄─────────── │   Internet  │
│   (ngrok)   │              │   Clients   │
└──────┬──────┘              └─────────────┘
       │
       ▼
┌─────────────────┐
│  Frontend (GCP) │
└────────┬────────┘
         │ Private Binding (kubernetes)
         ▼
┌──────────────────┐
│  Backend (AWS)   │
└────────┬─────────┘
         │ Private Binding (kubernetes)
         ▼
┌──────────────────┐
│  DB (On-Prem)    │
└──────────────────┘
```

**Key Message:** "One operator, two powerful capabilities - public exposure AND private connectivity."

---

## Common Questions

### "Which demo should I start with?"

**For developers:** Demo 2 (Ingress) - most familiar

**For platform engineers:** Demo 1 (Gateway API) - future-proof

**For architects:** Demo 3 (Bindings) - solves hard problems

**For sales/business:** Start with problem statement, then Demo 3 (biggest differentiator)

### "Can I combine these?"

Yes! Most production deployments use both:
- **Ingress/Gateway API** for public-facing services
- **Bindings** for internal cross-cluster communication

### "What if I only have 10 minutes?"

**Option A:** Demo 2 (Ingress) - shows value quickly

**Option B:** Demo 3 (Bindings) - shows unique capability

Skip Demo 1 (Gateway API) unless audience specifically cares about Gateway API standard.

---

## Troubleshooting

### Clusters won't start
```bash
# Check Docker is running
docker ps

# Delete and recreate
kind delete cluster --name <cluster-name>
kind create cluster --config <config-file>
```

### Operator pods not ready
```bash
# Check logs
kubectl logs -n ngrok-operator -l app=ngrok-operator-manager --tail=50

# Common issue: Invalid credentials
# Solution: Double-check NGROK_API_KEY and NGROK_AUTHTOKEN
```

### Service not accessible (Demo 3)
```bash
# Check AgentEndpoint
kubectl get agentendpoint -n demo -o yaml

# Check BoundEndpoint
kubectl get boundendpoints -n ngrok-operator

# Check operator logs
kubectl logs -n ngrok-operator -l app.kubernetes.io/component=bindings-forwarder
```

### Ingress not working (Demo 2)
```bash
# Check Ingress status
kubectl get ingress -n demo -o yaml

# Check for events
kubectl describe ingress <ingress-name> -n demo

# Check operator logs
kubectl logs -n ngrok-operator -l app=ngrok-operator-manager | grep ingress
```

---

## Demo Best Practices

### Before the Demo
- [ ] Test run the demo end-to-end
- [ ] Verify all clusters start successfully
- [ ] Check credentials are valid
- [ ] Prepare whiteboard/slides for architecture diagrams
- [ ] Set terminal font size large (18-20pt)
- [ ] Close unnecessary applications
- [ ] Have ngrok dashboard open

### During the Demo
- [ ] Explain what you're doing BEFORE running commands
- [ ] Pause after each output to let audience read
- [ ] Use `| head` to limit long output
- [ ] Point out key information in output
- [ ] Ask "Does this make sense?" after key concepts
- [ ] Invite questions throughout
- [ ] Don't rush - better to skip content than rush

### After the Demo
- [ ] Show ngrok dashboard (if possible)
- [ ] Discuss pricing and next steps
- [ ] Offer POC or architecture review
- [ ] Share documentation links
- [ ] Get feedback

---

## Customization

### Add Your Branding
Edit the HTML in deployment ConfigMaps:
- `gateway-api-demo/gcp-app-deployment.yaml`
- `ngrok-crds-demo/gcp-app-deployment.yaml`  
- `bindings-demo/gcp-app-deployment.yaml`
- `bindings-demo/aws-app-deployment.yaml`

### Use Your Domain
Edit Ingress/Gateway resources to use your custom domain:
```yaml
spec:
  rules:
  - host: demo.yourcompany.com
```

### Adjust Timing
Each demo script has time estimates. Adjust based on:
- Audience technical level
- Time available
- Questions expected
- Depth desired

---

## Support Resources

### Documentation
- [ngrok Kubernetes Operator](https://ngrok.com/docs/k8s)
- [Kubernetes Bindings Guide](https://ngrok.com/docs/k8s/guides/bindings)
- [Traffic Policies](https://ngrok.com/docs/http/traffic-policy)
- [Troubleshooting](https://ngrok.com/docs/k8s/troubleshooting)

### Examples
- [GitHub Examples](https://github.com/ngrok/ngrok-operator/tree/main/examples)

### Community
- [ngrok Slack Community](https://ngrok.com/slack)
- [GitHub Discussions](https://github.com/ngrok/ngrok-operator/discussions)

### Getting Help
- Community: Slack and GitHub
- Customers: support@ngrok.com
- Sales: sales@ngrok.com

---

## Contributing

Found an issue or have a suggestion? Please:
1. Test the issue is reproducible
2. Document the steps and expected vs actual behavior
3. Share feedback with your ngrok contact or via Slack

---

## License

These demos are provided as-is for demonstration and educational purposes.

ngrok Kubernetes Operator is licensed separately. See [ngrok.com/tos](https://ngrok.com/tos) for terms.

---

## Quick Reference

### Install ngrok Operator
```bash
helm repo add ngrok https://charts.ngrok.com
helm install ngrok-operator ngrok/ngrok-operator \
  --namespace ngrok-operator \
  --create-namespace \
  --set credentials.apiKey=$NGROK_API_KEY \
  --set credentials.authtoken=$NGROK_AUTHTOKEN \
  --set bindings.enabled=true  # Only for Demo 3
```

### Check Operator Status
```bash
kubectl get pods -n ngrok-operator
kubectl get crd | grep ngrok
```

### Get ngrok Resources
```bash
# Ingress
kubectl get ingress -A

# Gateway API
kubectl get gateway,httproute -A

# ngrok CRDs
kubectl get agentendpoint,cloudendpoint -A

# Bindings
kubectl get boundendpoints -n ngrok-operator
```
