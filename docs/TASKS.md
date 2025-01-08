# Tarefas

- [x] SETUP INICIAL

  - [x] Configurar pods que mostram 2 textos diferentes
  - [x] Criar deployments dos pods
  - [x] Criar services para os pods
  - [x] Acessar os pods via port-foward

- [x] INGRESS CONTROLLER

  - [x] NGINX INGRESS CONTROLLER

    - [x] Instalar o NGINX Ingress Controller
    - [x] Configurar TLS (com redirect de HTTP para HTTPS)
    - [ ] Configurar Ingress para estratégia de deploy Canário
    - [ ] Configurar Ingress para estratégia de deploy Blue/Green
    - [ ] Configurar Ingress para estratégia de deploy A/B Test

  - [x] ISTIO INGRESS CONTROLLER

    - [x] Instalar o Istio Ingress Controller
    - [x] Configurar TLS (com redirect de HTTP para HTTPS)
    - [x] Configurar Ingress para estratégia de deploy Canário
    - [x] Configurar Ingress para estratégia de deploy Blue/Green
    - [x] Configurar Ingress para estratégia de deploy A/B Test

- [x] GATEWAY API

  - [x] NGINX GATEWAY API

    - [x] Instalar NGINX Gateway API
    - [x] Configurar o Gateway
    - [x] Configurar os HTTPRoutes para as applicações v1 e v2
    - [x] Configurar o TLS para tráfegos HTTPs no Gateway e HttpRoutes
    - [x] Configurar HTTPRoutes para fazer redirect do HTTP para o HTTPS

  - [x] ISTIO GATEWAY API

    - [x] Instalar o ISTIO Gateway API
    - [x] Configurar o Gateway
    - [x] Configurar os HTTPRoutes para as applicações v1 e v2
    - [x] Configurar o TLS para tráfegos HTTPs no Gateway e HttpRoutes
    - [x] Configurar HTTPRoutes para fazer redirect do HTTP para o HTTPS
    - [x] Configurar HttpRoutes para estratégia de deploy Canário
    - [x] Configurar HttpRoutes para estratégia de deploy Blue/Green
    - [x] Configurar HttpRoutes para estratégia de deploy A/B Test
