# 🚀 Platform GitOps Lab

Repositorio GitOps para una plataforma DevOps basada en:

- Kubernetes (K3s)
- ArgoCD (GitOps)
- Harbor (Container Registry - HTTP)
- Traefik (Ingress Controller)
- TLS (self-signed)
- Ansible (Provisioning)

---

## 🧠 Arquitectura

```text
GitHub (source of truth)
        ↓
ArgoCD (GitOps controller)
        ↓
Kubernetes (K3s cluster)
        ↓
Traefik (Ingress)
        ↓
Applications (nginx, futuras apps)
        ↑
Harbor (container registry)
📁 Estructura del repositorio
platform/
├── ansible/                  # Provisioning de infraestructura
│   ├── hosts.ini
│   ├── install_base.yml
│   └── install_k3s.yml
│
├── bootstrap/               # Bootstrapping del cluster
│   └── argocd/
│       └── argocd-nodeport.yaml
│
├── apps/                    # Aplicaciones gestionadas por ArgoCD
│   └── nginx-harbor/
│       ├── namespace.yaml
│       ├── deployment.yaml
│       ├── service.yaml
│       └── ingress.yaml
│
├── argocd/                  # Definición de Applications
│   └── nginx-harbor-application.yaml
│
├── certs/                   # (NO versionado) certificados TLS
├── .gitignore
└── README.md
⚙️ Flujo GitOps
Se realiza un cambio en Git
ArgoCD detecta el cambio
ArgoCD sincroniza automáticamente el cluster
Kubernetes aplica los manifests

👉 No usar kubectl apply manualmente en producción

📦 Aplicación: nginx-harbor

Despliegue de ejemplo usando imagen almacenada en Harbor:

image: 192.168.1.24/platform/nginx:v1

Incluye:

Deployment
Service (ClusterIP)
Ingress (Traefik)
TLS self-signed
🌐 Acceso a la aplicación
HTTP
http://nginx.platform.local:32266
HTTPS
https://nginx.platform.local:31788

⚠️ El certificado es self-signed, el navegador mostrará advertencia.

🔐 Certificado TLS (self-signed)

Generación:

openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout tls.key \
  -out tls.crt \
  -subj "/CN=nginx.platform.local/O=platform-lab"

Creación del secret:

kubectl create secret tls nginx-harbor-tls \
  -n platform \
  --cert=tls.crt \
  --key=tls.key
🔐 Secret de Harbor

⚠️ No versionado en Git

kubectl create namespace platform

kubectl create secret docker-registry harbor-creds \
  -n platform \
  --docker-server=192.168.1.24 \
  --docker-username=admin \
  --docker-password='Harbor12345' \
  --docker-email='admin@local'
🚀 Despliegue con ArgoCD

Aplicar Application:

kubectl apply -f argocd/nginx-harbor-application.yaml -n argocd

Ver estado:

kubectl get applications -n argocd
🔍 Validaciones
kubectl get pods -n platform
kubectl get svc -n platform
kubectl get ingress -n platform
kubectl describe ingress nginx-harbor -n platform
🧪 Pruebas
HTTP
curl http://nginx.platform.local:32266
HTTPS
curl -k https://nginx.platform.local:31788
🧠 Red del cluster
Internal network: 192.168.58.x
External network (bridge): 192.168.1.x

Configuración:

node-ip → red interna
node-external-ip → red externa
📌 Roadmap
 Ingress sin NodePort (puerto 80/443 limpio)
 cert-manager (TLS automático)
 Keycloak (SSO)
 Grafana + Prometheus
 External Secrets / Vault
 Helm / Kustomize
 Multi-entorno (dev / uat / prod)
 DNS real (en lugar de /etc/hosts)
💡 Buenas prácticas
Git como única fuente de verdad
No subir secretos al repositorio
Usar namespaces por aplicación
Versionar imágenes (evitar latest)
Automatizar todo con ArgoCD
👨‍💻 Autor

Ruben Macchi
DevOps / Middleware / Kubernetes
