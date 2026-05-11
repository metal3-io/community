# General Technical Review - Metal3.io / Graduation

- **Project:** Metal3.io (Metal³)
- **Review target:** CNCF Graduation
- **Project Version:** v0.13.x (BMO), v1.13.x (CAPM3), v1.13.x (IPAM)
- **Website:** <https://metal3.io>
- **Date Updated:** 2026-05-04
- **Template Version:** v1.0
- **Description:** Metal3.io provides Kubernetes-native bare metal
  host provisioning and lifecycle management, integrated with the
  Cluster API for declarative bare metal cluster management. It
  uses OpenStack Ironic as its default provisioning backend.

## Overall Assessment

This self-technical-review evaluates Metal3.io's technical
maturity for progression from Incubation to Graduation. The
project demonstrates strong technical maturity with a
well-architected, Kubernetes-native solution for bare metal
lifecycle management that is proven in production by multiple
adopters at scale (up to 2000+ nodes). Security practices follow
cloud native best practices with hardened defaults, signed
images, and SBOMs. Documentation is thorough with architecture
guides, quick-start tutorials, and a dedicated book site.

The project fills a genuine gap in the Kubernetes ecosystem —
declarative bare metal management using standard BMC protocols
via the Cluster API. Metal3 has a clear separation of concerns
across independent controllers (BMO, CAPM3, IPAM), follows
Kubernetes API conventions, and integrates cleanly with the
broader cloud native ecosystem.

---

## Day 0 - Planning Phase

### Scope

- Describe the roadmap process, how scope is determined for mid
  to long term features, as well as how the roadmap maps back to
  current contributions and maintainer ladder?
   - The project maintains a [roadmap][roadmap] and tracks work
     through a [GitHub Project board][roadmap-board], with
     features proposed as issues in the metal3-docs repository
     accompanied by [design documents][design-docs]. Proposals
     are discussed in weekly community meetings, annual Metal3
     meetups, and curated by the designated "Roadmap editors"
     team. Roadmap editors are appointed from the maintainer
     pool.
- Describe the target persona or user(s) for the project?
   - The project targets platform engineers and infrastructure
     operators managing bare metal Kubernetes clusters, primarily
     in Telco, edge computing, and on-premise data center
     environments
     ([adopters list][adopters]).
- Explain the primary use case for the project. What additional
  use cases are supported by the project?
   - Declarative lifecycle management of bare metal servers as
     Kubernetes nodes using Cluster API — including provisioning,
     hardware inspection, disk cleaning, and decommissioning —
     via standard BMC protocols (Redfish, IPMI). Additional:
     standalone bare metal management without CAPI via Baremetal
     Operator directly, IP address management for bare metal
     networks.
- Explain which use cases have been identified as unsupported by
  the project.
   - Metal3 does not manage cloud provider VMs, orchestrate
     non-Kubernetes workloads, or provision hosts that lack
     BMC/out-of-band management access. It is designed
     exclusively for bare metal "self-hosted" workflows and does
     not require any commercial cloud provider service.
- Describe the intended types of organizations who would benefit
  from adopting this project.
   - Telco operators (Ericsson, Deutsche Telekom), enterprise
     infrastructure providers (Red Hat, SUSE, Fujitsu), telco
     cloud infrastructure platforms (Sylva, Airship), AI/ML
     infrastructure (Mistral AI), and large-scale on-premise
     environments (Mirantis). Deployments range from
     single-server setups to 2000+ node production
     environments.
- Please describe any completed end user research and link to any
  reports.
   - Adopter feedback is gathered through the CNCF incubation
     adopter interviews (Ericsson, Red Hat, SUSE, Deutsche
     Telekom, and a fifth Telco adopter — see
     [incubation DD][incubation]), direct engagement at community
     meetings, KubeCon presentations, and the
     [Metal3 annual meetup][meetup-notes].

### Usability

- How should the target personas interact with your project?
   - Operators interact entirely through Kubernetes-native APIs
     by creating and managing BaremetalHost, Metal3Machine,
     Metal3Cluster, IPPool, and IPClaim custom resources using
     standard tools like kubectl and clusterctl.
- Describe the user experience (UX) and user interface (UI) of
  the project.
   - Metal3 has no graphical UI; the interface is the Kubernetes
     API with CRDs and status conditions. Operators use kubectl (or other
     Kubernetes API clients)to inspect BaremetalHost status, events, and conditions,
     which surface the full hardware lifecycle state machine
     (registering → inspecting → available → provisioning →
     provisioned → deprovisioning).
   - The core experience is the BaremetalHost Custom Resource —
     to provision a host, a user writes a YAML manifest with
     BMC credentials and desired state. No graphical dashboard
     or vendor-specific tooling is required.
   - In Cluster API-based workflows, operators interact through
     Cluster API resources (for example Cluster,
     KubeadmControlPlane, and MachineDeployment) while CAPM3
     reconciles the corresponding Metal3 resources
     (Metal3Cluster and Metal3Machine) and drives bare metal
     lifecycle actions.
