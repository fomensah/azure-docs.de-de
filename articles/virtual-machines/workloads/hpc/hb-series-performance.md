---
title: Leistung von VM-Größen der HB-Serie – Azure Virtual Machines | Microsoft-Dokumentation
description: Hier finden Sie Leistungstestergebnisse für VM-Größen der HB-Serie in Azure.
services: virtual-machines
documentationcenter: ''
author: vermagit
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 05/15/2019
ms.author: amverma
ms.openlocfilehash: e064db5f67e6f8a7e82093bdae9fac7eaa4b6a55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534200"
---
# <a name="hb-series-virtual-machine-sizes"></a>VM-Größen der HB-Serie

Für die Größen der HB-Serie wurden zahlreiche Leistungstests durchgeführt. Im Anschluss finden Sie einige der Ergebnisse dieser Leistungstests:


| Workload                                        | HB                    |
|-------------------------------------------------|-----------------------|
| STREAM Triad                                    | ~260 GB/s (32-33 GB/s pro CCX)  |
| High-Performance Linpack (HPL)                  | ~1.000 GigaFLOPS (Rpeak), ~860 GigaFLOPS (Rmax) |
| RDMA-Wartezeit und -Bandbreite                        | 2,35 uSec, 96,5 GB/s   |
| FIO auf lokaler NVMe-SSD                           | ~1,7 GB/s (Lesen), ~1,0 GB/s (Schreiben)      |  
| IOR auf vier* Azure SSD Premium-Datenträgern (verwaltete Datenträger vom Typ „P30“, RAID0)**  | ~725 MB/s (Lesen), ~780 MB/s (Schreiben)   |



## <a name="infiniband-send-latency"></a>InfiniBand-Wartezeit (Senden)
Mellanox Perftest

```console
numactl --physcpubind=[INSERT CORE #]  ib_send_lat -a
```


|  Bytes         | Iterationen     | t_min[Mikrosekunden]     | t_max[Mikrosekunden]     | t_typical[Mikrosekunden] | t_avg[Mikrosekunden]     | t_stdev[Mikrosekunden]   |
|-----------------|-----------------|-----------------|-----------------|-----------------|-----------------|
| 2               | 1000            | 2,35            | 12,63           | 2,38            | 2,42            | 0,33            |
| 4               | 1000            | 2,35            | 18,53           | 2,38            | 2.4             | 0,21            |
| 8               | 1000            | 2,36            | 6,06            | 2,39            | 2.41            | 0,22            |
| 16              | 1000            | 2,36            | 6,05            | 2,39            | 2.41            | 0,21            |
| 32              | 1000            | 2,37            | 18,93           | 2.4             | 2,42            | 0,25            |
| 64              | 1000            | 2,39            | 17,98           | 2,43            | 2.45            | 0,18            |
| 128             | 1000            | 2.44            | 19,4            | 2.76            | 2.65            | 0,29            |
| 256             | 1000            | 3,06            | 18,31           | 3.1             | 3,12            | 0,27            |
| 512             | 1000            | 3,15            | 7,89            | 3.2             | 3,23            | 0,31            |
| 1024            | 1000            | 3,27            | 17,62           | 3,31            | 3,33            | 0,22            |
| 2048            | 1000            | 3.48            | 7,94            | 3.52            | 3.55            | 0,26            |
| 4096            | 1000            | 3,91            | 7,7             | 3,96            | 3,98            | 0,21            |


## <a name="osu-mpi-latency-test"></a>OSU-MPI-Wartezeittest

OSU MPI Latency Test v5.4.3

```azure-cli
./bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./osu_latency 
```


| Bytes  | Wartezeit [Mikrosekunden] (MPICH 3.3 + CH4) | Wartezeit [Mikrosekunden] (OpenMPI 4.0.0) | Wartezeit [Mikrosekunden] (MVAPICH2 2.3) | Wartezeit [Mikrosekunden] (Intel MPI 2019) |
|------|----------|----------|----------|----------|
| 2    | 2.44     | 2.52     | 2.84     | 2.76     |
| 4    | 2.44     | 2.53     | 2.84     | 2.76     |
| 8    | 2.44     | 2.53     | 2.83     | 2.76     |
| 16   | 2.45     | 2.53     | 2,87     | 2.77     |
| 32   | 2.62     | 2.69     | 2,89     | 2.78     |
| 64   | 2.72     | 2.79     | 2,93     | 2.85     |
| 128  | 2.76     | 2,88     | 3,06     | 2,91     |
| 256  | 3,53     | 3.65     | 3.73     | 3.57     |
| 512  | 3.68     | 3,78     | 3,81     | 3.70     |
| 1024 | 3,86     | 3,97     | 3,95     | 3,93     |
| 2048 | 4,12     | 4,5      | 4.24     | 4,22     |
| 4096 | 4,79     | 5.28     | 6,33     | 4,91     |


## <a name="mpi-bandwidth"></a>MPI-Bandbreite

OSU MPI Bandwidth Test v5.4.3

```azure-cli
./mvapich2-2.3.install/bin/mpirun_rsh -np 2 -hostfile ~/hostfile MV2_CPU_MAPPING=[INSERT CORE #] ./mvapich2-2.3/osu_benchmarks/mpi/pt2pt/osu_bw
```

| Größe            | Bandbreite (MB/s) | Bandbreite (GBit/s) |
|------------------|------------------|------------------|
| 2                | 4,03             | 0,03             |
| 4                | 8,2              | 0,07             |
| 8                | 16,15            | 0,13             |
| 16               | 32,33            | 0,26             |
| 32               | 64,36            | 0,51             |
| 64               | 126,29           | 1.01             |
| 128              | 234,14           | 1,87             |
| 256              | 486,89           | 3,90             |
| 512              | 874,24           | 6,99             |
| 1024             | 1538,47          | 12,31            |
| 2048             | 2743,98          | 21,95            |
| 4096             | 4194,69          | 33,56            |
| 8192             | 5657,67          | 45,26            |
| 16384            | 7618,96          | 60,95            |
| 32768            | 10333,76         | 82,67            |
| 65536            | 11171,06         | 89,37            |
| 131072           | 11539,64         | 92,32            |
| 262144           | 11768,43         | 94,15            |
| 524288           | 11908,59         | 95,27            |
| 1048576          | 12012,8          | 96,10            |
| 2097152          | 12049,38         | 96,40            |
| 4194304          | 12061,33         | 96,49            |


## <a name="next-steps"></a>Nächste Schritte

Informieren Sie sich ausführlicher über [High Performance Computing](https://docs.microsoft.com/azure/architecture/topics/high-performance-computing/) in Azure.




