# 🚀 Platform GitOps Lab

Repositorio GitOps para la plataforma DevOps basada en:

- Kubernetes (K3s)
- ArgoCD (GitOps)
- Harbor (Container Registry)
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
Applications (nginx, future apps)


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
│       └── service.yaml
│
├── argocd/                  # Definición de Applications
│   └── nginx-harbor-application.yaml
│
└── README.md
⚙️ Flujo GitOps
Se realiza un cambio en Git
ArgoCD detecta el cambio
ArgoCD sincroniza automáticamente el cluster
Kubernetes aplica los manifests

👉 NO usar kubectl apply manualmente en producción

📦 Aplicación: nginx-harbor

Despliegue de ejemplo usando imagen almacenada en Harbor:

image: 192.168.1.24/platform/nginx:v1

Incluye:

Deployment
Service (NodePort)
imagePullSecrets
🔐 Secret de Harbor

⚠️ Los secrets NO están versionados en Git.

Debe crearse manualmente:

kubectl create namespace platform

kubectl create secret docker-registry harbor-creds \
  -n platform \
  --docker-server=192.168.1.24 \
  --docker-username=admin \
  --docker-password='Harbor12345' \
  --docker-email='admin@local'
🚀 Despliegue con ArgoCD

Aplicar la aplicación:

kubectl apply -f argocd/nginx-harbor-application.yaml -n argocd

Ver estado:

kubectl get applications -n argocd
🌐 Acceso a la app
kubectl get svc -n platform

Acceder vía:

http://<NODE_IP>:<NODE_PORT>
🧪 Validaciones
kubectl get pods -n platform
kubectl get svc -n platform
kubectl describe pod -n platform
📌 Roadmap
 Ingress con Traefik
 TLS (cert-manager)
 Keycloak SSO
 Grafana + Prometheus
 External Secrets / Vault
 Helm / Kustomize
 Multi-env (dev / uat / prod)
💡 Buenas prácticas
Git como única fuente de verdad
No subir secretos al repo
Usar namespaces por aplicación
Versionar imágenes (evitar latest)
Automatizar todo con ArgoCD
👨‍💻 Autor

Ruben Macchi
DevOps / Middleware / Kubernetes