- Describe how this project integrates with other projects in a
  production environment.
   - Metal3 implements the [Cluster API infrastructure provider
     contract][capi] for bare metal, integrates OpenStack Ironic
     as its provisioning backend, and works with any CNI, CSI,
     and Kubernetes distribution. It is composable with the
     Metal3 IPAM or external IPAM solutions, and integrates with
     observability backends via Prometheus metrics.

### Design

- Explain the design principles and best practices the project
  is following.
   - Kubernetes-native CRDs + controllers; declarative,
     vendor-neutral bare metal management using standard BMC
     protocols; delegation of hardware provisioning to
     OpenStack Ironic rather than reimplementing it;
     separation of concerns across independent controllers
     (BMO, CAPM3, IPAM).
- Outline or link to the project's architecture requirements?
  Describe how they differ for Proof of Concept, Development,
  Test and Production environments, as applicable.
   - PoC/dev: [metal3-dev-env][dev-env] provides a virtualized
     environment using QEMU/KVM VMs with virtual BMCs
     (sushy-tools for Redfish, vbmc for IPMI) and a Kind or
     Minikube management cluster.
   - Production: requires BMC network connectivity, Ironic
     deployment with appropriate database backend (MariaDB for
     scale), and HA controller configuration with leader
     election.
   - End-to-end tests run in CI across BMO, CAPM3, and IrSO
     repositories covering install, provision, deprovision,
     and upgrade workflows.
   - Architecture documented at
     [book.metal3.io/architecture][arch].
- Define any specific service dependencies the project relies on
  in the cluster.
   - Kubernetes API server (with etcd), Ironic deployed via
     the [Ironic Standalone Operator][irso] (recommended for
     production), and a MariaDB or SQLite backend for Ironic
     state. PXE boot requires DHCP/TFTP infrastructure unless
     virtual media boot is used. No external database or SaaS
     dependency is required.
- Describe how the project implements Identity and Access
  Management.
   - Metal3 leverages Kubernetes-native security primitives
     rather than building its own IAM system. It follows the
     principle of least privilege, separating controller
     permissions per component (BMO manages hosts, CAPM3
     manages metal3 machines, IPAM manages addresses).
   - Each operator runs with its own Kubernetes ServiceAccount
     with RBAC scoped to minimal required permissions. BMC
     credentials are stored in Kubernetes Secrets referenced by
     BaremetalHost resources and are never logged or exposed in
     status fields.
   - Developers only need permission to create/modify the
     BaremetalHost and other CRDs in their specific namespace.
- Describe how the project has addressed sovereignty.
   - Metal3 runs entirely on-premise with no external cloud
     dependencies. It uses standard BMC protocols (Redfish,
     IPMI) and is not tied to any specific hardware vendor.
     All data — including BMC credentials, host inventory,
     and provisioning state — remains within the operator's
     Kubernetes cluster. The project supports air-gapped
     deployments.
- Describe any compliance requirements addressed by the project.
   - The project has earned an
     [OpenSSF Best Practices badge][ossf], uses DCO for all
     commits, and follows [CNCF SSCP][sscp] best practices
     with signed releases and SBOMs. The on-premise,
     air-gap-capable deployment model supports adopters' own
     compliance requirements (e.g., Telco security standards,
     data sovereignty regulations).
- Describe the project's High Availability requirements.
   - BMO and CAPM3 controllers support leader election via
     controller-runtime for HA deployments. Ironic can be
     deployed in active-passive HA configurations using the
     [Ironic Standalone Operator][irso]. The BaremetalHost state
     machine is idempotent and resumes correctly after controller
     restarts.
- Describe the project's resource requirements, including CPU,
  Network and Memory.
   - Upstream BMO and CAPM3 manifests do not prescribe fixed
     CPU or memory requests/limits for controller pods.
     Resource usage depends on fleet size, reconciliation
     churn, and provisioning concurrency, so operators should
     baseline from runtime metrics and set requests/limits for
     their environment. The Ironic Python Agent (IPA), which
     runs temporarily on each host during provisioning and
     inspection, is the main per-host resource consumer.
     Ironic itself can be tuned for large-scale deployments.
