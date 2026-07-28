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

