---
layout: default
---

![](http://sonata-nfv.eu/sites/sonata-nfv.eu/themes/zen/sonatina/images/sonata_logo.svg)

# Foreword

## Purpose of this tutorial

The purpose of this tutorial is to provide the reader with a general view about SONATA in the easiest and quickest way possible.
Here you will find a brief explanation about what SONATA is, how to install it, how to use it and how to get technical support if required.
This tutorial doesn´t pretend to be an extend document where to find all the information related to SONATA, but an "umbrella" document that will guide you to more extensive documentation if required.

## Document structure

This quick guide is organized in the following manner:

* Section 1 (this section) is an introduction to the guide.
* Section 2 provides general presentation about SONATA: what SONATA is and for whom, its general architecture, and a brief description of its main modules, the Service Platform (SP) and the Service Development Kit(SDK).
* Section 3 gives information about the current SONATA software release.
* Section 4 explains the installation process for each of the SONATA components that can be installed and used individually. These are the SP, the SDK and the Emulator that, although part of the SDK, has its own autonomy.
* In section 5, we describe briefly how to use SONATA once installed, from the creation of a service, how to test it using the emulator, to its deployment with the Service Platform . 
* Section 6 describes the support process provide by SONATA Team.
* Section 7 is a consolidation of all the acronyms used in this document.

# Brief SONATA introduction

SONATA is an agile service development and orchestration framework for 5G virtualized networks. It provides a programming model and a development tool chain for virtualized services, fully integrated with a DevOps-enabled service platform and orchestration system. SONATA therefore targets both the flexible programmability of software networks as well as the optimization of their deployments. Its modular design makes the SONATA service platform effortlessly customizable to meet the needs of different service providers.

*** What is SONATA ? ***

SONATA provides a platform for supporting the lifecycle of virtualized networking services. In particular, network function chaining and orchestration are the target domains of SONATA.

*** Who is it for ? ***

The SONATA platform supports two main stakeholders in telecommunications service creation: service developers and service operators. SONATA's Network Service Development Kit (SDK) facilitates network service development for service developers. Such services are then deployed and run on SONATA's Service Platform. Through its extensive customization capabilities, the service platform allows communication service providers to adapt service provisioning to their specific environment and needs. SONATA enables a DevOps workflow between the SDK tools and the service platform, which allows developers and operators to closely collaborate in providing an outstanding experience to customers.

## General architecture

The main architectural components of the SONATA platform are shown in the figure below. In line with the support of service developers and service operators, SONATA distinguishes the two main components SDK and service platform. Services developed and deployed by this system run on top of the underlying infrastructure accessible to the SONATA system via Virtual Infrastructure Managers (VIMs), abstracting from the actual hardware and software.

![](http://sonata-nfv.github.io/son-tutorials/figures/Sonatamaincomponents.png)

Each of the two main components can be divided into a number of subcomponents, realized in a micro-service-based approach. Detailed information on the architecture and its components can be found in [http://sonata-nfv.eu/sites/default/files/sonata/public/content-files/pages/SONATA_D2.2_Architecture_and_Design.pdf deliverable D2.2].

## SONATA 3.0 modules

The SONATA platform consists of a number of software modules which together provide the required functionality for network service creation, deployment, and management. Most modules can be attributed directly to either the service platform or to the service development (SDK), according to their use. Some modules are cross-cutting and are used in both major components.

### Service Platform

SONATA's Service Platform (SP) is where:

* users are created, authenticated and authorized;
* packages, containing (network) services and (virtual network) functions descriptions, are on-boarded, validated and stored in the catalogue. A service or a function can bring with it a specific manager, which may change the default behaviour the SP has for specific aspect of that service's or function's lifecycle (e.g., placement, scaling, etc.);
* services from the catalogue are instantiated (with licences verified) and orchestrated, through the MANO, in the abstracted infrastructure;
* instantiation records are generated and stored, providing instantiation data to the other components;
* monitoring data is collected and securely provided on demand to the service developer, thus allowing quick and frequent service improvements;
* KPIs are collected, to show the overall business performance of the system.

To support all these features, we have designed a modular and very flexible architecture, shown below.

![](http://sonata-nfv.github.io/son-tutorials/figures/Implementation_architecture.png)

The main modules of the SP are the following:

* ***[Gatekeeper](https://github.com/sonata-nfv/son-gkeeper):*** controls and enforces whoever (and whatever) wants to interact with the SP and guarantees the quality of the submitted packages, by validating them against a schema (syntactically), the topology of the described service and it's integrity;
* ***[Catalogues](https://github.com/sonata-nfv/son-catalogue):*** stores and manages (smart delete: only packages with services that do not have running instances can be deleted) package files, it's meta-data as well as service's and functions' meta-data. The catalogues use the SONATA schema for their metadata as defined in [SONATA schema]([https://github.com/sonata-nfv/son-schema);
* ***[MANO Framework](https://github.com/sonata-nfv/son-mano-framework):*** the orchestrator, who manages each service's lifecycle, including when the service and/or its functions bring specific managers with them to be used in certain segments of their lifecycle. Please note the clear separation between the two levels, the ***Network Function Virtualization Orchestrator (NFVO)*** and the ***Virtual Network Function Manager(VNFM)*** and Controller. This separation was originally recommended by ETSI, and it effectively corresponds to two very different levels of abstraction that is important to be kept separate;
* ***[Infrastructure Abstraction](https://github.com/sonata-nfv/son-sp-infrabstract):*** hides the complexity and diversity of having to deal with multiple VIMs and WIMs;
* ***[Repositories](https://github.com/sonata-nfv/son-catalogue-repos):*** stores and manages service and function records, resulting from the instantiation, update and termination processes;
* ***[Monitoring](https://github.com/sonata-nfv/son-catalogue-repos):*** collects (via [monitoring probes](https://github.com/sonata-nfv/son-monitor-probe)), stores and provides monitoring data for the services and functions instances.

These are the highest level modules. Further details on each one of them can be found in each one of those modules GitHub's repositories.

### SDK

The service development kit (SDK) consists of the following main modules:

* ***[CLI](https://github.com/sonata-nfv/son-cli)***: SONATA SDK's command line interface tools to aid in developing network services and VNFs
* ***[Editor](https://github.com/sonata-nfv/son-editor)***: SONATA's web-based editor for service and function descriptors. The [Editor frontend](https://github.com/sonata-nfv/son-editor-frontend) and [Editor backend](https://github.com/sonata-nfv/son-editor-backend) are the frontend and backend of SONATA's web-based service and function descriptor editor.
* ***[Emulator](https://github.com/sonata-nfv/son-emu)***: emulation platform to support network service developers in locally prototyping and testing complete network service chains in realistic end-to-end multi-PoP scenarios
* ***[Analyzer](https://github.com/sonata-nfv/son-analyze)***: analysis framework to study a service's behaviour

## About the latest minor release v3.1

Release v3.1 of the SONATA platform is mainly a bug fixing update. Still, a number of enhancements made it into this version. Among them are the following:

* Enhancement of email notification mechanism in monitoring system
* Additional information exposure by infrastructure adapter
* Increased flexibility of the SSM/FSM mechanisms
* Example of placement SSM
* Security enhancements
* Upgrade of underlying libraries to eliminate security vulnerabilities

## About the latest major release v3.0

This is version 3.0 of the SONATA platform, the fifth release of the code. The main enhancements since the last release, v2.1, are:

This is version 3.0 of the SONATA platform, the fifth release of the code. The main enhancements since the last release, v2.1, are:

* SP:
  * improved customizability/programmability of the MANO framework
  * improved user, role and group management
  * improved access control
  * added package validation and signature verification
  * added package, service and function ownership (to connect with licence management)
  * added location-awareness for placement decisions in service instantiation
  * added KPIs to most operations
  * added synch (through a Web Socket) and asynch (through JSON) monitoring data requests
  * added service instance termination
  * added rate limit control to all operations
  * support for service licence management
* SDK:
  * Integrated authentication support in son-access (part of son-cli). This allows to authenticate users before they connect to the/a service platform
  * Support for package signatures in son-package (part of son-cli). This enables a/the service platform to validate that service/function packages are actually originating from trusted developers
  * Graphical User Interface (son-editor) for the creation of network services and associated service and network function descriptors.
  * Improved integration with GitHub. This enables easy reuse of existing service and function descriptors directly from GitHub repositories.
  * Inclusion of advanced service validation functionality including graphical debugging tool (son-validate).
  * The emulator (son-emu) now supports OpenStack-like API endpoints to allow carrier-grade MANO stacks (SONATA, OSM) to control the emulated VIMs
  * The emulator (son-emu) has added a prototype implementation of OpenStack Neutron's SFC chaining API to the emulator
  * The emulator (son-emu) is now compatible with 802.1Q Standard 5.3.1 in using the fixed VLAN-tag range for internal chaining system
  * The emulator (son-emu) now supports experimenting with different placement topologies as well as evaluating different scaling options.
  * The profiling tool now supports two profiling modes: i) active mode in order to generate a range of service and resource configurations which can be deployed and for which metrics can be collected, and ii) passive mode which enables to gather metrics directly in the emulator
  * The monitoring component (son-monitor) enables to receive streamed monitoring data from the SONATA service platform through Websockets
  * The monitoring component (son-monitor) supports host overload detection functionality from VNFs.
  * The monitoring component (son-monitor) supports external service access points for traffic generation and reception.
  * Reusable Service and Function Specific Management templates have been added for monitoring, scaling and placement. These can be easily adapted by developers in order to implement custom logic.
* General ones:
  * improvements for multi-PoP environments
  * improved installation process
  * many additional features, bug fixes, performance improvements

# [Component Installation](/component_installation)

# [Administration and User guide](/start_using)
