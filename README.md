# Ceph on Kuberntes

## Problem statement

We want to use the Ceph CSI driver to consume from an external cluster. Wiring this is is a bit tricky, as it involves building a trust relationship including key exchange.

Aiming at a fast development cycle, we also want a local environment.

Ideally, this would be fully covered by ArgoCD and terraform and making the local bit an implementation detail.

However, we need to get things going first.

Hence, we start by borrowing proven things and adapting them as we go. Naturally, this starts by learning from the Rook developers. Thats how we arrive at minikube for external server.

We feature:

- Rook using an external Ceph Service
- Ceph RBD (RWO)- / CephFS (RWX) Persistent Volumes / Persistent Volume Claims via CSI
- Ceph Volume Snapshots
- S3 ObjectBuckets/ObjectBucketClaims
- Velero CSI based Backup/Restore
- Health Checks/Recovery/Ops (naturally by Rook Operator)

## TODO

- mons on port 3300 (workaround: use port 6789 / `ROOK_EXTERNAL_CEPH_MON_DATA`): `2024-12-16T16:56:02.784+0000 7fd593d1c000 -1 failed for service _ceph-mon._tcp
mount error: no mds (Metadata Server) is up. The cluster might be laggy, or you may not be authorized
  Warning  FailedMount  2m25s  kubelet  (combined from similar events): MountVolume.MountDevice failed for volume "pvc-026c86e8-9ee4-4261-a7e4-083011b80494" : rpc error: code = Internal desc = an error (exit status 32) occurred while running mount args: [-t ceph 192.168.122.231:3300:/volumes/csi/csi-vol-7072e90c-5d6b-477b-bbab-655b76d0425f/e8d828a3-a1ad-4a22-9b36-7d5bc9fe9026 /var/lib/kubelet/plugins/kubernetes.io/csi/rook-ceph.cephfs.csi.ceph.com/f172f41f387d01c38f46e71a4097304d70c35494e81e1c8a070549de56234790/globalmount -o name=csi-cephfs-node,secretfile=/tmp/csi/keys/keyfile-2436134297,mds_namespace=myfs,_netdev] stderr: unable to get monitor info from DNS SRV with service name: ceph-mon`
- [Looking up Monitors through DNS](https://docs.ceph.com/en/latest/rados/configuration/mon-lookup-dns/)
- [OperatorHub Sub Outdated - at 1.1.1](https://operatorhub.io/operator/rook-ceph)
