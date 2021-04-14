# Desplegar genoit-istio-ansiblerole


## Resumen

Descargar el codigo ansible role del repositorio, configurar y ejecutar.

### Pasos


- El Bastion ANSIBLE debe tener instalado HELM, y configurado el KUBECTL con el cluster kubernetes a utilizar.

```raw
helm version
kubectl version
```

- El cluster KUBERNETES debe existir el NAMESPACE "istio-system".

```raw
kubectl create namespace istio-system
```
- Descargar y copiar el proyecto genoit-istio-ansiblerole al Bastion ANSIBLE.

```raw
git clone -b feature/istio codecommit::us-east-1://genoit@genoit-infrastructure-ansible
```
- Descargar y copiar el proyecto genoit-istio-helmchart al Bastion ANSIBLE, dentro de la carpeta helmchart del proyecto genoit-istio-ansiblerole.

```raw
- git clone -b feature/istio-helmchart codecommit::us-east-1://genoit@genoit-infrastructure-ansible
- Copiar:
    |-- genoit-istio-ansiblerole
        |-- helmchart
            |-- genoit-istio-helmchart
```
- Copiar los certificados en la carpeta files del proyecto genoit-istio-ansiblerole

```raw
- Copiar:
    |-- genoit-istio-ansiblerole
        |-- files
            |-- ca-cert.pem
            |-- ca-key.pem            
```

- Variables ANSIBLE ROLE 

```raw
- Modificar variables importantes en el archivo main.yaml
    |-- genoit-istio-ansiblerole
        |-- vars
            |-- main.yaml
        
- Todas las variables del HelmChart en el archivo main.yaml
    |-- genoit-istio-ansiblerole
        |-- roles
            |-- istio
                |-- vars
                    |-- main.yaml
```
- Desplegar el proyecto genoit-istio-ansiblerole con ANSIBLE PLAYBOOK

```raw
- Dirigirse a la carpeta raiz del proyecto.
    |-- genoit-istio-ansiblerole
        |-- main.yaml
        
- Ejecutar el comando con un usuario con permisos sudo en el Bastion Ansible.
  ansible-playbook main.yaml
```
- Desinstalar proyecto genoit-istio-ansiblerole

```raw       
  helm uninstall {{ desired_deployment_name }} -n {{ namespace }}
  ejm:
    helm uninstall istio-egress -n istio-system
    helm uninstall istio-ingress -n istio-system
    helm uninstall istio-discovery -n istio-system
    helm uninstall istio-cni -n istio-system
    helm uninstall istio-base -n istio-system

  kubectl -n {{ namespace }} delete secret {{ nombre_secreto_tls }}
  kubectl -n {{ namespace }} delete configmap --all
  ejm:
    kubectl -n istio-system delete secret istio-ca-secret genoit-credential
    kubectl -n istio-system delete configmap istio-ca-root-cert istio-leader istio-namespace-controller-election istio-validation-controller-election

```