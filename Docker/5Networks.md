# Docker Networking - Simplified Guide (Interview Ready)

## 🔹 1. Bridge Network (Default)

-   Default network when you run a container without specifying
    `--network`.
-   Creates a **virtual bridge (docker0)** on the host.
-   Containers get private IPs (e.g., 172.17.x.x).
-   NAT (iptables) allows outbound internet.
-   Containers on the same user-defined bridge can **resolve each other
    by name**.

**Use Case:** Single-host apps, local dev.\
**Interview Tip:** *"Bridge network provides isolated container
networking with DNS-based service discovery in user-defined networks."*

**Task:**

``` bash
docker network create mynet
docker run -d --name app1 --network mynet nginx
docker run -it --name app2 --network mynet alpine ping app1
```

------------------------------------------------------------------------

## 🔹 2. Host Network

-   Removes network isolation.
-   Container shares **host network namespace** (uses host IP).
-   No port mapping needed, but risk of conflicts.
-   Faster, as there's no NAT overhead.

**Use Case:** Monitoring agents, proxies, apps needing raw host
performance.\
**Interview Tip:** *"Host network gives maximum performance but
sacrifices container-level isolation."*

**Task:**

``` bash
docker run --network host nginx
# Access via host's IP directly on port 80
```

------------------------------------------------------------------------

## 🔹 3. None Network

-   Completely disables networking.
-   Only `lo` (loopback) is available.
-   You can attach the container to a network later.

**Use Case:** Security isolation, custom network plugins.\
**Interview Tip:** *"None network provides maximum isolation; useful
when networking is externally managed."*

**Task:**

``` bash
docker run --network none alpine ip addr
```

------------------------------------------------------------------------

## 🔹 4. Overlay Network

-   Enables communication **across multiple Docker hosts**.
-   Built using **VXLAN tunneling**.
-   Requires Docker Swarm or Kubernetes (uses CNI).

**Use Case:** Multi-node microservices communication.\
**Interview Tip:** *"Overlay networks are key for distributed
microservices since they span across hosts."*

**Task:**

``` bash
docker swarm init
docker network create -d overlay myoverlay
# Deploy services to communicate across nodes
```

------------------------------------------------------------------------

## 🔹 5. Macvlan Network

-   Assigns containers IPs directly from the **physical LAN**.
-   Containers appear as physical devices to the switch/router.
-   Needs parent interface (e.g., eth0).

**Use Case:** Legacy apps, IoT, when real LAN IP is required.\
**Interview Tip:** *"Macvlan makes containers first-class citizens on
the LAN."*

**Task:**

``` bash
docker network create -d macvlan   --subnet=192.168.1.0/24   --gateway=192.168.1.1   -o parent=eth0 mymacvlan
```

------------------------------------------------------------------------

## 🔹 6. IPvlan Network

-   Similar to macvlan but designed for **large-scale networks**.
-   Modes:
    -   **L2 mode:** each container gets its own MAC (like macvlan).
    -   **L3 mode:** containers share host MAC but have unique IPs.

**Use Case:** Data centers with MAC address limits.\
**Interview Tip:** *"IPvlan is preferred in large-scale deployments
where MAC address sprawl must be avoided."*

**Task:**

``` bash
docker network create -d ipvlan   --subnet=192.168.1.0/24   --gateway=192.168.1.1   -o parent=eth0 --ipvlan-mode=l3 myipvlan
```

------------------------------------------------------------------------

## 🔑 Quick Comparison (Table)

  -------------------------------------------------------------------------------
  Network      Scope        Isolation    Performance    Real IP   Use Case
  ------------ ------------ ------------ -------------- --------- ---------------
  Bridge       Single host  Yes          Medium         No        Default, local
                                                                  apps

  Host         Single host  No           High           Yes       Monitoring,
                                                                  proxies

  None         Single host  Max          N/A            No        Isolation

  Overlay      Multi-host   Yes          Medium         No        Microservices

  Macvlan      Multi-host   Yes          High           Yes       Legacy, IoT

  IPvlan       Multi-host   Yes          High           Yes       Data centers
  -------------------------------------------------------------------------------

------------------------------------------------------------------------

## 🎯 Key Interview Pointers

-   Always mention **use case + trade-off** when asked.\
-   Bridge = default, NAT-based, DNS in user-defined networks.\
-   Host = no isolation, high performance.\
-   None = full isolation.\
-   Overlay = multi-host, VXLAN tunnels.\
-   Macvlan = container gets LAN IP.\
-   IPvlan = scalable macvlan alternative.

------------------------------------------------------------------------

✅ With this knowledge, you can confidently explain Docker networking in
interviews with clear terminology and practical examples.
