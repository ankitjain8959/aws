# Virtualization
`Machine`: It is simply a physical computer (laptop, desktop, etc.).

> Machine = Hardware + Operating System + Software

- Hardware such as CPU, RAM, Storage, and Network components form the physical part of a machine.
- Operating System manages the hardware resources and provides services for application software.
- Software are programs/applications that perform specific tasks for users installed on the OS.

`Server`: A machine with a job (job to provide service to other machines or clients over a network)
```
  Web server → serves websites  
  Database server → stores & serves data  
  Email server → serves email  
```
`Physical server`: A physical server is a dedicated machine that runs server software to provide services to other machines over a network. Physical servers are typically housed in data centers.

`Data Center`: Data center is a physical facility that stores thousands of servers. Data center provides power, cooling, internet connectivity, and physical security to keep the servers running 24/7.

`Virtualization`: It means running fake/virtual computers on one physical/real computer. Virtual means "not real" but "simulated by software".

`Virtual machines`: These fake computers are called Virtual Machines (VMs). A VM is a software computer that behaves like a real one. Each VM runs its own operating system and applications, just like a physical computer. Multiple VMs can run on a single physical machine, sharing its resources (CPU, memory, storage). All VMs share the same hardware of the host machine.  
**Example 1:** 1 physical server running 3 VMs. VM1 runs web server, VM2 runs database server, VM3 runs email server.  
**Example 2:** 1 physical laptop (that runs with Windows OS) running 2 VMs: one VM with Linux OS and another VM with MacOS.

`Hypervisor/Virtual Machine Monitor (VMM)`: It is a software that creates and manages VMs. It sits between the physical hardware and the VMs, allocating resources like CPU, memory, and storage to each VM as needed.  
![Hypervisor](images/Hypervisor.png)

**Types of Hypervisors:**
1. **Type 1 (Bare-metal hypervisor):** Runs directly on the physical hardware. This type of hypervisor has OS built into it. Type 1 hypervisors are more efficient and provide better performance because they have direct access to the hardware. Examples: Microsoft Hyper-V, KVM, VMware ESXi.    
   Note: Cloud providers like AWS, Azure, GCP use Type 1 hypervisors to run VMs on their physical servers in data centers.
2. **Type 2 (Hosted hypervisor):** Runs on top of a host operating system. Examples: VMware Workstation, Oracle VirtualBox.

![Hypervisor-Types](images/Hypervisor-Types.png)

`Bare metal server`: A physical server without any virtualization layer is called a bare metal server. Applications run directly on the physical hardware without the overhead of a hypervisor.


## Advantages of Virtualization
**Problems before virtualization:**  
- Underutilization of resources: Most physical servers were not fully utilized, leading to wasted resources because each physical server ran a single application or service.
- High costs: Purchasing and maintaining multiple physical servers was expensive.
- Limited flexibility: Deploying new applications or services required setting up new physical servers, which was time-consuming.

**Benefits of virtualization:**  
- Better resource utilization: Multiple virtual machines can run on a single physical server, maximizing resource usage.
- Cost savings: Reduces the need for physical hardware, lowering costs for businesses.
- Increased flexibility: New virtual machines can be created quickly, allowing for faster deployment of applications and services.


# Cloud Computing
1. Cloud computing is the delivery of computing services — including servers, storage, databases, software, and intelligence — over the Internet (“the cloud”), instead of owning & maintaining their own physical infrastructure or data centers.

2. The `Cloud` refers to servers that are `accessed over the Internet`, and accessing the software and databases that run on those servers.  
   Cloud servers are located in data centers all over the world.

3. Cloud means you rent virtual resources from someone else (i.e. cloud providers) who owns data centers full of servers, and you access those resources via the internet.  
   Cloud providers own and maintain the physical infrastructure, including data centers and servers.
   Examples of cloud providers are Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP).

**For example:**   
**Old way:** If you wanted to host a Spring boot app, you had to buy a physical server, install OS, set up networking, and maintain the hardware. This process was time-consuming and costly.  
**Cloud way:** With cloud computing, you can quickly rent a virtual server from a cloud provider, deploy your Spring boot app, and scale resources as needed, all without the hassle of managing physical hardware. This process is much faster and more cost-effective.


## Types of Cloud Computing services or models
These are defined by how much control you have versus how much the cloud provider or vendor manages for you.    
Control: IaaS > PaaS > SaaS  

1. `Infrastructure as a Service (IaaS)`: Offers the highest level of control, allowing users to manage operating systems, apps, and middleware, while the vendor manages the physical data center, servers, and networking.
   **What you manage:** OS, Middleware, Apps, Data  
   **What provider manages:** Virtualization, Servers, Storage, Networking  
   **Target users:** IT administrators, DevOps engineers      
   **Example:** Amazon EC2, Google Compute Engine

2. `Platform as a Service (PaaS)`: Provides a platform allowing customers to develop, run, and manage applications without dealing with the underlying infrastructure.
   What you manage: Apps, Data  
   What provider manages: Middleware, OS, Virtualization, Servers, Storage, Networking  
   Target users: Software Developers  
   Example: AWS Elastic Beanstalk, Google App Engine

3. `Software as a Service (SaaS)`: A fully managed application delivered via a web browser, ideal for end-users. The vendor handles all maintenance, upgrades, and security.  
   **What you manage:** Nothing (fully managed by vendor)    
   **What provider manages:** Apps, Data, Middleware, OS, Virtualization, Servers, Storage, Networking    
   **Target users:** End-users  
   **Example:** Google Workspace (Gmail, Google Docs), Microsoft Office 365


## Types of Cloud Deployment
1. `Public Cloud`: A shared cloud environment where services are delivered over the public internet and shared across multiple organizations.  
Public cloud is multi-tenant (meaning, multiple organizations/customers share the same infrastructure).   
Examples: AWS, Microsoft Azure, and Google Cloud Platform.

2. `Private Cloud`: A cloud environment dedicated to a single organization, either hosted on-premises or by a third-party provider. It offers greater control, privacy, and security. 
Private cloud is single-tenant (meaning, exclusively offered to one organization/customer).  

3. `Hybrid Cloud`: A combination of public and private clouds, allowing data and applications to move between them for greater flexibility and optimization.

# Summary
| Term              | Simple definition                                  |
|-------------------|----------------------------------------------------|
| Machine           | Physical computer                                  |
| Server            | A machine that provides services                   |
| Data Center       | Huge warehouse full of servers                     |
| Virtualization    | Running virtual computers on one physical computer |
| Hypervisor        | Software that creates and manages VMs              |
| Virtual Machine   | Software-based computer                            |
| Bare Metal Server | Physical server without virtualization layer       |
| Cloud             | Renting virtual infrastructure over internet       |


# VPC: Virtual Private Cloud
1. A virtual private cloud (VPC) is a private cloud hosted within a public cloud; no one else shares the VPC except a single organization/customer.  
2. A VPC is a secure, isolated, private network hosted within a public cloud provider’s infrastructure (e.g., AWS, Google Cloud, Azure).  
3. VPC allows organizations to define their own IP address ranges, subnets, route tables, and network gateways, combining the scalability of public cloud with the security of a private network.  