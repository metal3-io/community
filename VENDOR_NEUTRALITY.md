# Metal3 vendor neutrality declaration

As a Cloud Native Computing Foundation project, Metal3 will keep its
[vendor neutrality](https://contribute.cncf.io/maintainers/community/vendor-neutrality/)

**Hardware vendor neutrality:**
As it is stated in the [documentation](https://book.metal3.io/) Metal3 is a
CNCF project with the goal to provide bare metal life cycle management
functionality regardless of the vendor of the hardware. The only requirements
are that the machines are equipped with a [BMC](https://www.servethehome.com/explaining-the-baseboard-management-controller-or-bmc-in-servers/)
and support either [Redfish](https://redfish.dmtf.org/) or [IPMI](https://en.wikipedia.org/wiki/Intelligent_Platform_Management_Interface)
protocols. Metal3 focuses industry-wide vendor neutral BMC management protocols
`Redfish` and `IPMI` to conduct out of band bare metal life cycle management
activities. Metal3 also supports some vendor specific protocols in addition to
the vendor neutral protocols in order to provide full hardware compatibility
equal to the scope of the vendor neutral protocols across the widest range of
vendors possible.

**Feature vendor neutrality:**
Metal3 features are developed with the specific aim to manage the life cycle
of bare metal machines and  K8s clusters on top of those machines thus the
Metal3  stack by nature only supports so called "self-hosted" workflows.
None of the Metal3's features require any sort of commercial infrastructure
provider service (e.g. AWS, Azure etc...) to be usable. In regards to feature
support for specific hardware it has to be mentioned that all of the
functionality provided by Metal3 is hardware vendor agnostic.

**Developement vendor neutrality:**
The Metal3 community has decided to make its own CI/CD/Q&A utilize
CNCF resources as much as possible in order to not rely on any external
organization to host the project's development infrastructure. There are
certain development and Q&A use cases that still require computational
resources outside of the scope of the CNCF but those resources are provided by
the maintainer organizations. The long term goal is to minimize
and if possible eliminate such use cases.