- Describe the project's storage requirements, including its use
  of ephemeral and/or persistent storage.
   - Storage needs are primarily in Ironic and image-serving
     components, not in BMO/CAPM3 controllers. Ironic can run
     with an ephemeral local SQLite database or with a
     persistent external MariaDB database. In production,
     image-server storage for OS images (and IPA artifacts)
     is typically the dominant requirement and should be sized
     for the number of images, image formats, and parallel
     rollout/upgrade retention policy. BMO/CAPM3 controllers
     are stateless and persist state in Kubernetes APIs.
- Please outline the project's API Design:
   - Describe the project's API topology and conventions
      - Metal3 uses Kubernetes CRDs following standard API
        conventions: BaremetalHost,
        Metal3Machine, Metal3Cluster, Metal3MachineTemplate,
        IPPool, IPClaim, and IPAddress.
      - Topology: Cluster API Cluster objects reference
        Metal3Cluster as infrastructure; Cluster API Machine
        objects reference Metal3Machine as infrastructure
        (typically created from Metal3MachineTemplate via
        MachineDeployment/KubeadmControlPlane). Metal3Machine
        then associates to a BaremetalHost for lifecycle
        actions. IPClaim requests addresses from an IPPool and
        results in an IPAddress allocation.
      - Controller grouping: BaremetalHost is reconciled by BMO
        controllers; Metal3Cluster/Metal3Machine/
        Metal3MachineTemplate are reconciled by CAPM3
        controllers; IPPool/IPClaim/IPAddress are reconciled by
        Metal3 IPAM controllers.
   - Describe the project defaults
      - The recommended Ironic deployment overlay enables basic
        authentication and TLS. Newly created BaremetalHosts
        go through registering → inspecting → available by
        default, and inspection is enabled for new hosts.
      - With CAPM3 and Metal3 IPAM installed via clusterctl
        provider manifests, default components include
        controller-manager Deployments, ServiceAccounts, and
        RBAC (ClusterRoles/RoleBindings plus leader-election
        Roles/RoleBindings) needed to reconcile their CRDs.
      - CAPM3/IPAM provider manifests also enable admission
        webhooks and webhook Services by default, with
        certificate resources and CA-injection annotations for
        webhook TLS (typically backed by cert-manager in
        production installs).
   - Outline any additional configurations from default to make
     reasonable use of the project
      - Key environment-specific setup is outside one-size
        defaults: Ironic network configuration (for example
        provisioning/external network layout, DHCP/PXE and
        endpoint exposure via IRSO) and an image server strategy
        for OS images/IPA artifacts. These depend on site
        topology, addressing, security policy, and operational
        model. The [quick start][quickstart] walks through a
        reference setup.
   - Describe any new or changed API types and calls — including
     to cloud providers — that will result from this project
     being enabled and used
      - Adds a set of Metal3 CRDs to the cluster (non-exhaustive
        list): infrastructure and machine APIs (for example
        Metal3Cluster, Metal3Machine, Metal3MachineTemplate,
        Metal3Data and Metal3DataTemplate), host lifecycle and
        inventory APIs (for example BaremetalHost, HardwareData,
        HostFirmwareComponents, HostClaim), and IPAM APIs
        (IPPool, IPClaim, IPAddress). When IRSO is installed, it
        also adds Ironic CRDs. The exact set evolves by release.
        Metal3 does not change Kubernetes core APIs and does not
        add cloud-provider APIs. Controllers call BMC endpoints
        (for example Redfish or IPMI) and Ironic APIs inside the
        deployment.
   - Describe compatibility of any new or changed APIs with API
     servers, including the Kubernetes API server
      - CRDs follow standard Kubernetes structural schema
          validation.
   - Describe versioning of any new or changed APIs, including
     how breaking changes are handled
      - Current released Metal3 APIs are v1beta1. API versions
        follow Kubernetes conventions (v1alpha1, v1beta1).
        Unreleased `main` branch work toward newer Cluster API
        contracts is not reflected here yet. Check the release
        notes for version history.
- Describe the project's release processes, including major,
  minor and patch releases.
   - Releases happen aligned with Cluster API and Kubernetes
     cycles (~every 4 months) following SemVer, with patch
     releases as needed. Releases are cut from stable branches
     (two supported release lines plus one additional
     CI-tested line), published via GitHub Releases with
     changelogs, signed with cosign, and include SBOMs.
- Describe how the project is installed and initialized, e.g. a
  minimal install with a few lines of code or does it require
  more complex integration and configuration?
   - Installation via `clusterctl init --infrastructure metal3`
     (Cluster API CLI) or kustomize manifests.
    The upstream [quick start][quickstart] walks through a
     complete initial setup for both direct BaremetalHost
    provisioning and Cluster API-based deployments.
     Ironic is deployed as containers either within the
     management cluster or standalone via the
     [Ironic Standalone Operator][irso]. The
     [metal3-dev-env][dev-env] provides a turnkey development
     environment, while CI e2e suites in BMO/CAPM3/IPAM/IrSO
     continuously validate production paths.
