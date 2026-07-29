### Firewall 
**Reglas Completa de Firewall (Matriz Nutanix CSI):** Asegúrate de mantener abiertos de forma bidireccional desde los nodos Worker de Kubernetes hacia las IPs de CVM / Data Services VIP:

- **TCP 9440:** API REST Prism (Control Plane)
- **TCP 3260:** Tráfico de Datos iSCSI (Data Plane)
- **TCP 3205:** Descubrimiento de Targets iSCSI (Stargate)

| **Puerto / Protocolo** | **Origen** | **Destino**                                      | **Propósito / Servicio**                                                                                          |
| ---------------------- | ---------- | ------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------- |
| **TCP 9440**           | Nodos K8s  | **Prism Element VIP / CVMs** y **Prism Central** | **API Rest / Control Plane:** Gestión de volúmenes, aprovisionamiento de PVCs y comunicación gRPC del driver.     |
| **TCP 3260**           | Nodos K8s  | **Data Services IP (iSCSI VIP)**                 | **Nutanix Volumes (iSCSI Block Storage):** Tráfico del plano de datos para volúmenes de bloque (_ReadWriteOnce_). |
| **TCP 3205**           | Nodos K8s  | **Data Services IP / CVMs**                      | **iSCSI Discovery:** Servicio de descubrimiento iSCSI de Nutanix Volumes.                                         |
| **TCP 2049 / 111**     | Nodos K8s  | **Nutanix Files (IPs del File Server)**          | **Nutanix Files (NFS Storage):** Para volúmenes de archivos/compartidos (_ReadWriteMany_).                        |

### Prerequisitos

```shell
sudo apt-get update
sudo apt-get install -y open-iscsi lsscsi sg3-utils multipath-tools scsitools
sudo systemctl enable --now iscsid
```
### Instalación

```shell
# 1. Agregar el nuevo repositorio oficial de releases de Nutanix CSI
helm repo add nutanix-releases https://nutanix.github.io/helm-releases/

# 2. Actualizar la lista de repositorios
helm repo update

# 3. Listar las versiones disponibles de nutanix-csi-*
helm search repo nutanix-releases/nutanix-csi- --versions

# 4.1 Instalación de Nutanix snapshot
helm install nutanix-csi-snapshot nutanix-releases/nutanix-csi-snapshot \ 
	--namespace ntnx-system \ 
	--create-namespace
	
# 4.2 Instalación de Nutanix Storage
helm install nutanix-csi-storage nutanix-releases/nutanix-csi-storage \ 
	--namespace ntnx-system \
	--create-namespace \
	--set secretName="ntnx-secret" \
	--set createSecret=true \
	--set username="usuario_Nutanix" \
	--set password='password_Nutanix' \
	--set prismEndPoint="IP_Nutanix_Prism_Element" \
	--set ntnxInitConfigMap.usePC=false \
	--set createPrismCentralSecret=false
```