- How does an adopter test and validate the installation?
   - Create a BaremetalHost resource pointing to a real or
     virtual BMC and confirm the host transitions through
     registering → inspecting → available states. The
     metal3-dev-env provides a fully virtualized environment
     with virtual BMCs for local end-to-end validation, and CI
     runs full install → provision → deprovision → uninstall
     workflows across component repositories.

### Security

- Please provide a link to the project's cloud native security
  self assessment.
   - [metal3-docs PR #456][self-assessment] and
     [OpenSSF Best Practices badge][ossf].
- Please review the Cloud Native Security Tenets from TAG
  Security.
   - How are you satisfying the tenets of cloud native security
     projects?
      - Runs controllers with least-privilege RBAC, stores BMC
        credentials in Kubernetes Secrets, recommends TLS/auth
        for Ironic communication, signs and scans container
        images,
        and documents a [security policy][sec-policy]. The
        [self-assessment][self-assessment] covers all the
        details.
   - Describe how each of the cloud native principles apply to
     your project.
      - Metal3 is implemented as Kubernetes controllers that
        reconcile CRDs such as BaremetalHost, Metal3Machine,
        Metal3Cluster, IPPool, IPClaim, and IPAddress. Desired
        state is declared in the Kubernetes API, while
        provisioning and inspection are executed through Ironic,
        IPA, and BMC protocols such as Redfish and IPMI.
        Components are loosely coupled: BMO manages hosts,
        CAPM3 integrates with Cluster API, and IPAM manages
        address allocation.
   - How do you recommend users alter security defaults in order
     to "loosen" the security of the project? Please link to any
     documentation the project has written concerning these use
     cases.
      - Metal3's recommended production setup uses Ironic
        authentication and TLS, BMC credentials in Secrets, and
        minimal
        RBAC. Users who need to loosen defaults can adjust:
         - **Disable Ironic TLS/auth**: for legacy environments
           that cannot support TLS — increases exposure of the
           provisioning API.
         - **Widen RBAC scope**: granting broader permissions to
           controllers — increases blast radius of credential
           leak.
         - **Disable inspection**: skip hardware introspection
           for faster provisioning — reduces hardware inventory
           data.
      - Each change weakens a specific layer of defense-in-depth;
        users should evaluate the trade-off against their
        environment's requirements. See the
        [security documentation][sec-policy].
- Security Hygiene
   - Please describe the frameworks, practices and procedures the
     project uses to maintain the basic health and security of
     the project.
      - DCO sign-off required on all commits. Dependabot monitors
        Go module dependencies. Container base images are
        regularly updated. CI runs linters and vulnerability
        scanners on every PR. Published container images include
        SBOMs. The project has earned an
        [OpenSSF Best Practices badge][ossf].
   - Describe how the project has evaluated which features will
     be a security risk to users if they are not maintained by
     the project?
      - The main security-sensitive areas are BMC credential
        storage and access, Ironic API exposure, container image
        provenance, and network-level access to provisioning
        infrastructure — all mentioned in the
        [security self-assessment][self-assessment].
- Cloud Native Threat Modeling
   - Explain the least minimal privileges required by the project
     and reasons for additional privileges.
      - Baseline model: each controller needs read/write access
        to the CRDs it reconciles, plus standard controller
        permissions for Events and leader election. Some
        controllers also need read access to Secrets and
        ConfigMaps for credentials and configuration.
      - CAPM3 also interacts with Cluster API objects
        (for example Cluster and Machine resources) to satisfy
        the infrastructure-provider contract.
      - IRSO manages Kubernetes built-in workload resources (for example
        Deployments, DaemonSets, Services, Pods, and Secrets) as part of Ironic
        lifecycle management.
      - Additional privileges: default manifests are generally
        cluster-scoped for ease of installation and multi-
        namespace operation. BMO can also run in namespace-
        scoped mode (for example via WATCH_NAMESPACE) with
        namespace Roles/RoleBindings, reducing privilege scope
        when that operating model is preferred.
   - Describe how the project is handling certificate rotation
     and mitigates any issues with certificates.
      - TLS certificates for Ironic communication can be managed
        via cert-manager or manual rotation. The Ironic
        Standalone Operator supports automated certificate
        management. In standard CAPM3/IPAM/BMO/IRSO
        installations, webhook certificates are issued and
        rotated by cert-manager, which also maintains the
        Secrets mounted by the controller pods.
   - Describe how the project is following and implementing
     secure software supply chain best practices.
      - Released container images are signed with keyless
        cosign and include SPDX SBOM attestations in the
        release workflows. Go module dependencies are pinned in
        `go.sum` and verified by Go module checksum
        validation. Dependabot and periodic OSV scanning are
        used to detect known vulnerabilities. The project
        follows [CNCF SSCP][sscp] best practices.

## Day 1 - Installation and Deployment Phase

### Project Installation and Configuration

- Describe what project installation and configuration look like.
   - Typical bootstrap flow: create a bootstrap Kubernetes
     cluster (or reuse an existing one), install cert-manager,
     deploy Ironic via IRSO (unless Ironic is already provided
     externally), then deploy Bare Metal Operator manifests.
     Optionally initialize Cluster API plus CAPM3/IPAM via
     `clusterctl` (or CAPI Operator). At that point the cluster
     is ready to register BaremetalHosts. If the bootstrap
     cluster is temporary, create the persistent management
     cluster and move management objects with `clusterctl move`
     following Cluster API guidance. [Quick start here][quickstart]
     and [metal3-dev-env][dev-env] for local reproduction.

### Project Enablement and Rollback

- How can this project be enabled or disabled in a live cluster?
  Please describe any downtime required of the control plane or
  nodes.
   - Simple deployment apply/delete. BMO, CAPM3, and IPAM are
     standard Kubernetes Deployments — no control plane
     downtime. Existing bare metal hosts continue running
     unaffected when controllers are removed.
- Describe how enabling the project changes any default behavior
  of the cluster or running workloads.
   - Enabling Metal3 does not affect the cluster's default
     behavior or running workloads, other than consuming some
     resources.
- Describe how the project tests enablement and disablement.
   - CI covers this in multiple layers: BMO e2e tests and
     Cluster API test-framework-based integration/feature/upgrade
     tests across CAPM3 and IPAM repositories. Those jobs
     exercise install → provision → deprovision → uninstall,
     including CRD cleanup and controller removal across
     supported release branches. metal3-dev-env scenarios are
     kept as secondary validation.
- How does the project clean up any resources created, including
  CRDs?
   - Controllers use finalizers on BaremetalHost and
     Metal3Machine resources to ensure hardware is properly
     deprovisioned before deletion. CRDs can be removed after
     all CR instances are deleted. `clusterctl delete
     --infrastructure metal3 --ipam metal3` handles full
     cleanup of CAPM3 and IPAM. BMO and Ironic can be removed
     the same way they were installed, for example by deleting
     BMO release manifests and deleting the Ironic object.

### Rollout, Upgrade and Rollback Planning

- How does the project intend to provide and maintain
  compatibility with infrastructure and orchestration management
  tools like Kubernetes and with what frequency?
   - Release lines follow Cluster API cadence. CI includes
     N-3 upgrade-path testing for control plane and worker
     rollouts across branch-specific jobs (for example,
     clusterctl upgrade tests from older supported release
     lines to newer ones). Releases are cut about every
     4 months aligned with CAPI/Kubernetes cycles.
- Describe how the project handles rollback procedures.
   - Rollback is performed by redeploying the previous
     controller version; the CRD state machine is designed to
     be forward-compatible, and controllers reconcile from
     current resource state regardless of version.
     **Ironic database migrations are forward-only and
     represent the main rollback risk** — operators should
     snapshot the Ironic database before upgrading.
- How can a rollout or rollback fail? Describe any impact to
  already running workloads.
   - CRD schema incompatibilities with stored resources, or new
     controller version introducing a bug in the state machine.
     Ironic database migrations being forward-only mean
     downgrades may require database restoration.
     Already-provisioned bare metal hosts continue running
     unaffected — only lifecycle operations are affected.
- Describe any specific metrics that should inform a rollback.
   - Watch for: BaremetalHost resources stuck in `error` state,
     increased controller restart count, or elevated
     reconciliation error rates in Prometheus metrics.
- Explain how upgrades and rollbacks were tested and how the
  upgrade->downgrade->upgrade path was tested.
   - Upgrade tests are part of the CI matrix. CAPM3 runs
     `clusterctl` upgrade and Kubernetes version-upgrade e2e
     tests from supported releases to current versions.
     Downgrade testing is not part of this path: Cluster API
     does not support version downgrades and warns that
     downgrades may leave a management cluster non-functional.
     Conversion webhooks in Metal3 CRDs provide API-version
     conversion compatibility for stored objects, but this is
     not equivalent to a full component downgrade
     certification.
- Explain how the project informs users of deprecations and
  removals of features and APIs.
   - Through release notes, the [Metal3 book][book], and
     community meetings. API field deprecations follow the
     Kubernetes deprecation policy with at least one release of
     overlap.
- Explain how the project permits utilization of alpha and beta
  capabilities as part of a rollout.
   - Alpha features are gated behind feature flags in controller
     configuration and are disabled by default. API versions
     follow Kubernetes conventions (v1alpha1, v1beta1).
     Component maturity:

     | Component | Maturity | Version |
    | :---------- | :--------- | :-------- |
      | Baremetal Operator (BMO) | **Stable** | v0.13.x |
      | Cluster API Provider Metal3 (CAPM3) | **Stable** | v1.13.x |
      | IP Address Manager (IPAM) | **Stable** | v1.13.x |
     | Ironic Standalone Operator (IrSO) | **Beta** | v0.x |
     | Ironic Image | **Stable** | Released with BMO |
     | IPA Downloader | **Stable** | Released with BMO |
     | MariaDB Image | **Stable** | Released independently |

## Day 2 - Day-to-Day Operations Phase

### Scalability/Reliability

- Describe how the project increases the size or count of
  existing API objects.
   - Enabling Metal3 components adds a bounded set of Kubernetes
     API objects in the management cluster: controller
     Deployments/Pods, webhook Services and configurations,
     RBAC objects, CRDs, and related Secrets/ConfigMaps. Ongoing
     API-object growth is primarily driven by user workloads and
     fleet size: each managed host maps to one BareMetalHost,
     with associated Secrets and optional IPClaim/IPAddress
     resources. Controllers also emit Events, mostly in response
     to user-triggered provisioning and lifecycle actions. CI
     includes scalability scenarios that exercise increased
     API-object counts and reconciliation behavior at higher
     host counts, with no fan-out amplification pattern in
     controller logic.
- Describe how the project defines Service Level Objectives
  (SLOs) and Service Level Indicators (SLIs).
   - Exposes Prometheus metrics for reconciliation latency,
     provisioning success rate, and controller error rates that
     adopters can use to define their own SLOs. Formal SLOs are
     not yet defined by the project.
- Describe any operations that will increase in time covered by
  existing SLIs/SLOs.
   - Larger fleet sizes increase total reconciliation loop time.
     Concurrent provisioning operations are bounded by Ironic's
     conductor thread pool.
- Describe the increase in resource usage in any components as a
  result of enabling this project, to include CPU, Memory,
  Storage, Throughput.
   - Pretty minimal footprint per controller (~256Mi memory,
     0.5 CPU). Usage scales linearly with managed
     BaremetalHosts. Ironic database size grows proportionally
     to host count.
- Describe which conditions enabling / using this project would
  result in resource exhaustion of some node resources (PIDs,
  sockets, inodes, etc.)
   - Extremely large fleet sizes (thousands of hosts) with
     concurrent provisioning could exhaust Ironic database
     connections. Mitigated by tuning Ironic's conductor thread
     pool and database connection pooling.
- Describe the load testing that has been performed on the
  project and the results.
   - Red Hat has validated Metal3 at 2000+ node scale in
     production. Community CI tests cover up to ~100 virtual
     hosts; larger-scale testing is performed by adopters
     (Ericsson, Red Hat, SUSE) in their production pipelines.
     Ironic scalability has also been tested manually with fake
     IPA agents at 1000-cluster scale.
- Describe the recommended limits of users, requests, system
  resources, etc. and how they were obtained.
   - A single BMO instance comfortably manages hundreds of
     BaremetalHosts; for 1000+ hosts, Ironic tuning (conductor
     workers, database connection pooling) is recommended.
     Limits determined through adopter production experience.
- Describe which resilience pattern the project uses and how,
  including the circuit breaker pattern.
   - Uses the Kubernetes controller-runtime pattern with work
     queues, exponential backoff, rate limiting, and leader
     election. The BaremetalHost state machine is idempotent —
     controllers reconcile from observed state and recover
     automatically from transient failures.

### Observability Requirements

- Describe the signals the project is using or producing,
  including logs, metrics, profiles and traces. Please include
  supported formats, recommended configurations and data
  storage.
   - Produces Prometheus metrics (reconciliation duration, error
     counts, queue depth) via controller-runtime's built-in
     endpoint on `:8443/metrics`. Controller logs are emitted
     through controller-runtime logging (klog/logr, with zap in
     some components/configurations). Ironic services emit their
     own service logs. Distributed tracing is not enabled by
     default.
- Describe how the project captures audit logging.
   - Relies on Kubernetes audit logging for API operations. The
     controller logs all reconciliation actions. BMC credential
     access is logged but credentials are redacted from all log
     output.
- Describe any dashboards the project uses or implements as well
  as any dashboard requirements.
   - Metal3 does not ship built-in dashboards. Its Prometheus
     metrics can be scraped and visualized with tooling chosen
     by adopters (for example Grafana, Headlamp, or other
     Kubernetes observability UIs) to track BareMetalHost state
     distribution, provisioning latency, and error rates.
- Describe how the project surfaces project resource requirements
  for adopters to monitor cloud and infrastructure costs, e.g.
  FinOps
   - BaremetalHost status exposes hardware inventory (CPU, RAM,
     disk, NIC details) discovered during inspection, enabling
     adopters to build asset tracking and capacity planning on
     top of the CRD data.
- Which parameters is the project covering to ensure the health
  of the application/service and its workloads?
   - Health is tracked across BMH, CAPM3, and IPAM resources:
     BaremetalHost `.status.conditions` and
     `.status.operationalStatus`, Metal3Machine/Metal3Cluster
     readiness and error conditions, and IPPool/IPClaim/
     IPAddress allocation state. Controller health is exposed
     via `/healthz` and `/readyz` endpoints, and monitored via liveness and
     readiness probes.
- How can an operator determine if the project is in use by
  workloads?
   - Look for active BaremetalHost, Metal3Machine,
     Metal3Cluster, IPPool, IPClaim, and IPAddress resources
     in the cluster. Controller metrics showing non-zero
     reconciliation counts confirm active operation.
- How can someone using this project know that it is working for
  their instance?
   - BaremetalHost resources should show expected state
     transitions (registering → inspecting → available),
     Metal3Machine/Metal3Cluster resources should converge to
     ready states, IPAM objects should show successful claim and
     allocation, reconciliation metrics should be healthy, and
     no persistent errors should appear in controller logs.
- Describe the SLOs (Service Level Objectives) for this project.
   - Not formally defined by the project. Adopters define SLOs
     based on their fleet size and operational requirements.
- What are the SLIs (Service Level Indicators) an operator can
  use to determine the health of the service?
   - Reconciliation success rate, provisioning success rate,
     BaremetalHost time-to-available, Metal3Machine/Cluster
     reconciliation latency, IP claim allocation latency,
     controller restart count, Ironic API response time — all
     available through
     Prometheus metrics.

### Dependencies

- Describe the specific running services the project depends on
  in the cluster.
   - Kubernetes API server, Ironic (with its httpd, dnsmasq,
     and MariaDB/SQLite components), cert-manager by default
     for certificate management, and optionally DHCP/TFTP
     infrastructure for PXE boot. Operators can bring their own
     certificates, but cert-manager is the default deployment
     path. All dependencies except Kubernetes can be deployed
     as containers within the management cluster.
- Describe the project's dependency lifecycle policy.
   - Dependencies get updated regularly via Dependabot. Metal3
     tracks Cluster API and Kubernetes release cycles, dropping
     support for EOL upstream versions, maintaining two
     supported release lines while also running N-3
     upgrade-path CI coverage for one additional prior line.
- How does the project incorporate and consider source
  composition analysis as part of its development and security
  hygiene? Describe how this source composition analysis (SCA)
  is tracked.
   - Uses Dependabot, GitHub security advisories, and container
     image scanning. Every release includes an SBOM. `go.sum`
     provides cryptographic verification of all dependency
     versions.
- Describe how the project implements changes based on source
  composition analysis (SCA) and the timescale.
   - Critical vulnerabilities get fixed within days in patch
     releases. Other SCA findings are addressed in the next
     regular release cycle on a timeline proportional to
     severity.

### Troubleshooting

- How does this project recover if a key component or feature
  becomes unavailable? e.g Kubernetes API server, etcd,
  database, leader node, etc.
   - Controllers use exponential backoff and requeue — if Ironic
     or the Kubernetes API is temporarily unavailable,
     reconciliation retries automatically. Bare metal hosts
     continue running unaffected; only lifecycle operations are
     paused until connectivity is restored.
- Describe the known failure modes.
   - BMC unreachable (network or credential issues) — surfaces
     as BaremetalHost error condition
   - Firmware bugs causing inspection/provisioning failures —
     documented workarounds per vendor
   - Ironic database corruption — requires database restore
     from backup
   - DHCP/PXE network misconfiguration — host fails to boot IPA
   - Race conditions during concurrent provisioning of large
     batches — mitigated by work queue rate limiting

All failure modes surface as BaremetalHost error conditions
   with descriptive messages and are documented in the
   [troubleshooting guide][troubleshooting].

### Compliance

- What steps does the project take to ensure that all
  third-party code and components have correct and complete
  attribution and license notices?
   - Maintains an Apache 2.0 LICENSE in every repository.
     All third-party Go dependencies are pinned in
     `go.mod`/`go.sum`. There is currently no single automated,
     org-wide dependency license-attribution verification step;
     maintainers review dependency licenses when introducing
     or updating dependencies.
     Container images are built from tracked, versioned base
     images.
- Describe how the project ensures alignment with CNCF
  recommendations for attribution notices.
   - Follows CNCF guidance by providing a project LICENSE,
     and SBOMs attached to released container images. See the
     [LICENSE file](https://github.com/metal3-io/baremetal-operator/blob/main/LICENSE).
   - How are notices managed for third-party code incorporated
     directly into the project's source files?
      - Apache 2.0 headers go in source files. Vendored code
        keeps its original licenses.
   - How are notices retained for unmodified third-party
     components included within the project's repository?
      - The project does not vendor third-party source code;
        dependencies are consumed via Go modules, so no
        aggregated NOTICE file is needed.
   - How are notices for all dependencies obtained at build time
     included in the project's distributed build artifacts
     (e.g. compiled binaries, container images)?
      - SBOM attestation is attached to container images via
        cosign.

### Security (Day 2)

- Security Hygiene
   - How is the project executing access control?
      - Each controller (BMO, CAPM3, IPAM) runs with its own
        RBAC-scoped ServiceAccount with least-privilege
        permissions. BMC credentials are stored exclusively in
        Kubernetes Secrets and are never exposed in CRD status,
        logs, or events.  All user access is through the Kubernetes API,
        relying on RBAC for access control.
- Cloud Native Threat Modeling
   - How does the project ensure its security reporting and
     response team is representative of its community diversity
     (organizational and individual)?
      - The Metal3 [security team][sec-policy] includes members
        from multiple organizations (Red Hat, Ericsson). The
        security lead role rotates via a nomination process
        documented in [CONTRIBUTOR-ROLE.md][roles].
   - How does the project invite and rotate security reporting
     team members?
      - Nomination by a security team member plus Org Admin
        approval, with onboarding via shadowing. The process is
        documented in [CONTRIBUTOR-ROLE.md][roles]. Vulnerability
        reports are handled per the published
        [security policy][sec-policy] with a 7-day initial
        response SLA.

---

## References

- [Metal3 website](https://metal3.io/)
- [Metal3 documentation (book)][book]
- [Metal3 architecture][arch]
- [Metal3 security policy][sec-policy]
- [Metal3 quick start][quickstart]
- [Metal3 troubleshooting][troubleshooting]
- [Metal3 community repo](https://github.com/metal3-io/community)
- [Metal3 roadmap board][roadmap-board]
- [Metal3 design documents][design-docs]
- [Metal3 adopters][adopters]
- [Metal3 incubation DD][incubation]
- [Security self-assessment][self-assessment]
- [OpenSSF Best Practices badge][ossf]
- [LFX Insights](https://insights.linuxfoundation.org/project/metal3/contributors)
- [BMO releases](https://github.com/metal3-io/baremetal-operator/releases)
- [CAPM3 releases](https://github.com/metal3-io/cluster-api-provider-metal3/releases)
- [IPAM releases](https://github.com/metal3-io/ip-address-manager/releases)
- [metal3-dev-env][dev-env]
- [Ironic Standalone Operator][irso]

<!-- Reference links -->

[roadmap-board]: https://github.com/orgs/metal3-io/projects/8
[design-docs]: https://github.com/metal3-io/metal3-docs/tree/main/design
[roadmap]: https://github.com/metal3-io/community/blob/main/ROADMAP.md
[incubation]: https://github.com/cncf/toc/blob/main/projects/metal3-io/2025-metal3-io-incubation.md
[meetup-notes]: https://docs.google.com/document/d/1Exm5k6JB2JlD9_FOaM68R2ASHi7ZsryF_xEqLG1j4zA
[dev-env]: https://github.com/metal3-io/metal3-dev-env
[capi]: https://cluster-api.sigs.k8s.io/
[arch]: https://book.metal3.io/architecture
[book]: https://book.metal3.io/
[quickstart]: https://book.metal3.io/quick-start
[troubleshooting]: https://book.metal3.io/troubleshooting
[sec-policy]: https://book.metal3.io/security_policy
[self-assessment]: https://github.com/metal3-io/metal3-docs/pull/456
[ossf]: https://www.bestpractices.dev/en/projects/9160
[sscp]: https://project.linuxfoundation.org/hubfs/CNCF_SSCP_v1.pdf
[irso]: https://github.com/metal3-io/ironic-standalone-operator
[roles]: https://github.com/metal3-io/community/blob/main/CONTRIBUTOR-ROLE.md
[adopters]: https://github.com/metal3-io/community/blob/main/ADOPTERS.md
